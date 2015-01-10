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

<div style="overflow: hidden; position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px;">In my current project, we had to provide a licencing solution to our client. This made me look into the True Licencing API(url).</div>

<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px;">Before we delve into the details of using the API lets have a brief look at the various concepts associated with licencing.</div>

<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px;">Private Key:</div>

<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px;">Public Key:</div>

<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px;">Certificate:</div>

<p>For my current project, we had to provide a licencing solution to our client.This made me look into the<a href="https://truelicense.dev.java.net/tutorial.html" target="_blank"> True Licensing API(TLC)</a>.</p>
<p>To understand the concept of licensing, it is imperative to know the concept of digital signing using a private key at the sender's end and recieving at the receiver's end using a public key. The sender generally sends this public key certificate. A number of intensive tutorials can be found explaining these concepts in detail. One of them by sun can be found <a href="http://java.sun.com/docs/books/tutorial/security/sigcert/index.html" target="_blank">here</a>.</p>
<p><strong>Use Story</strong>: I have a jar file which needs to be protected by a license.  Also I have to generate licenses for authentic users so that they can use my jar file.</p>
<p><strong>Solution</strong> : As the owner of the JAR file I need to have a keystore in place and then sign my JAR with a private key in my keystore. Using this keystore I have to generate licenses which would be then given to authentic users. Now lets look at each step one by one.</p>
<!--more-->

<p><em><strong>1</strong></em><strong>: Generate KeyStore using  java keytool-genkey command</strong></p>
<p>[java]
keytool -genkey -alias &lt;aliasForYourPrivateKey&gt; -keypass &lt;privateKeyPwd&gt; -keystore &lt;keystoreName&gt; -storepass &lt;keystorePwd&gt;
[/java]</p>
<p>Keeping the above syntax in mind I create the keystore with the following details :</p>
<p>[java]
keytool -genkey -alias signedJar -keypass nancy456 -keystore nancyStore -storepass nancy123
[/java]</p>
<p>Here, I am creating a keystore with the following details:
 ● keystore name : nancyStore
 ● password : nancy123</p>
<p>A keystore file "nancyStore" would be created at your system's home. In this keystore I am creating a key(which is my private key) with an alias of signedJar. This key is further protected with a password nancy456. When you run this command on the command line, it prompts you for some details. The screen output is shown here:
<img class="alignnone size-full wp-image-2181" title="keytoolOutput" src="http://xebee.xebia.in/wp-content/uploads/2009/11/keytoolOutput.jpg" alt="keytoolOutput" width="100%" height="70%" />
When using the TLC you need to provide two Java key stores:
            ● One to be distributed with your application, containing your public key.
            ● The other, your personal and private key store, containing your private key.
The file holding the license key is signed with your private key, then encrypted and distributed to your application users. What we have created here is the private key store. Your public key, in the distributed key store, is used to test the validity of the license key in the file once it has been decrypted.
You should never distribute the key store containing your private key. TLC enforces this by throwing an exception if a private key is found in the key store entry when verifying a license key.</p>
<p>[java]
keytool -export -alias signedJar -file certfile.cer -keystore nancyStore
[/java]</p>
<p>Here we export the public key from the private key with an alias "signedJar" in nancyStore to a file called "certfile.cert". This certificate file is used for transporting the public key to the keystore that will be distributed with the application.</p>
<p>[java]
keytool -import -alias publiccert -file certfile.cer -keystore publicCerts.store
[/java]</p>
<p>publicCerts.store is your public keystore which should be distributed with the application.</p>
<p><strong>2. Sign your JAR file : java jarsigner tool</strong></p>
<p>[java]jarsigner -keystore nancystore -signedjar sCount.jar Count.jar signedJar[/java]</p>
<p>This would prompt you for the store password(nancy123) and private key password(nancy456).
The jarsigner tool extracts the certificate from the keystore entry with an alias signerJar and attaches it to the generated signature of the signed JAR file.</p>
<p>If you look into the Manifest file in sCount.jar you would see an entry of the digital signature.</p>
<p>[java]
Manifest-Version: 1.0
Created-By: 1.5.0_10 (Sun Microsystems Inc.)</p>
<p>Name: Count.class
SHA1-Digest: /x1MdqhZ+NJgZXjiGQrufnleE4k=
[/java]</p>
<p>Now I have my keystore in place and my jar file is locked. Im left with generating licenses for my users so that they can unlock the jar file and use it.</p>
<p><strong>3. Generating Licenses - Using the API</strong>
<em>Required Jars</em>: truelicense.jar, truexml.jar and commons-codec.jar</p>
<p>While going through the <a href="https://truelicense.dev.java.net/nonav/javadoc/index.html" target="_self">javadoc</a> for this API you come across a class called LicenseManager. Important method to look here is:</p>
<p>[java]
public final void store(LicenseContent content, File keyfile) {
throws Exception
}
[/java]</p>
<p>LicenseContent is an interface needed to build the content of the license. Looking at the constructor for this manager class we need to provide a LicenceParam object. This is again an interface to configure parameters for the license. With the help of the javadoc we start coding to these interfaces. First in the tasks is to prepare the LicenseParam object. The code looks like this:</p>
<p>[java]
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import de.schlichtherle.license.KeyStoreParam;</p>
<p>public class KeyStoreParamImpl implements KeyStoreParam {
      public InputStream getStream() throws IOException {
               private static final String KEY_STORE_LOC = &quot;C:/Users/nsharma/privateKeys.store&quot;;
           FileInputStream fis = new FileInputStream(KEY_STORE_LOC);
           if (fis == null)
           throw new FileNotFoundException(KEY_STORE_LOC);
        return fis;
        }
      public String getAlias() {
                return &quot;signedJar&quot;;
        }
      public String getKeyPwd() {
            return &quot;nancy456&quot;;
        }
      public String getStorePwd() {
            return &quot;nancy123&quot;;
        }
}
[/java]</p>
<p>Now KeystoreParam and CipherParam are again interfaces required to build this object. So we move on to building these interfaces:</p>
<p>[java]
import de.schlichtherle.license.CipherParam;</p>
<p>public class CipherParamImpl implements CipherParam {
            public String getKeyPwd() {
        return &quot;passwor987&quot;;
    }
}
[/java]</p>
<p>[java]
import java.util.prefs.Preferences;
import de.schlichtherle.license.CipherParam;
import de.schlichtherle.license.KeyStoreParam;
import de.schlichtherle.license.LicenseParam;</p>
<p>public class LicenseParamImpl implements LicenseParam {</p>
<pre><code>public CipherParam getCipherParam() {
    return new CipherParamImpl();
}

public KeyStoreParam getKeyStoreParam() {
    return new KeyStoreParamImpl();
}

public Preferences getPreferences() {
    return Preferences.userNodeForPackage(GenerateLicense.class);
}

public String getSubject() {
    return &amp;quot; counting application&amp;quot;;// The artifact being licensed.Also used in LicenseContentInfo
}
</code></pre>
<p>}
[/java]</p>
<p>Now let's move on to building our licence content.</p>
<p>[java]
import java.util.Calendar;
import java.util.Date;
import javax.security.auth.x500.X500Principal;
import de.schlichtherle.license.LicenseContent;</p>
<p>public class LicenseContentInfo {
    public de.schlichtherle.license.LicenseContent createLicenseContent() {</p>

## Comments

**[Nancy Sharma](#5263 "2011-02-04 20:36:36"):** Thanks Fred. The second part is in progress. Should be available in a few days time.

**[fred](#5261 "2011-02-04 19:28:56"):** Thanks Nancy, this post about TrueLicense was very clear and helpful! I can't seem to find the second part though!?

**[Vova](#6426 "2011-12-21 13:12:58"):** Hi. Good aticle!!! Did you write second path?

