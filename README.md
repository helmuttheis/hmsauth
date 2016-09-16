# HMS.Auth
Authentication for Xamarin Forms

For a simple approach with `System.Net.Http` see [Authentication in a portable library with System.Net.Http](wiki/Authentication-in-a-portable-library-with-System.Net.Http)

# Installation
You can install the package with NuGet.

**The package does not contain the dependencies, that means you have to install them.**

You need the following packages:

- Newtonsoft.JSON 
- PCLCrypt 

# Usage
## Console application

Create a console application.

- set .NET Framework to 4.5	
- NuGet:
	- add Newtonsoft.JSON 
	- add PCLCrypt 
	- add HMSAuth

With the following code you can fetch JSON data from a server with NTLM authentication:

	static void Main(string[] args)
    {
    	Task.Run(async () =>
    	{
    		HMS.Auth.Login hmsLogin = new HMS.Auth.LoginNTLM("http://winxmap/NTLM", "winxmap", "authtest", "hello@123");
    		string ret = await hmsLogin.getJSON("data.json");
    		Console.WriteLine(ret);
    	}).Wait();
    	Console.WriteLine("done");
    }
    
    
	
## Xamarin Forms application
	
Create a Xamarin Forms project.

###Retarget the Solution
- NuGet: remove Xamarin Forms from all projects
- NuGet: remove all other NuGet packages
- restart VS, if required
- change target, i.e. remove Windows Phone Silverlight 8.1 from (Portable)
- NuGet: add Xamarin Forms to all projects
- NuGet: add Microsoft.NETCore.UniversalWindowsPlatform to UWP
- restart VS, if required

###Add the dependencies with NuGet
- add Newtonsoft.JSON to all projects
- add PCLCrypt to all projects
- add HMSAuth to (Portable)

###Adjust the capabilities 
If you want to access a server in the LAN, you have to change some capabilities:

- Windows 8.1: 
	- Package.appxmanifest
	- Private Networks (Client and Sever)
- UWP
	- open Package.appxmanifest
	- allow Private Networks (Client and Sever)

###Adjust the App class
We have to know the platform but we cannot use the Device class, since we have to distinguish UWP,Windows 8.1 and Windows Phone 8.1.	

This can be achieved by adding a string argument to the App class constructor,
e.g.: 


    new App("Droid")
    

Valid platform names are:


	Droid
	iOS
	UWP
	Windows
	WindowsPhone

In the App constructor you can get the enum value for the string as follows:


    HMS.Auth.AppPlatform platform = HMS.Auth.AppPlatform.UWP;
    public App(string platform)
    {
    	Enum.TryParse(platform, out this.platform);
    	...
    
    

	
	
	

