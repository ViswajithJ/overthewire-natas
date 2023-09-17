<h2>🔰 Level - 8</h2>

- username - natas8
- password - a6bZCNYwdKqN5cGP11ZdtPg0iImQQhAB

	Given info is same as level 6. A html page with an input field, and a link to its sourcecode.
	This time, `secret` is not loaded from any external paths, but given within the embedded php code.
	But its encoded. 
	Specifically, 
	```php
	function encodeSecret($secret) {
    	return bin2hex(strrev(base64_encode($secret)));
	}
	```
	I made a python script to convert the given hex string to bytes and then decode it to ASCII.
	Then I reversed the string and decoded the base64 string to get the value of `secret`

	Passed this to the input field and got the password to level 9.

	```python
	encoded_secret = "3d3d516343746d4d6d6c315669563362"

	string = bytes.fromhex(encoded_secret).decode("ASCII")

	string = string[-1::-1]

	print(string)
	```
Password for level 9 - Sda6t0vkOPkM8YeOZkAGVhFoaplvlJFd