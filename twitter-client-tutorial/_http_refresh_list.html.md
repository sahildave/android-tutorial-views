Currently, when new objects are added, it overwrites the previous visible list. In this lesson, we would first find a solution to this problem and then would add a "Refresh" button in the actionbar, which would get more tweets and add them on top.

### Adding Menu items on ActionBar

Menu buttons on actionbar can be used for multitude of tasks. You can edit the xml files in the `menu` folder to add, delete or edit menu items on actionbar.

	<menu xmlns:android="http://schemas.android.com/apk/res/android" >
		...
	    <item
	        android:id="@+id/action_settings"			
	        android:showAsAction="never"
	        android:title="@string/action_settings"/>
		...
	</menu>

You can customize the order, visibility and many more aspects of a menu item by adding different identifiers which you can [read about here](http://developer.android.com/guide/topics/resources/menu-resource.html).

### Listening to Menu item click

Actionbar items dont have a listener like buttons, the actions can be handled by overriding `onOptionsItemSelected` method instead. You should have the id of the view you want to handle and compare it with the Id of the item clicked.

    	public boolean onOptionsItemSelected(MenuItem item) {
			// get id of clicked item
    		int id = item.getItemId();
			..
    		if (id == R.id.action_refresh) {
    			//refresh the list
    			return true;
    		}
			..
    	}

### Assignment - Add refresh button and show new tweets at top

#### Refreshing List

1. Create an item in the actionbar to refresh list. Call `AsyncFetchTweets` task when the button is pressed.
2. Add the newly fetched tweets the old list.


#### Avoiding Overwriting of list
The major cause overwriting is that our list adapter is being initialized everytime a list is added. First time, the adapter is initialized when tweets from local storage are used. This works fine but as soon as the tweets are fetched from the server, `renderTweets` is again called, which re-initializes the adapter.

1. Initialize adapter only once, preferably in `onCreate`.
2. Modify the `renderTweets` method to add the new list to the top of the previous list by using a loop or using #addAll(..) method. 


#### Help

If you're having trouble completing this module, read on for some help.

1. Move the call to async task into a public method `refreshList();`

	    public void refreshlist() {
    		AsyncFetchTweets asnyc = new AsyncFetchTweets(this);
    		asnyc.execute();
    	}
2. You can add the new tweets on top of old list by using `#addAll(0, newTweets);` and calling `adapter.notifyDataSetChanged();`
