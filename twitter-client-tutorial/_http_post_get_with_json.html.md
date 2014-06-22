In the previous lesson, we did a basic HTTP POST call and sent a simple `String` as the parameter. We received a String saying "*Hello, <username\>*" and displayed it on the screen as a `Toast` message. 

In this lesson, we would be sending and receiving `JSON` `String`. The response would be a unique token which would be used to determine the logged-in status of user in the app.


### Creating JSON

Instead of sending a string as the parameter that you used in the last lesson, you should now create a `JSONObject` and put the required parameters in it and send its string representation as the parameter.

	//Create JSON Object	
	JSONObject jObj = new JSONObject();
	jObj.put("key", "value");
	..
	//Add JSON Object's string representation as parameter
	wr.writeBytes(jObj.toString());
	


### Sending and receiving JSON ###

Before sending the JSON string as parameter, you should set some request properties about the headers of the connection you want.
	
	con.setRequestProperty(field, property);

This sets the value of the specified request header `field` to `property`.

### Reading JSON

When you have received the response from the server, convert it to a JSON Object by passing the response string as parameter when creating a response JSON Object.
From this JSON object, you can extract the value of key by a simple JSONObject#get() method.

	JSONObject json = new JSONObject(jsonString);
	valueFromJson = json.get(key);


### Assignment - Save token from server and modify app flow

1. As we are dealing with JSON now, you should change the `URL` you using to 

    	http://app-dev-challenge-endpoint.herokuapp.com/login

2. After receiving the JSON, you should extract the value from the key `token`.
3. If the response is successfully received, save the token in `SharedPreferences` and create an Intent to open the `TweetListActivity`.
4. You should now modify the app flow by checking if the token is present in `SharedPreferences` and open the `TweetListActivity` if it is.


#### Help

If you're having trouble completing this module, read on for some help.

1. You should set the keys for username and passwords as "username" and "password", otherwise our test won't pass your app.
2. Set the request properties of connection to `"application/json"` for `"Accept"` and `"Content-Type"` by using:

	    con.setRequestProperty("Content-Type", "application/json");
		con.setRequestProperty("Accept", "application/json");
	    