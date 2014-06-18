You already know the benefits of AsyncTask class. It helps you by making a new thread and doing all the long-running tasks in the background while keeping your user interface responsive. As getting a token from a server can sometimes take longer than expected, we should put the HTTP code in an AsyncTask class. 

Lets begin by making an AsyncTask in our `MainActivity` itself. Just before the the curly bracket showing the end of `MainActivity` class, add a nested MyAsyncTask class as, 

    private class MyAsyncTask extends AsyncTask<Void, Void, Boolean> {
    	
		@Override
		protected Boolean doInBackground(Void... params) {
			return null;
		}

		@Override
		protected void onPostExecute(Boolean result) {
			super.onPostExecute(result);
		}	

    }

This is the basic overview of our `MyAsyncTask` (you can change the name to anything you like) which we would be working with. We would be making HTTP calls in the `doInBackground(...)` method which would return a boolean `result` to the `onPostExecute` method. This boolean would be true only when the HTTP call is successful and a token is received from the server. If there are any exceptions while making the call, the boolean would be false and error would be displayed by `onPostExecute`.

### Sending Data ###

There are two major HttpClients which can be used for making calls. First is `HttpURLConnection` and the second, `DefaultHttpClient`. We would be using the former as it is lightweight and is more suitable for mobile development. 

Start up by making an `URL` object. This would contain the url 
which would be called for a response. Here, we are using our server, which returns a `String` as response. Before the `onCreate` of `MainActivity`, create a global `String` variable pointing to our server.

    String codelearnUrl = "http://app-dev-challenge-endpoint.herokuapp.com/loginstring";

Now lets go back to the Async class.  
The server connection can throw different types of error while making calls. To cope with this create a try-catch block, and in it add a `URL` object:

    URL url = new URL(codelearnUrl);

We would use it to open a connection to the server. Continue writing,

    HttpURLConnection con = (HttpURLConnection) url.openConnection();
    con.setRequestMethod("POST");
    String parameters = "username=" + usernameString;
    
*You would be getting an error at `usernameString` but we would fix it just after completing the Async class.*

There are different methods of sending requests to a server and here we are using `POST`. You can read more about different request methods [here](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods). 

Now that we have our connection open, lets create something to send our data with. We would get the `OutputStreamWriter`object from our `con` object and write the parameters in it. After we are done with writing the parameters we would close the stream to release any system resources associated with it.

    DataOutputStream wr = new DataOutputStream(con.getOutputStream());
    wr.writeBytes(parameters); 
    wr.flush();
    wr.close();

At this point, all our data is send. We should now handle the response.

### Receiving Data ###

Just like we used the `OutputWriter` to write to the stream, we would use the `InputReader` to read the stream. Begin by creating a `BufferedReader` object by passing the input reader from our connection `con`.

	BufferedReader br = new BufferedReader(new InputStreamReader(con.getInputStream()));

This `BufferedReader` can be used to populate a `StringBuffer` object which can be used to construct a String object for our use. Start by declaring a String variable `line` and creating a `StringBuffer` object. We would read lines from `br` and append them to our `StringBuffer`.

    String line;
    StringBuffer sb = new StringBuffer();
    
    while ((line = br.readLine()) != null) {
    	sb.append(line + "\n");
    }
    
Now we have the response from the server in our `StringBuffer` object. We would extract it to a `String`. Finally, you should close the `BufferedReader` by using `br.close();`.

As we would be returning true only if no exception occur, therefore add `return true;` only at the end of try-block. If any exception is caught, you should return false.


### Using Data ###

As explained in previous lessons, the `onPostExecute` method is automatically called after `doInBackground` returns the required class object. We would keep it simple and just check what is stored in `responseString`.

If the `result` parameter is true, we would directly display the `responseString` on the mobile screen by using Toast messages, otherwise we would share our concern of something going wrong. Create a Toast object in the if-block by:

    if (result) { // if no exception occured
    	Toast.makeText(MainActivity.this, responseString, Toast.LENGTH_LONG).show();
    } else {
    	...
    }

For the `Toast` object we are passing a context, the string which would want to display and the time duration for which it would be showed. Calling `.show();` is very important because otherwise nothing would be displayed. In the else-block you can add another `Toast` message displaying another string on user's mobile screen saying the login has failed.

### Calling AsyncTask ###

We would be calling the `MyAsyncTask` method when the user clicks on the login button. Start by declaring `usernameString` and `passwordString` globally, before `onCreate` method just like the Button `_loginBtn`.

You should delete the code which puts the username and password into the SharedPreferences because we would later on save the token instead of username and password. Also remove the `intent` to start the `TweetListActivity` because from now on, we would call it from our AsyncTask class instead.

<strike>

	SharedPreferences prefs = getSharedPreferences("codelearn_twitter", MODE_PRIVATE);
    
	Editor edit = prefs.edit();
    edit.putString("user",username.getText().toString());
    edit.commit();
    edit.putString("pass",password.getText().toString());
    edit.commit();
    
    Intent intent = new Intent(MainActivity.this, TweetListActivity.class);
    startActivity(intent); 
</strike>

You should now add the texts from the edit fields into the global strings that you have declare. This should also remove the error, from the asynctask.

    usernameString = username.getText().toString();
    passwordString = username.getText().toString();
    
    MyAsyncTask async = new MyAsyncTask();
    async.execute();


The code is now ready to be deployed but it would throw a Runtime Error if you dont give app the permission to access the internet.  
Open the AndroidManifest.xml file and add permission for internet before the `<uses-sdk.../>` tag. 

    <uses-permission android:name="android.permission.INTERNET" />


The app is now finally ready. You should test your app by deploying it as an Android app. If you are assured that the app works fine, run it with Codelearn launcher 'Android App Codelearn' to get it tested with our Virtual Assistant. You will see a popup here in the browser intimating you of success/error of your submission.

If you have not setup Virtual Assistant yet, get started with the setup [here](http://www.codelearn.org/android-tutorial/twitter/10/setup/setup-virtual-assistant).