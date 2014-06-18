Currently, when new objects are added, it overwrites the previous visible list. In this lesson, we would first find a solution to this problem. Moreover, we would also add a "Refresh" button in the actionbar, which would get more tweets and add them on top.

### Not Overwriting Old List

The major cause of this issue is that our list adapter is being initialized everytime a list is added. First time, the adapter is initialized when tweets from local storage are used. This works fine but as soon as the tweets are fetched from the server, `renderTweets` is again called, which re-initializes the adapter.

This can be avoided by:

1. Initializing adapter only once, preferably in `onCreate`. You should move the initializing and setting list adapter logic to `onCreate`.

2. Modify the `renderTweets` method to take add the new list to the top or at the 0th position of the previous list. 

	    public void renderTweets(List<Tweet> tweets) {
			//Add the received tweets to the initial list which is set
			//on the adapter. Remember to add the received list at 
			//the top of the previous list
    		tweetsRead.addAll(0, tweets);
			//Call the notifyDataSetChanged() to refresh the adapter
    		tweetItemArrayAdapter.notifyDataSetChanged();
    		...
    	}
	

### Refresh To Get New Items

We would create a button in the actionbar which would refresh the list when pressed. This can be done, if the `AsyncFetchTweets` task is called everytime the button is pressed. For doing the above you should:

1. Separate out the logic calling `AsyncFetchTweets` task into a public method which can be called when needed.
2. Create a `Menu` item in `tweet_list.xml` and add a click listener which would call the public method which is created in step 1.


#### Help

If you're having trouble completing this module, read on for some help.

1. Move the call to async task into a public method `refreshList();`

	    public void refreshlist() {
    		AsyncFetchTweets asnyc = new AsyncFetchTweets(this);
    		asnyc.execute();
    	}
2. Actionbar items dont have a listener like buttons, the actions can be handled by overriding `onOptionsItemSelected` method. You should have the id of the view you want to handle and compare it with the Id of the item clicked.

    	public boolean onOptionsItemSelected(MenuItem item) {
    		int id = item.getItemId();
			..
    		if (id == R.id.action_refresh) {
    			refreshlist();
    			return true;
    		}
			..
    	}


You should test your app by deploying it as an Android app. If you are assured that the app works fine, run it with Codelearn launcher 'Android App Codelearn' to get it tested with our Virtual Assistant. You will see a popup here in the browser intimating you of success/error of your submission.

If you have not setup Virtual Assistant yet, get started with the setup [here](http://www.codelearn.org/android-tutorial/twitter/10/setup/setup-virtual-assistant).