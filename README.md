# HMSAuth
Authentication for Xamarin Forms

# Installation
You can install the package with NuGet.
Ths package does not contain the dependencies, that means you have to install them.
You need the followiong packages:
	Newtonsoft.JSON 
	PCLCrypt 

# Usage
## Console application

Create a console application
	set .NET Framework to 4.5
	NuGet:
	add Newtonsoft.JSON 
	add PCLCrypt 
	add HMSAuth
	
```C#
	 static void Main(string[] args)
        {
            Task.Run(async () =>
            {
                HMS.Auth.Login hmsLogin = new HMS.Auth.LoginNTLM("http://winxmap/NTLM", "winxmap", "authtest", "hello@123");
                string ret = await hmsLogin.getJSON("data.json");
                Console.WriteLine(ret);
            }
            ).Wait();
            Console.WriteLine("done");
                
        }
´´´

	
## Xamarin Forms application
	
Create a Xamarin Forms project

1. retarget the Solution
	1.1 NuGet: remove Xamarin Forms from all projects
	1.2 NuGet: remove all other NuGet packages
	1.3 restart VS, if required
	1.4 change target, i.e. remove Windows Phone Silverlight 8.1 from (Portable)
	1.5 NuGet: add Xamarin Forms to all projects
	1.6 NuGet: add Microsoft.NETCore.UniversalWindowsPlatform to UWP
	1.7 restart VS, if required
2. add the dependencies
	add NuGet: Newtonsoft.JSON to all projects
	add NuGet: PCLCrypt to all projects
	add NuGet: HMSAuth to (Portable)
3. adjust the capabilities	
	If you want to acces server in the the LAN, you have t0 change some capabilities:
	3.1 Windows 8.1: 
		Package.appxmanifest
		Private Networks (Client and Sever)
	3.2 UWP
		open Package.appxmanifest
		allow Private Networks (Client and Sever)
4. adkust the App class
	We have to know the platform but we cannot use the Device class, since we have to distinguish UWP,Windows 8.1 and Wiondows Phone 8.1		.	
	A simple way is, to add a string argument to the App class 
	e.g.: 
```C#
		new App("Droid")
´´´
	Valid platform nanes are
	..* Droid
	..* iOS
	..* UWP
	..* Windows
	..* WindowsPhone

```C#
		HMS.Auth.AppPlatform platform = HMS.Auth.AppPlatform.UWP;
			public App(string platform)
			{
				Enum.TryParse(platform, out this.platform);
				...
´´´	
	
	
	

