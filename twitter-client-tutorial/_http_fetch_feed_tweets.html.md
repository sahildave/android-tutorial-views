Since [Module 8](http://www.codelearn.org/android-tutorial/twitter/listview-dynamic-data-example), we were creating and populating the tweet list with a for loop. In this lesson, we would fetch tweets from the server and display them on our list. Simultaneously, we would be saving the list in storage as well.


### Fetching Dummy Tweets

This time we are just reading from the server and not sending any parameters. This is the case when you should use `"GET"` request method. The logic for catching the response remains the same for both these of request method but our server sends the dummy tweets as an Array of JSONObjects.

### Reading Dummy Tweets

For reading Array of JSONObjects, we would need a [`JSONTokener`](http://developer.android.com/reference/org/json/JSONTokener.html), which helps in converting a JSON string into a JSON array.

	JSONTokener tokener = new JSONTokener(jsonString);
	JSONArray array = new JSONArray(tokener);

This `array` object would contain JSONObject as values which can be extracted by using `#get(position)` method.

### Assignment - Reading and displaying dummy tweets in list

Using the knowledge of knowledge of network calling, you can now edit out the `AsyncFetchTweets` class to receive the dummy tweets from the server. Extract the body, title from the JSON Objects and show them on the list using the `Tweet` model.

The `renderTweets` method does add the `Tweet` object list to the listview but deletes the previous list which was populated from the local files. This can be fixed, and would be covered in the next lesson of this module.

#### Help

If you're having trouble completing this module, read on for some help.

1. You should checkout Lesson 17 to see how you could populate `Tweet` objects list array.
2. The JSONArray from the feed is an array of JSONObject, so you would need to extract the JSONObject at every iteration of for loop, and extract "title" and "body" from the object in a loop. You can extract a value from the array elements by using:
	
		tweet.setTitle(array.getJSONObject(i).getString("title"));