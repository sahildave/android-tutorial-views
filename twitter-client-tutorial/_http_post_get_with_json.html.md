In the previous lesson, we did a basic HTTP call for learning how network calling works. This time we would use JSON for sending and receiving data over network. After extracting the required token from the JSON response, we would use it to judge whether the user had already logged in before or not.

### Sending & Receiving JSON ###

First of all, you would need to tell the server what kind of data you are sending and what kind would you like to receive. You should set both, the `Content-Type` and `Accept` type to `application/json` for this purpose.

Now, instead of sending a string as the parameter, you should create a `JSONObject` and put the required parameters in it and send its string representation.

    ...
    
    con.setRequestMethod("POST");
    ...
    // We are now sending and receiving JSON,
    // Set the request properties to JSON.
    con.setRequestProperty("Content-Type", "application/json");
	con.setRequestProperty("Accept", "application/json");
    
    // Create a JSON object and put two credentials -
    // the usernameString with "username" key
    // and passwordString with "password" key
	JSONObject jObj = new JSONObject();
	jObj.put(key, value);
    
    // Write the JSON's .toString() representation in the OutStream


Now this way, we would be sending a JSON string which would have username and password from the textfields.

The response would also be a JSON string. Instead of using it directly, like we did last time, you should first convert it to a JSONObject by passing its string representation as a parameter. The key we are interested in is "token" and you should extract the value using .get(...) method. This would be the new `responseString`.

Further, in the `onPostExecute`, you should use this string and save it in the SharedPreferences. Save the `usernameString` and `passwordString` as well. You should now remove the `Toast` object so that the token is not displayed on the screen after login.


### Modify App Flow ###

You should now edit your code in onCreate to check if token is present or not. If yes, then skip to the `TweetListActivity` just like before.

Tip: After starting the `TweetListActivity`, you should `finish();` the current activity. This would ensure that if the user clicks the back button while he is in `TweetListActivity`, he should not return to the login screen.


#### Help

If you're having trouble completing this module, read on for some help.

1. You should set the keys for username and passwords as "username" and "password", otherwise our test won't pass your app.
2. Set the parameters of `DataOutputStream` as `jObj.toString();`, where jObj is the JSONObject that you have created to put in the credentials.


Once completed, you should test your app by deploying it as an Android app. If you are assured that the app works fine, run it with Codelearn launcher 'Android App Codelearn' to get it tested with our Virtual Assistant. You will see a popup here in the browser intimating you of success/error of your submission.

If you have not setup Virtual Assistant yet, get started with the setup [here](http://www.codelearn.org/android-tutorial/twitter/10/setup/setup-virtual-assistant).