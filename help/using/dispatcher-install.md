---
title: Installing Dispatcher
seo-title: Installing Dispatcher
description: null
seo-description: Learn how to install the Dispatcher module on Microsoft Internet Information Server, Apache Web Server and Sun Java Web Server/iPlanet.
uuid: 2384b907-1042-4707-b02f-fba2125618cf
contentOwner: User
converted: true
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
discoiquuid: f00ad751-6b95-4365-8500-e1e0108d9536
---

# Installing Dispatcher{#installing-dispatcher}

<!-- 

Comment Type: draft

<h2>Introduction</h2>

 -->

>[!NOTE]
>
>Dispatcher versions are independent of AEM. You may have been redirected to this page if you followed a link to the Dispatcher documentation that is embedded in the documentation for a previous version of AEM.

Use the [Dispatcher Release Notes](release-notes.md) page to obtain the latest Dispatcher installation file for your operating system and web server. Dispatcher release numbers are independent of the Adobe Experience Manager release numbers and are compatible with Adobe Experience Manager 6.x, 5.x and Adobe CQ 5.x releases.

The following file naming convention is used:

`dispatcher-<*web-server*>-<*operating-system*>-<*dispatcher-version-number*>.<*file-format*>`

For example, the dispatcher-apache2.4-linux-x86_64-ssl-4.3.1.tar.gz file contains Dispatcher version 4.3.1 for an Apache 2.4 web server that runs on Linux i686 and is packaged using the **tar** format.

The following table lists the web server identifier that is used in file names for each web server:

|||
|--- |--- |
|Web Server|Installation Kit|
|Apache 2.4|dispatcher-apache2.4-&lt;other parameters&gt;|
|Apache 2.2|dispatcher-apache2.2-&lt;other parameters&gt;|
|Microsoft Internet Information Server 7.5, 8, 8.5|dispatcher-iis-&lt;other parameters&gt;|
|Sun Java Web Server iPlanet | dispatcher-ns-&lt;other parameters&gt;|

>[!NOTE]
>
>You should install the latest version of Dispatcher that is available for your platform. On a yearly basis, you should upgrade your Dispatcher instance to use the latest version to take advantage of product improvements.

Each archive contains the following files:

* the Dispatcher modules  
* an example configuration file
* the README file that contains installation instructions and last-minute information
* the CHANGES file that lists issues fixed in current and past releases

>[!NOTE]
>
>Please check the README file for any last-minute changes / platform specific notes before starting the installation.

<!-- 

Comment Type: draft

<h3>Supported Web Servers</h3>

 -->

<!-- 

Comment Type: draft

<p>The following web servers are supported for use with Dispatcher version 4.1.12:</p>

 -->

<!-- 

Comment Type: draft

<p>The following sections detail the specific web server installation procedures.</p>

 -->

## Microsoft Internet Information Server {#microsoft-internet-information-server}

For information on how to install this web server, see the following resources:

* Microsoft's own documentation on the Internet Information Server
* ["The Official Microsoft IIS site"](http://www.iis.net/)

### Required IIS Components {#required-iis-components}

IIS versions 8.5 and 10 require that the following IIS components are installed:

* ISAPI Extensions

Also, you must add the Web Server (IIS) role. Use Server Manager to add the role and components.  

## Microsoft IIS - Installing the Dispatcher module {#microsoft-iis-installing-the-dispatcher-module}

The required archive for Microsoft Internet Information System is:

* dispatcher-iis-&lt;operating-system&gt;-&lt;*dispatcher-release-number*&gt;.zip

The ZIP file contains the following files:

|File|Description|
|--- |--- |
|disp_iis.dll|The Dispatcher dynamic link library file.|
|disp_iis.ini|Configuration file for the IIS. This example can be updated with your requirements. NOTE: The ini file must have the same name-root as the dll.|
|dispatcher.any|An example configuration file for the Dispatcher.|
|author_dispatcher.any|An example configuration file for Dispatcher working with the author instance.|
|README|Readme file that contains installation instructions and last-minute information. Note: Please check this file before starting the installation.|
|CHANGES|Changes file that lists issues fixed in current and past releases.|

Use the following procedure to copy the Dispatcher files to the correct location.

1. Use Windows Explorer to create the `<*IIS_INSTALLDIR*>/Scripts` directory,  
       for example, `C:\inetpub\Scripts`.

1. Extract the following files from the Dispatcher package into this Scripts directory:

    * `disp_iis.dll`
    * `disp_iis.ini`
    * One of the following files depending on if Dispatcher is working with an AEM author instance or publish instance:

      * Author instance: `author_dispatcher.any`
      * Publish instance: `dispatcher.any`

## Microsoft IIS - Configure the Dispatcher INI File {#microsoft-iis-configure-the-dispatcher-ini-file}

Edit the `disp_iis.ini` file to configure the Dispatcher installation. The basic format of the `.ini` file is as follows:

```xml
[main]
configpath=<path to dispatcher.any>
loglevel=1|2|3
servervariables=0|1
replaceauthorization=0|1
```

The following table describes each property.

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <th>Parameter</th> 
   <th>Description</th> 
  </tr> 
  <tr> 
   <td valign="top">configpath</td> 
   <td>The location of <span class="code">dispatcher.any</span> within the local file system (absolute path).</td> 
  </tr> 
  <tr> 
   <td>logfile</td> 
   <td>The location of the dispatcher.log file. If this is not set then log messages go to the windows event log.</td> 
  </tr> 
  <tr> 
   <td valign="top">loglevel</td> 
   <td><p>Defines the Log Level used to output messages to the event log. The following values may be specified:</p> <p>0: error messages only.<br /> 1: errors and warnings.<br /> 2: errors, warnings and informational messages.<br /> 3: errors, warnings, informational and debug messages.</p> <p><br /> Note: It is recommended to set the log level to 3 during installation and testing, then revert to 0 when running in a production environment.</p> </td> 
  </tr> 
  <tr> 
   <td>replaceauthorization</td> 
   <td><p>Specifies how authorization headers in the HTTP request are handled. The following values are valid:</p> <p>0: Authorization headers are not modified.<br /> 1: Replaces any header named "Authorization" other than "Basic" with its "Basic &lt;IIS:LOGON_USER&gt;" equivalent.</p> </td> 
  </tr> 
  <tr> 
   <td valign="top">servervariables</td> 
   <td><p>Defines how server variables are processed.</p> <p>0: IIS server variables are sent to neither the Dispatcher nor AEM.<br /> 1: all IIS server variables (such as <span class="code">LOGON_USER, QUERY_STRING,</span> ...) are sent to the Dispatcher, together with the request headers (and also to the AEM instance if not cached).<br /> <br /> Server variables include AUTH_USER, LOGON_USER, HTTPS_KEYSIZE and many others. See the IIS documentation for the full list of variables, with details.</p> </td> 
  </tr> 
  <tr> 
   <td>enable_chunked_transfer</td> 
   <td>Defines whether to enable (1) or disable (0) chunked transfer for the client response. The default value is 0.</td> 
  </tr> 
 </tbody> 
</table>

An example configuration:

```xml
[main]
configpath=C:\Inetpub\Scripts\dispatcher.any
loglevel=1
servervariables=1
replaceauthorization=0
```

### Configuring Microsoft IIS {#configuring-microsoft-iis}

Configure IIS to integrate the Dispatcher ISAPI module. In IIS you use wildcard application mapping.

### Configuring Anonymous Access - IIS 8.5 and 10 {#configuring-anonymous-access-iis-and}

The default Flush replication agent on the Author instance is configured so that it does not send security credentials with flush requests. Therefore, the website that you are using a the Dispatcher cache must allow anonymous access.

If your website uses an authentication method, the Flush replication agent must be configured accordingly.

1. Open IIS Manager and select the website that you are using as the Disptcher cache.
1. Using Features View mode, in the IIS section double click Authentication.
1. If Anonymous Authentication is not enabled, select Anonymous Authentication and in the Actions area click Enable.

### Integrating the Dispatcher ISAPI Module - IIS 8.5 and 10 {#integrating-the-dispatcher-isapi-module-iis-and}

Use the following procedure to add the Dispatcher ISAPI Module to IIS.

1. Open IIS Manager.
1. Select the website that you are using as the Dispatcher Cache.
1. Using Features View mode, in the IIS section double click Handler Mappings.
1. In the Actions panel of the Handler Mappings page, click Add Wildcard Script Map, add the following property values and then click OK:

    * Request Path: &#42;
    * Executable: The absolute path of the disp_iis.dll file, for example `C:\inetpub\Scripts\disp_iis.dll`.
    * Name: A descriptive name for the handler mapping, for example `Dispatcher`.

1. In the dialog box that appears, to add the disp_iis.dll library to the ISAPI and CGI Restrictions list, click Yes.

   For IIS 7.0 and 7.5 the configuration is complete. Continue with the remaining steps if you are configuring IIS 8.0.

1. (IIS 8.0) In the list of handler mappings, select the one that you just created, and in the Actions area click Edit.
1. (IIS 8.0) In the Edit Script Map dialog box, click the Request Restrictions button.
1. (IIS 8.0) To ensure that the handler is used for files and folders that are not yet cached, deselect Invoke Handler Only If Request Is Mapped To, and then click OK.
1. (IIS 8.0) On the Edit Script Map dialog box, click OK.

### Configuring Access to the Cache - IIS 8.5 and 10 {#configuring-access-to-the-cache-iis-and}

Provide the default App Pool user with write-acess to the folder that is being used as the Dispatcher cache.

1. Right-click the root folder of the website that you are using as the Dispatcher cache and click Properties, such as C:\inetpub\wwwroot.
1. On the Security tab, click Edit, and then on the Permissions dialog box, click Add. A dialog box opens for selecting user accounts. Click the Locations button, select your computer name, and then click OK.

   Keep this dialog box open while you complete the next step.

1. in IIS Manager, select the IIS site that you are using for the Dispatcher cache, and on the right side of the window click Advanced Settings.
1. Select the value of the Application Pool property and copy it to the clipboard.
1. Return to the open dialog box. In the Enter The Object Names To Select box, type `IIS AppPool\` and then paste the contents of your clipboard. The value should look like the following example:

   `IIS AppPool\DefaultAppPool`

1. Click the Check Names button. When Windows resolves the user account, click OK.
1. In the Permissions dialog box for the dispatcher folder, select the account that you just added, enable all of the permissions for the account** except for Full Control,** and click OK. Click OK to close the folder Properties dialog box.

### Registering the JSON Mime Type - IIS 8.5 and 10 {#registering-the-json-mime-type-iis-and}

Use the following procedure to register the JSON MIME type, when you want Dispatcher to allow JSON calls.

1. In IIS Manager, select your web site and using Features View, double-click Mime Types.
1. If the JSON extension is not in the list, in the Actions panel click Add, enter the following property values, and then click OK:

    * File Name Extension: . `json`
    * MIME Type: application/json

### Removing the bin Hidden Segment - IIS 8.5 and 10 {#removing-the-bin-hidden-segment-iis-and}

Use the following procedure to remove the `bin` hidden segment. Web sites that are not new can contain this hidden segment.

1. In IIS Manager, select your web site and using Features View, double-click Request Filtering.
1. Select the `bin` segment, click Remove, and in the confirmation dialog box click Yes.

### Logging IIS Messages to a File - IIS 8.5 and 10 {#logging-iis-messages-to-a-file-iis-and}

Use the following procedure to write Dispatcher log messages to a log file instead of to the Windows Event log. You need to configure Dispatcher to use the log file, and provide IIS with write-access to the file.

1. Use Windows Explorer to create a folder named `dispatcher` below the logs folder of the IIS installation. The path of this folder for a typical installation is `C:\inetpub\logs\dispatcher`.

1. Right-click the dispatcher folder and click Properties.
1. On the Security tab, click Edit, and then on the Permissions dialog box, click Add. A dialog box opens for selecting user accounts. Click the Locations button, select your computer name, and then click OK.

   Keep this dialog box open while you complete the next step.

1. in IIS Manager,select the IIS site that you are using for the Dispatcher cache, and on the right side of the window click Advanced Settings.
1. Select the value of the Application Pool property and copy it to the clipboard.
1. Return to the open dialog box. In the Enter The Object Names To Select box, type `IIS AppPool\` and then paste the contents of your clipboard. The value should look like the following example:

   `IIS AppPool\DefaultAppPool`

1. Click the Check Names button. When Windows resolves the user account, click OK.
1. In the Permissions dialog box for the dispatcher folder, select the account that you just added, enable all of the permissions for the account** except for Full Control,** and click OK. Click OK to close the folder Properties dialog box. 
1. Use a text editor to open the disp_iis.ini file.
1. Add a line of text similar to the following example to configure the location of the log file and then save the file:

   ```xml
   logfile=C:\inetpub\logs\dispatcher\dispatcher.log
   ```

### Next Steps {#next-steps}

Before you can start using the Dispatcher you must now:

* [Configure](dispatcher-configuration.md) Dispatcher
* [Confgure AEM](page-invalidate.md) to work with Dispatcher.

## Apache Web Server {#apache-web-server}

>[!CAUTION]
>
>Instructions for installation under both **Windows** and **Unix** are covered here. Please be careful when performing the steps.

### Installing Apache Web Server {#installing-apache-web-server}

For Information about how to install an Apache Web Server read the installation manual - either [online](http://httpd.apache.org/) or in the distribution.

>[!CAUTION]
>
>If you are creating an Apache binary by compiling the source files, make sure that you turn on **dynamic modules support**. This can be done by using any of the **--enable-shared** options. At a minimum include the** mod_so** module.
>
>More information can be found in the Apache Web Server installation manual.

Also see the Apache HTTP Server [Security Tips](http://httpd.apache.org/docs/2.2/misc/security_tips.html) and [Security Reports](http://httpd.apache.org/security_report.html).

### Apache Web Server - Add the Dispatcher Module {#apache-web-server-add-the-dispatcher-module}

The Dispatcher comes as either:

* **Windows**: a Dynamic Link Library (DLL)
* **Unix**: a Dynamic Shared Object (DSO)

The installation archive files contains the following files - dependent on whether you have selected Windows or Unix:

|File|Description|
|--- |--- |
|disp_apache&lt;x.y&gt;.dll|Windows: The Dispatcher dynamic link library file.|
|dispatcher-apache&lt;x.y&gt;-&lt;rel-nr&gt;.so|Unix: The Dispatcher shared object library file.|
|mod_dispatcher.so|Unix: An example link.|
|http.conf.disp&lt;x&gt;|An example configuration file for the Apache server.|
|dispatcher.any|An example configuration file for the Dispatcher.|
|README|Readme file that contains installation instructions and last-minute information. Note: Please check this file before starting the installation.|
|CHANGES|Changes file that lists issues fixed in the current and past releases.|

Use the following steps to add Dispatcher to your Apache Web Server:

1. Place the Dispatcher file in the appropriate Apache module directory:

   * **Windows**: Place `disp_apache<*x.y*>.dll` `<APACHE_ROOT>/modules`
   * **Unix**: Locate either the `<APACHE_ROOT>/libexec` or `<APACHE_ROOT>/modules`directory according to your installation.  
     Copy `dispatcher-apache<*options*>.so` into this directory.  
     To simplify long-term maintenance you can also create a symbolic link named `mod_dispatcher.so` to the Dispatcher:  
     `ln -s dispatcher-apache<*x*>-<*os*>-<rel-nr>.so mod_dispatcher.so`

1. Copy the dispatcher.any file to the `<*APACHE_ROOT*>/conf` directory.

   **Note:** You can place this file in a different location, as long as the DispatcherLog property of the Dispatcher module is configured accordingly. (See Dispatcher-Specific Configuration Entries below.)

### Apache Web Server - Configure SELinux Properties {#apache-web-server-configure-selinux-properties}

If you are running Dispatcher on RedHat Linux Kernel 2.6 with SELinux enabled, you might run into error messages like this in the dispatcher logfile.

`Mon Jun 30 00:03:59 2013] [E] [16561(139642697451488)] Unable to connect to backend rend01 (10.122.213.248:4502): Permission denied`

This is likely due to an enabled SELinux security. Then you you need perform the following tasks:

* Configure the SELinux context of the dispatcher module file.
* Enable HTTPD scripts and modules to make network connections.
* Configure the SELinux context of the docroot, where the cached files are stored.

Enter the following commands in a terminal window, replacing * `[path to the dispatcher.so file]`* with the path to the Dispatcher module that you installed to Apache Web Server, and *[path to the docroot]* with the path where the docroot is located (e.g. `/opt/cq/cache`):

```shell
semanage fcontext -a -t httpd_modules_t [path to the dispatcher.so file]
setsebool -P httpd_can_network_connect on
chcon -R --type httpd_sys_content_t [path to the docroot]
semanage fcontext -a -t httpd_sys_content_t "[path to the docroot](/.*)?"
```

### Apache Web Server - Configure Apache Web Server for Dispatcher {#apache-web-server-configure-apache-web-server-for-dispatcher}

The Apache Web Server needs to be configured, using `httpd.conf`. In the Dispatcher installation kit you will find an example configuration file named `httpd.conf.disp<*x*>`.

These steps are compulsory:

1. Navigate to `<*APACHE_ROOT*>/conf`.
1. Open `httpd.conf`for editing.  
1. The following configuration entries must be added, in the order listed:

   * **LoadModule** to load the module on start up.
   * Dispatcher-specific configuration entries, including **DispatcherConfig, DispatcherLog** and **DispatcherLogLevel**.
   * **SetHandler** to activate the Dispatcher. **LoadModule**.
   * **ModMimeUsePathInfo** to configure behavior of **mod_mime**.

1. (Optional) It is recommended that you change the owner of the htdocs directory:

   * The apache server starts as root, though the child processes start as daemon (for security purposes). The DocumentRoot (&lt;*APACHE_ROOT*&gt;/htdocs) must belong to the user daemon:  
    ```
    cd &lt;APACHE_ROOT&gt;  
    chown -R daemon:daemon htdocs
    ```

**LoadModule**

The following table lists examples that can be used; the exact entries are according to your specific Apache Web Server:

|||
|--- |--- |
|Windows|`... LoadModule dispatcher_module modules\disp_apache.dll ...`|
|Unix  (assuming symbolic link)|`... LoadModule dispatcher_module libexec/mod_dispatcher.so ...`|

>[!NOTE]
>
>*The first parameter of each statement must be written *exactly as in the above examples*.
>
>See the example configuration files provided and the Apache Web Server documentation for full details about this command.

**Dispatcher specific configuration entries**

The Dispatcher-specific configuration entries are placed after the LoadModule entry. The following table lists an example configuration that is applicable for both Unix and Windows:

|||
|--- |--- |
|Windows and Unix|... <br/> `<IfModule disp_apache2.c>` <br/>`DispatcherConfig conf/dispatcher.any`<br/> `DispatcherLog logs/dispatcher.log DispatcherLogLevel 3` <br/>`DispatcherNoServerHeader 0 DispatcherDeclineRoot 0`<br/> `DispatcherUseProcessedURL 0`<br/> `DispatcherPassError 0`<br/> `DispatcherKeepAliveTimeout 60`<br/> `</IfModule>` ...|


The individual configuration parameters:

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <td valign="top"><a id="dispatcherconfig" name="dispatcherconfig"></a>DispatcherConfig</td> 
   <td><p>Location and name of the Dispatcher configuration file.</p> <p>When this property is located in the main server configuration, all virtual hosts inherit the property value. However, virtual hosts can include a <span class="code">DispatcherConfig</span> property to override the main server configuration.</p> </td> 
  </tr> 
  <tr> 
   <td valign="top">DispatcherLog</td> 
   <td>Location and name of the log file.</td> 
  </tr> 
  <tr> 
   <td valign="top">DispatcherLogLevel</td> 
   <td><p>Log level for the log file:<br /> <strong>0</strong> - Errors<br /> <strong>1</strong> - Warnings<br /> <strong>2</strong> - Infos<br /> <strong>3</strong> - Debug<br /> <strong>Note</strong>: It is recommended to set the log level to 3 during installation and testing, then to 0 when running in a production environment.</p> </td> 
  </tr> 
  <tr> 
   <td valign="top">DispatcherNoServerHeader</td> 
   <td><p><strong>This parameter is deprecated and no longer has any effect.</strong></p> <p>Defines the Server Header to be used:<br /> <strong>undefined or 0</strong> - the HTTP server header contains the AEM version.<br /> <strong>1</strong> - the Apache server header is used.</p> </td> 
  </tr> 
  <tr> 
   <td valign="top">DispatcherDeclineRoot</td> 
   <td>Defines whether to decline requests to the root "/":<br /> <strong>0</strong> - accept requests to <strong>/</strong><br /> <strong>1</strong> - requests to <strong>/</strong> are not handled by the dispatcher; use mod_alias for the correct mapping.</td> 
  </tr> 
  <tr> 
   <td valign="top">DispatcherUseProcessedURL</td> 
   <td>Defines whether to use pre-processed URLs for all further processing by Dispatcher:<br /> <strong>0</strong> - use the original URL passed to the web server.<br /> <strong>1</strong> - the dispatcher uses the URL already processed by the handlers that precede the dispatcher (i.e. mod_rewrite) instead of the original URL passed to the web server.<br /> <br /> For example, either the original or the processed URL is matched with Dispatcher filters. The URL is also used as the basis for the cache file structure. <br /> <br /> See the Apache web site documentation for information about mod_rewrite; for example, <a href="http://httpd.apache.org/docs/2.2/mod/mod_rewrite.html">Apache 2.2</a>. When using mod_rewrite, it is advisable to use the flag <strong><a href="http://docs.adobe.com/content/kb/home/Dispatcher/troubleshooting/DispatcherModReWrite.html">'passthrough|PT' (pass through to next handler)</a></strong> to force the rewrite engine to set the uri field of the internal request_rec structure to the value of the filename field.<br /> </td> 
  </tr> 
  <tr> 
   <td valign="top">DispatcherPassError<br /> </td> 
   <td>Defines how to support error codes for ErrorDocument handling:<br /> <strong>0</strong> - Dispatcher spools all error responses to the client.<br /> <strong>1</strong> - Dispatcher does not spool an error response to the client (where the status code is greater or equal than 400), but passes the status code to Apache, which e.g. allows an ErrorDocument directive to process such a status code.<br /> <strong>Code Range -</strong> Specify a range of error codes for which the response is passed to Apache. Other error codes are passed to the client. For example, the following configuration passes responses for error 412 to the client, and all other errors are passed to Apache: DispatcherPassError 400-411,413-417</td> 
  </tr> 
  <tr> 
   <td>DispatcherKeepAliveTimeout</td> 
   <td>Specifies the keep-alive timeout, in seconds. Starting with Dispatcher version 4.2.0 the default keep-alive value is 60. A value of 0 disables keep-alive. </td> 
  </tr> 
  <tr> 
   <td>DispatcherNoCanonURL</td> 
   <td><p>Setting this parameter to <strong>On</strong> will pass the raw URL to the backend instead of the canonicalised one and will override the settings of <span class="code">DispatcherUseProcessedURL.</span> The default value is <strong>Off</strong>.</p> <p><strong>Note:</strong> The filter rules in the Dispatcher configuration will always be evaluated against the <strong>sanitised</strong> URL not the raw URL.</p> </td> 
  </tr> 
 </tbody> 
</table>

>[!NOTE]
>
>****Path entries are relative to the root directory of the Apache Web Server.

>[!NOTE]
>
>The default settings for the Server Header are: `  
>ServerTokens Full` `  
>DispatcherNoServerHeader 0`  
>Which shows the AEM version (for statistical purposes). If you want to disable such information being available in the header you can set: `  
>ServerTokens Prod`  
>See the [Apache Documentation about ServerTokens Directive (for example, for Apache 2.2)](http://httpd.apache.org/docs/2.2/mod/core.html) for more information.

**SetHandler**

After these entries you must add a **SetHandler** statement to the context of your configuration ( `<Directory>`, `<Location>`) for the Dispatcher to handle the incoming requests. The following example configures the Dispatcher to handle requests for the complete website:

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <td>Windows<br /> and<br /> Unix<br /> </td> 
   <td>...<br /> &lt;Directory /&gt;<br /> &lt;IfModule disp_apache2.c&gt;<br /> SetHandler dispatcher-handler<br /> &lt;/IfModule&gt;<br /> <br /> Options FollowSymLinks<br /> AllowOverride None<br /> &lt;/Directory&gt;<br /> ...</td> 
  </tr> 
 </tbody> 
</table>

The following example configures the Dispatcher to handle requests for a virtual domain:

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <td>Windows</td> 
   <td>...<br /> &lt;VirtualHost 123.45.67.89&gt;<br /> ServerName www.mycompany.com<br /> DocumentRoot <i>[cache-path]</i>\docs<br /> &lt;Directory <i>[cache-path]</i>\docs&gt;<br /> &lt;IfModule disp_apache2.c&gt;<br /> SetHandler dispatcher-handler<br /> &lt;/IfModule&gt;<br /> AllowOverride None<br /> &lt;/Directory&gt;<br /> &lt;/VirtualHost&gt;<br /> ...</td> 
  </tr> 
  <tr> 
   <td>Unix</td> 
   <td> ...<br /> &lt;VirtualHost 123.45.67.89&gt;<br /> ServerName www.mycompany.com<br /> DocumentRoot /usr/apachecache/docs<br /> &lt;Directory /usr/apachecache/docs&gt;<br /> &lt;IfModule disp_apache2.c&gt;<br /> SetHandler dispatcher-handler<br /> &lt;/IfModule&gt;<br /> AllowOverride None<br /> &lt;/Directory&gt;<br /> &lt;/VirtualHost&gt;<br /> ...</td> 
  </tr> 
 </tbody> 
</table>

>[!NOTE]
>
>****The parameter of the **SetHandler** statement must be written *exactly as in the above examples*, as this is the name of the handler defined in the module.
>
>See the example configuration files provided and the Apache Web Server documentation for full details about this command.

**ModMimeUsePathInfo**

After the **SetHandler** statement you should also add the **ModMimeUsePathInfo** definition.

>[!NOTE]
>
>The `ModMimeUsePathInfo` parameter should only be used and configured if you are using Dispatcher version 4.0.9, or higher.
>
>(Note that Dispatcher version 4.0.9 has been released in 2011. If you are using an older version, upgrading to a recent Dispatcher version would be appropriate).

The **ModMimeUsePathInfo** parameter should be set `On` for all Apache configurations:

`ModMimeUsePathInfo On`

The mod_mime module (see for example, [Apache Module mod_mime](http://httpd.apache.org/docs/2.2/mod/mod_mime.html)) is used to assign content metadata to the content selected for an HTTP response. The default setup means that when mod_mime determines the content type, only the part of the URL that maps to a file or directory will be considered.

When `On`, the `ModMimeUsePathInfo` parameter specifies that `mod_mime` is to determine the content type based on the *complete* URL; this means that virtual resources will have metainformation applied based on their extension.

The following example activates **ModMimeUsePathInfo**:

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <td>Windows<br /> and<br /> Unix<br /> </td> 
   <td><p>...<br /> &lt;Directory /&gt;<br /> &lt;IfModule disp_apache2.c&gt;<br /> SetHandler dispatcher-handler<br /> ModMimeUsePathInfo On<br /> &lt;/IfModule&gt;<br /> <br /> Options FollowSymLinks<br /> AllowOverride None<br /> &lt;/Directory&gt;<br /> ...</p> <p></p> </td> 
  </tr> 
 </tbody> 
</table>

### Enable Support for HTTPS (Unix and Linux) {#enable-support-for-https-unix-and-linux}

Dispatcher uses OpenSSL to implement secure communication over HTTP. Starting from Dispatcher version **4.2.0**, OpenSSL 1.0.0 and OpenSSL 1.0.1 are supported. Dispatcher uses OpenSSL 1.0.0 by default. To use OpenSSL 1.0.1, use the following procedure to create symbolic links so that Dispatcher uses the OpenSSL libraries that are installed.

1. Open a terminal and change the current directory to the directory where the OpenSSL libraries are installed, for example:

   ```shell
   cd /usr/lib64
   ```

1. To create the symbolic links, enter the following commands:

   ```shell
   ln -s libssl.so libssl.so.1.0.1
   ln -s libcrypto.so libcrypto.so.1.0.1
   ```

### Next Steps {#next-steps-1}

Before you can start using the Dispatcher you must now:

* [Configure](dispatcher-configuration.md) Dispatcher
* [Confgure AEM](page-invalidate.md) to work with Dispatcher.

## Sun Java System Web Server / iPlanet {#sun-java-system-web-server-iplanet}

>[!NOTE]
>
>Instructions for both Windows and Unix environments are covered here. 
>
>Please be careful when selecting which to execute.

### Sun Java System Web Server / iPlanet - Installing your Web Server {#sun-java-system-web-server-iplanet-installing-your-web-server}

For full information on how to install these web servers, please refer to their respective documentation:

* Sun Java System Web Server
* iPlanet Web Server

### Sun Java System Web Server / iPlanet - Add the Dispatcher Module {#sun-java-system-web-server-iplanet-add-the-dispatcher-module}

The Dispatcher comes as either:

* **Windows**: a Dynamic Link Library (DLL)
* **Unix**: a Dynamic Shared Object (DSO)

The installation archive files contains the following files - dependent on whether you have selected Windows or Unix:

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <td valign="top">File</td> 
   <td>Description</td> 
  </tr> 
  <tr> 
   <td valign="top">disp_ns.dll</td> 
   <td><strong>Windows</strong>:<br /> The Dispatcher dynamic link library file.</td> 
  </tr> 
  <tr> 
   <td valign="top">dispatcher.so</td> 
   <td><strong>Unix</strong>:<br /> The Dispatcher shared object library file.</td> 
  </tr> 
  <tr> 
   <td valign="top">dispatcher.so</td> 
   <td><strong>Unix</strong>:<br /> An example link.</td> 
  </tr> 
  <tr> 
   <td valign="top">obj.conf.disp </td> 
   <td>An example configuration file for the iPlanet / Sun Java System web server.</td> 
  </tr> 
  <tr> 
   <td valign="top">dispatcher.any </td> 
   <td>An example configuration file for the Dispatcher.</td> 
  </tr> 
  <tr> 
   <td valign="top">README</td> 
   <td>Readme file that contains installation instructions and last-minute information.<br /> <strong>Note</strong>: Please check this file before starting the installation.</td> 
  </tr> 
  <tr> 
   <td valign="top">CHANGES</td> 
   <td>Changes file that lists issues fixed in the current and past releases.</td> 
  </tr> 
 </tbody> 
</table>

Use the following steps to add the Dispatcher to your web server:

1. Place the Dispatcher file in the web server's `plugin` directory:
1.

### Sun Java System Web Server / iPlanet - Configure for the Dispatcher {#sun-java-system-web-server-iplanet-configure-for-the-dispatcher}

The web server needs to be configured, using `obj.conf`. In the Dispatcher installation kit you will find an example configuration file named `obj.conf.disp`.

1. Navigate to `<*WEBSERVER_ROOT*>/config`.
1. Open `obj.conf`for editing.
1. Copy the line that starts:  
   `Service fn="dispService"`  
   from `obj.conf.disp` to the initialization section of `obj.conf`.  

1. Save the changes.
1. Open `magnus.conf` for editing.
1. Copy the two lines that start:  
   `Init funcs="dispService, dispInit"`  
   and  
   `Init fn="dispInit"`  
   from `obj.conf.disp` to the initialization section of `magnus.conf`.

1. Save the changes.

>[!NOTE]
>
>The following configurations should all be on one line and the `$(SERVER_ROOT)` and `$(PRODUCT_SUBDIR)` must be replaced by the respective values.

**Init**

The following table lists examples that can be used; the exact entries are according to your specific web server:

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <td>Windows<br /> and<br /> Unix</td> 
   <td><span class="code">...<br /> Init funcs="dispService,dispInit" fn="load-modules" shlib="$(SERVER_ROOT)/plugins/dispatcher.so"<br /> Init fn="dispInit" config="$(PRODUCT_SUBDIR)/dispatcher.any" loglevel="1" logfile="$(PRODUCT_SUBDIR)/logs/dispatcher.log"<br /> keepalivetimeout="60" <br /> ...</span></td> 
  </tr> 
 </tbody> 
</table>

where: 

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <td valign="top">Parameter</td> 
   <td>Description</td> 
  </tr> 
  <tr> 
   <td valign="top">config</td> 
   <td>Location and name of the configuration file <span class="code">dispatcher.any.</span></td> 
  </tr> 
  <tr> 
   <td valign="top">logfile </td> 
   <td>Location and name of the log file.</td> 
  </tr> 
  <tr> 
   <td valign="top">loglevel </td> 
   <td>Log level for when writing messages to the log file:<br /> <strong>0</strong> Errors<br /> <strong>1</strong> Warnings<br /> <strong>2</strong> Infos<br /> <strong>3</strong> Debug<br /> <strong>Note</strong>: It is recommended to set the log level to 3 during installation and testing and to 0 when running in a production environment.</td> 
  </tr> 
  <tr> 
   <td>keepalivetimeout</td> 
   <td>Specifies the keep-alive timeout, in seconds. Starting with Dispatcher version 4.2.0 the default keep-alive value is 60. A value of 0 disables keep-alive. </td> 
  </tr> 
 </tbody> 
</table>

Depending on your requirements you can define the Dispatcher as a service for your objects. To configure the Dispatcher for your entire website modify the default object:

<table border="1" cellpadding="1" cellspacing="0" columns="3" header="none" width="600"> 
 <tbody> 
  <tr> 
   <td>Windows</td> 
   <td><span class="code">...<br /> NameTrans fn="document-root" root="$(PRODUCT_SUBDIR)\dispcache"<br /> ...<br /> Service fn="dispService" method="(GET|HEAD|POST)" type="*\*"<br /> ...</span></td> 
  </tr> 
  <tr> 
   <td>Unix </td> 
   <td><span class="code">...<br /> NameTrans fn="document-root" root="$(PRODUCT_SUBDIR)/dispcache"<br /> ...<br /> Service fn="dispService" method="(GET|HEAD|POST)" type="*/*"<br /> ...</span></td> 
  </tr> 
 </tbody> 
</table>

### Next Steps {#next-steps-2}

Before you can start using the Dispatcher you must now:

* [Configure](dispatcher-configuration.md) Dispatcher
* [Confgure AEM](page-invalidate.md) to work with Dispatcher.
