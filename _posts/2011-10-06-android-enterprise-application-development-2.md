---
layout: post
header-img: img/default-blog-pic.jpg
---

# Android Enterprise Application Development

For past two years, Android has evolved from an immature mobile OS to a popular choice for users as well as device manufacturers.  Google is continuously updating Android and adding features to make it more and more powerful and competent with other mobile OS mainly iPhone and Blackberry. One major thing which Android was lacking from its competitors was: Enterprise solutions. Now it’s no more a weakness for Android.  **_What are Enterprise applications:_** Enterprise applications enable an organization's employees to have access to important organization information/data through mobile applications anywhere, anytime. On the same time, organizations can enforce security policies on every device to control the communication, restrict the flow of data and protect the device from loss or theft. **_Why they are important:_** Mobile phones and tablet computers have had a significant impact on the way people think, communicate or work. Organizations are making enterprise applications for mobile phones to improve productivity and communication. "Anywhere and anytime access to information" can be a powerful tool for organization to enhance productivity and reduce cost. **_How other mobile platforms do it: _**In Blackberry, enterprise solutions are provided by Blackberry enterprise server setup directly. In iPhone, enterprise applications are supported by Apple to some extent. Organization can develop and distribute in-house iPhone applications. For developing and testing these applications, Apple also provides code level technical support with a fee of US $299 per year. **_How Android is doing it :_** On the release of Android OS version 2.2(froyo), Google added new API called “Android Device Administration”. By using this API, developers can create "security aware" enterprise applications through which an organization can enforce its own “settings” on their employees’ devices. These applications will be loaded with following features: **_Data Sharing: _**_Users can have access to sensitive system information and organization data._ **_Security:_** **_When organizations allow sensitive data to be accessed remotely, one thing which is most important is security. Android device administrator can enforce security policies that can be either hard-coded into the app, or the application can dynamically fetch policies from a third-party server._**

  1. **Password policies**:  Administrator can enforce a password policy. The password policy can be set using terms like minimum password length, alphanumeric password, minimum letters required, minimum lowercase letters, password expiration timeout, maximum inactivity time lock and other various conditions.
  2. **Remote lock: **The device can be immediately locked remotely by the administrator if required in case of theft or loss.
  3. **Encryption of data:** Android apps can use SSL tunnels through which data can be transported. However, the data stored on the device (phone memory and SD) is up to the applications. Prior to honeycomb (Android 3.0) there is no support for turning on encryption on the device.
If a user fails to comply with the policies (for example, if a user sets a password that violates the guidelines), it is up to the application to decide how to handle this. However, typically this will result in the user not being able to sync data. Consider a use case where a device attempts to connect to a server (which belongs to an enterprise) that requires policies which are not supported in the Device Administration API, the connection will not be allowed. **_Device control /Device management:_** **_Remote wipe: Facility for controlling administration to remotely wipe the device (to restore factory settings) in case the device is lost or stolen._**

  1. **Two way benefits**: System admin can implement the security policies and the device user can have access to the sensitive system and data and may be able to sync data.
  2. **Provisioning: mass deployment of application: **Though  Android does not have an automated provisioning solution, a system admin can distribute the application by following ways:
  * Uploading the application in Android Market.
  * Enabling non-market installation.
  * Distributing the application through email or website.
3**. Wipe the device remotely: **The administrator can wipe the device data remotely and restore the factory settings. **_To uninstall an existing device admin application, users need to first unregister the application as an administrator_**. Developers can create robust enterprise application using the Device Administration API. I am working on a small demo application using this API, will post code snippets in next blog.

## Comments

**[Rajni](#6005 "2011-10-11 11:01:49"):** Nice and useful document. Thanks for sharing it.

**[Sumeet kumar](#6004 "2011-10-10 15:03:34"):** Hi param, thats g8 discussion

**[Yogesh Kumar](#5987 "2011-10-06 21:10:05"):** Good One! Would like to know more about "Wipe the device remotely:"

**[Paramvir Singh](#6020 "2011-10-13 17:53:23"):** The remote wipe of data is the facility using which the administrator or controller can reset the device to default factory settings. This may be useful in a case where device is lost. On code level, you can do remote wipe using remote wipeData() of DevicePolicyManager class. e.g. `DevicePolicyManager policyManager; policyManager.wipeData(0);`

**[Swati](#7614 "2012-02-18 13:48:50"):** Interesting feature "Wipe the data remotely" and now want to know more about it.

**[Faisal Wahab](#8113 "2012-03-29 11:05:08"):** Is there any solution to prevent our application from uninstalling. i am developing an enterprise type application and want to control that user cannot uninstall my application... on uninstall user must have to give ID, password. ???

