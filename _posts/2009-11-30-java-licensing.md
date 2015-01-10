---
layout: post
header-img: img/default-blog-pic.jpg
author: nsharma
description: 
post_id: 2198
created: 2009/11/30 12:21:06
created_gmt: 2009/11/30 07:21:06
comment_status: open
---

# Java Licensing

In my current project, we had to provide a licencing solution to our client. This made me look into the True Licencing API(url).

Before we delve into the details of using the API lets have a brief look at the various concepts associated with licencing.

Private Key:

Public Key:

Certificate:

For my current project, we had to provide a licencing solution to our client.This made me look into the[ True Licensing API(TLC)][1].

To understand the concept of licensing, it is imperative to know the concept of digital signing using a private key at the sender's end and recieving at the receiver's end using a public key. The sender generally sends this public key certificate. A number of intensive tutorials can be found explaining these concepts in detail. One of them by sun can be found [here][2].

**Use Story**: I have a jar file which needs to be protected by a license.  Also I have to generate licenses for authentic users so that they can use my jar file.

**Solution** : As the owner of the JAR file I need to have a keystore in place and then sign my JAR with a private key in my keystore. Using this keystore I have to generate licenses which would be then given to authentic users. Now lets look at each step one by one.

_**1**_**: Generate KeyStore using  java keytool-genkey command**

[java] keytool -genkey -alias <aliasForYourPrivateKey> -keypass <privateKeyPwd> -keystore <keystoreName> -storepass <keystorePwd> [/java]

Keeping the above syntax in mind I create the keystore with the following details :

[java] keytool -genkey -alias signedJar -keypass nancy456 -keystore nancyStore -storepass nancy123 [/java]

Here, I am creating a keystore with the following details:  ● keystore name : nancyStore  ● password : nancy123

A keystore file "nancyStore" would be created at your system's home. In this keystore I am creating a key(which is my private key) with an alias of signedJar. This key is further protected with a password nancy456. When you run this command on the command line, it prompts you for some details. The screen output is shown here: ![keytoolOutput][3] When using the TLC you need to provide two Java key stores:             ● One to be distributed with your application, containing your public key.             ● The other, your personal and private key store, containing your private key. The file holding the license key is signed with your private key, then encrypted and distributed to your application users. What we have created here is the private key store. Your public key, in the distributed key store, is used to test the validity of the license key in the file once it has been decrypted. You should never distribute the key store containing your private key. TLC enforces this by throwing an exception if a private key is found in the key store entry when verifying a license key.

[java] keytool -export -alias signedJar -file certfile.cer -keystore nancyStore [/java]

Here we export the public key from the private key with an alias "signedJar" in nancyStore to a file called "certfile.cert". This certificate file is used for transporting the public key to the keystore that will be distributed with the application.

[java] keytool -import -alias publiccert -file certfile.cer -keystore publicCerts.store [/java]

publicCerts.store is your public keystore which should be distributed with the application.

**2\. Sign your JAR file : java jarsigner tool**

[java]jarsigner -keystore nancystore -signedjar sCount.jar Count.jar signedJar[/java]

This would prompt you for the store password(nancy123) and private key password(nancy456). The jarsigner tool extracts the certificate from the keystore entry with an alias signerJar and attaches it to the generated signature of the signed JAR file.

If you look into the Manifest file in sCount.jar you would see an entry of the digital signature.

[java] Manifest-Version: 1.0 Created-By: 1.5.0_10 (Sun Microsystems Inc.)

Name: Count.class SHA1-Digest: /x1MdqhZ+NJgZXjiGQrufnleE4k= [/java]

Now I have my keystore in place and my jar file is locked. Im left with generating licenses for my users so that they can unlock the jar file and use it.

**3\. Generating Licenses - Using the API** _Required Jars_: truelicense.jar, truexml.jar and commons-codec.jar

While going through the [javadoc][4] for this API you come across a class called LicenseManager. Important method to look here is:

[java] public final void store(LicenseContent content, File keyfile) { throws Exception } [/java]

LicenseContent is an interface needed to build the content of the license. Looking at the constructor for this manager class we need to provide a LicenceParam object. This is again an interface to configure parameters for the license. With the help of the javadoc we start coding to these interfaces. First in the tasks is to prepare the LicenseParam object. The code looks like this:

[java] import java.io.FileInputStream; import java.io.FileNotFoundException; import java.io.IOException; import java.io.InputStream; import de.schlichtherle.license.KeyStoreParam;

public class KeyStoreParamImpl implements KeyStoreParam { public InputStream getStream() throws IOException { private static final String KEY_STORE_LOC = "C:/Users/nsharma/privateKeys.store"; FileInputStream fis = new FileInputStream(KEY_STORE_LOC); if (fis == null) throw new FileNotFoundException(KEY_STORE_LOC); return fis; } public String getAlias() { return "signedJar"; } public String getKeyPwd() { return "nancy456"; } public String getStorePwd() { return "nancy123"; } } [/java]

Now KeystoreParam and CipherParam are again interfaces required to build this object. So we move on to building these interfaces:

[java] import de.schlichtherle.license.CipherParam;

public class CipherParamImpl implements CipherParam { public String getKeyPwd() { return "passwor987"; } } [/java]

[java] import java.util.prefs.Preferences; import de.schlichtherle.license.CipherParam; import de.schlichtherle.license.KeyStoreParam; import de.schlichtherle.license.LicenseParam;

public class LicenseParamImpl implements LicenseParam {
    
    
    public CipherParam getCipherParam() {
        return new CipherParamImpl();
    }
    
    public KeyStoreParam getKeyStoreParam() {
        return new KeyStoreParamImpl();
    }
    
    public Preferences getPreferences() {
        return Preferences.userNodeForPackage(GenerateLicense.class);
    }
    
    public String getSubject() {
        return &quot; counting application&quot;;// The artifact being licensed.Also used in LicenseContentInfo
    }
    

} [/java]

Now let's move on to building our licence content.

[java] import java.util.Calendar; import java.util.Date; import javax.security.auth.x500.X500Principal; import de.schlichtherle.license.LicenseContent;

public class LicenseContentInfo { public de.schlichtherle.license.LicenseContent createLicenseContent() {

   [1]: https://truelicense.dev.java.net/tutorial.html
   [2]: http://java.sun.com/docs/books/tutorial/security/sigcert/index.html
   [3]: http://xebee.xebia.in/wp-content/uploads/2009/11/keytoolOutput.jpg (keytoolOutput)
   [4]: https://truelicense.dev.java.net/nonav/javadoc/index.html

## Comments

**[Nancy Sharma](#5263 "2011-02-04 20:36:36"):** Thanks Fred. The second part is in progress. Should be available in a few days time.

**[fred](#5261 "2011-02-04 19:28:56"):** Thanks Nancy, this post about TrueLicense was very clear and helpful! I can't seem to find the second part though!?

**[Vova](#6426 "2011-12-21 13:12:58"):** Hi. Good aticle!!! Did you write second path?

