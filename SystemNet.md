##Authentication in a portable library with System.Net.Http

If you want to access a server with authentication, for example `NTLM`, you can do so with the `HttpClient` from `System.Net.Http`.


	NetworkCredential netCred = new NetworkCredential(this.userName, this.password);

    using (var handler = new HttpClientHandler() { AllowAutoRedirect = false })
    using (var client = new HttpClient( handler))
    {
		handler.Credentials = netCred;
    	var request = new HttpRequestMessage(HttpMethod.Get,useUrl);
		request.Headers.Add("Connection", "keep-alive");
        HttpResponseMessage response = await client.SendAsync(request); 
	}



#### I am aware that the using blocks are not always the best solution, but currently its the way we do it. ####

There are three important points to have in mind:

1. Make sure that you use `NetworkCredential` and not `CredentialCache`.
	- This guaranties that the HttpClient does the handshaking.
2. Set `AllowAutoRedirect` to   `false`.
	- This guaranties that you are not redirected to the Login page.
3. Set the `Connection` header to `keep-alive`.
	- This maybe required for the handshaking.



###Problems on UWP and Windows Phone 8.1###

If you only want to read data (`HttpMethod.Get`) this will work on all platforms.

But if you need `HttpMethod.Put` or `HttpMethod.Post` on UWP as well, you will run into the bug described in 
[Win10SDKToolsIssues](https://social.msdn.microsoft.com/Forums/en-US/9e137127-e0e5-4aec-a7a9-d66f5b84c70b/rtm-known-issue-systemnethttphttpclient-or-httpwebrequest-class-usage-in-a-uwp-app-throws-a?forum=Win10SDKToolsIssues "Win10SDKToolsIssues").

On Windows Phone 8.1 there maybe the error:

`The Connection header is empty.` 

This forces you to switch over to `Windows.Web.Http.HttpClient`. But this can not be done in portable library.


