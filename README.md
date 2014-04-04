Azure FAQ
=========

A place to put helpful information on using the Azure platform

Windows Azure Website Reverse Proxy
=========

I recently needed to create a reverse proxy on an Azure Website. Websites have ARR installed, but proxies are disabled. Turns out it can be done with through the use of site extensions. Information about site extensions can be found at
https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions.

In order to do this you need to place an applicationHost.xdt file directly under the site directory.

```
/site
    applicationHost.xdt
    /wwwroot
/LogFiles

```

You can do this using Kudu. Information on accessing Kudu can be found at https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service

Inside of the applicationHost.xdt we need to modify the servers applicationHost.config to enable proxies. To do this we can use the following code below.

*applicationHost.xdt*
```

<?xml version="1.0"?> 
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform"> 
    <system.webServer> 
            <proxy enabled="true"  xdt:Transform="Insert"  />
    </system.webServer> 
</configuration> 

```

Simply restart your website after adding the applicationHost.xdt file with the code above. This will enable the use of reverse proxies for your website. Now you can configure your web.config or use the IIS Manager to remotely manage your website, http://blogs.msdn.com/b/windowsazure/archive/2014/02/28/remote-administration-of-windows-azure-websites-using-iis-manager.aspx

Hope this helps anyone else who has the same requirements.
