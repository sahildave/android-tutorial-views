Till now we were creating our own list with a for loop. In this lesson, we would fetch tweets from the server and display them on our list. Simultaneously, we would be saving the list in storage.


### Editing AsyncFetchTweets 

Now that you know about HTTP clients, you should create a `HttpUrlConnection` client with `GET` request method. Your connection should "Accept" JSON response array from the server which you should use to populate the listview.

You would need to pass the response string to a `JSONTokener` object and pass the tokener object to a JSONArray object.

	JSONTokener tokener = new JSONTokener(sb.toString());
	result = new JSONArray(tokener);

Like previously, you should now use the `Tweet` model to fill up your listview. From the `result` JSONArray, you should get the JSONObject and extract `body` and `title` from the particular object.

The `renderTweets` method adds the `Tweet` object list to the listview but deletes the previous list which was populated from the local files. This can be fixed, and would be covered in the next lesson of this module.

#### Help

If you're having trouble completing this module, read on for some help.

1. You should checkout Lesson 17 to see how you could populate `Tweet` objects list array.
2. The JSONArray from the feed is an array of JSONObject, so you would need to extract the JSONObject at every iteration of for loop, and extract "title" and "body" from the object.

You should test your app by deploying it as an Android app. If you are assured that the app works fine, run it with Codelearn launcher 'Android App Codelearn' to get it tested with our Virtual Assistant. You will see a popup here in the browser intimating you of success/error of your submission.

If you have not setup Virtual Assistant yet, get started with the setup [here](http://www.codelearn.org/android-tutorial/twitter/10/setup/setup-virtual-assistant).