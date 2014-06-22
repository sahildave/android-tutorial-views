Till now, we were creating and fetching the tweets locally. We had created the login screen and were storing the username and password in our SharedPreferences. In reality, the app should send the username and password entered by the user to a server which would send back a token which can be used to identify the user. The token is used for fetching the twitter feed from the server for the user.

In this module, we would learn all the basic aspects of making HTTP calls to a server and receiving the response. The response can be used for many purposes, but we would be using it to get a token for the user. Further, we would be learning how to get tweets from a server and hook them up with our Listview.

### HTTP clients

If not all, most of the Android apps in the market send and receive data from a server. For this purpose, HTTP clients are used. They make calls to the server in a separate background thread. There are three major clients which are used in Android network programming. 

#### Apache HTTP clients

[`DefaultHttpClient`](http://developer.android.com/reference/org/apache/http/impl/client/DefaultHttpClient.html) and its sibling [`AndroidHttpClient`](http://developer.android.com/reference/android/net/http/AndroidHttpClient.html) are based on Apache HttpClient libraries. Although they are stable but because of their large size, the next client is more preferred. 

#### HttpUrlConnection

Unlike Apache clients, [`HttpUrlConnection`](http://developer.android.com/reference/java/net/HttpURLConnection.html) is a general-purpose lightweight client which is more suited for mobile applications with respect to Apache clients. Moreover, Google developers have mentioned before in their [blog](http://android-developers.blogspot.in/2011/09/androids-http-clients.html) that they would be focussing more on developing HttpUrlConnection client rather than Apache.

### Request Methods

When an HTTP call is made to a server, the server should know what kind of action the call is requesting. For this purpose while creating an HTTP client we add the required `RequestMethod` property to the client object. There are several [Request Methods](http://en.wikipedia.org/wiki/HTTP#Request_methods) which can be used with HTTP. In this project we'd be using two of them, namely `POST` and `GET`. 

The names should give a little hint, `GET` is used when some kind of data is needed from the server without making an change to the server data. On the other hand, if you need to send some data to server and need a response, you should use `POST`. There are many more differences but those are out of scope of this tutorial.

### Working with HttpUrlConnection


**Step 1:** Making network calls with HttpUrlConnection starts with creating a `URL` object and opening a connection by calling & casting `URL.openConnection()`.

**Step 2:** You can now add the different properties to your connection object, for eg: `HttpUrlConnection#setRequestMethod`, `HttpUrlConnection#setRequestProperty` etc.

**Step 3:** For transmitting data you should get an output stream by using `HttpUrlConnection#getOutputStream` method and write the required parameters onto the stream object. Don't forget to close the stream to avoid memory leaks. At this point your data is being sent and you should prepare to read the response

**Step 4:** Similar to how you got the output stream, you should now get the input stream from the connection object. Pass this input stream to a `BufferedReader`object, which would let you read the response from the input stream.

    //create URL object and create a connection
    URL url = new URL(..);
    HttpURLConnection con = (HttpURLConnection) url.openConnection();
    try{
	    //Add the properties to the connection object
	    con.setRequestMethod(..)
		con.setRequestProperty(..)
	    ...
	    //Get output stream and input
	    DataOutputStream wr = new DataOutputStream(con.getOutputStream());
	    //write your parameters in the wr object and close it
	    ..
	    //Get input stream and read it for the response
	    BufferedReader br = new BufferedReader(new InputStreamReader(con.getInputStream()));
	    //Use the br object to read and use the response string
		..
    } catch(..){ 
    	..
    }


### Adding Permissions to access internet

By default, Android doesn't allow any app to impact directly on the device and the user data in anyway. The app should have particular [permissions](http://developer.android.com/guide/topics/security/permissions.html#permissions) which are granted by the user while installing the app from Play Store. There are around [150 different permissions](http://developer.android.com/reference/android/Manifest.permission.html), ranging from using internet to finding location.

You would need the [`INTERNET`](http://developer.android.com/reference/android/Manifest.permission.html#INTERNET) permission to let the app contact the server in the following lessons.

	<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	    package="com.android.app.myapp" >
	    <uses-permission android:name="android.permission.INTERNET" />
	    ...
	</manifest>
	

