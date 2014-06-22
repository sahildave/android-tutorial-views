Till now we were saving the username and password in our `SharedPreferences` and checking them on the app start to modify the app flow. If you want a refresher or may have missed the lesson, you can go through the [Module 7](http://www.codelearn.org/android-tutorial/twitter/sharedpreference-example) again.

From now on, instead of using username and password as a criteria to check user's logged-in status, we would use a unique token received from our dummy server. As getting a token from a server can sometimes take longer than expected, you should put the HTTP code in an AsyncTask class. Again, if you don't remember some aspects about AsyncTask, you can head back to [Module 10](http://www.codelearn.org/android-tutorial/twitter/asynctask-tutorial-example).

### Sending Data

You should use `"POST"` request method here as we are sending some parameters to the server. The parameter would be the data the user have entered in the username fields in the login Activity. The parameter should be preceded by a `"username="` string so that it looks something like

	String parameters = "username=" + usernameFromField;

This would ensure that the username is read correctly by our server and the designated response is sent.

### Receiving Data ###

As explained earlier, the response is read using a `BufferedReader` object. After reading the response string it should be displayed in a `Toast` message on the screen. 

`Toast` displays a small overlaying text on the bottom of the device's screen for few seconds. It should be used to give a small feedback to the user about any action that he had performed.

	Toast.makeText(context, responseString, Toast.LENGTH_LONG).show();

First and second parameters are self-explanatory. The third parameter defines the time for which the `Toast` would be shown. Don't forget to add `Toast#show()` method at the end, otherwise the Toast wont be shown on the screen.


### Assignment - Receiving a response showing a Toast message

Create an nested `AsyncTask` class in your `MainActivity`. From now on, we would be using this class to get response from the server and in the later lessons, saving data to `SharedPreferences`. 

##### Steps

1. You should now call the newly created AsyncTask when the button is clicked.
2. Use the below given URL to create a client connection to our dummy server.

	    codelearnUrl = "http://app-dev-challenge-endpoint.herokuapp.com/loginstring";

3. Get the response from the server and show it in a `Toast` message if the response is successfully received.