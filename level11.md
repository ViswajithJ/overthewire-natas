<h2>🔰 Level - 11</h2>

- username - natas11
- password - 1KFqoJXi6hRaPluAmk8ESDW4fSysRoIg

	Concept is simple, but can be a bit tricky.

	Site contains the text "Cookies are protected with XOR encryption" and an input field, where we can give the hex value of a color. On submit, the page color changes to given color.<br>
	There was a link to view sourcecode as in the previous level.<br>
	```php
	$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

	function xor_encrypt($in) {
	    $key = '<censored>';
	    $text = $in;
	    $outText = '';

	    // Iterate through each character
	    for($i=0;$i<strlen($text);$i++) {
	    $outText .= $text[$i] ^ $key[$i % strlen($key)];
	    }

	    return $outText;
	}

	function loadData($def) {
	    global $_COOKIE;
	    $mydata = $def;
	    if(array_key_exists("data", $_COOKIE)) {
	    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
	    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
	        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
	        $mydata['showpassword'] = $tempdata['showpassword'];
	        $mydata['bgcolor'] = $tempdata['bgcolor'];
	        }
	    }
	    }
	    return $mydata;
	}

	function saveData($d) {
	    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
	}

	$data = loadData($defaultdata);

	if(array_key_exists("bgcolor",$_REQUEST)) {
	    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
	        $data['bgcolor'] = $_REQUEST['bgcolor'];
	    }
	}

	saveData($data);



	?>
	```

	What this code does is, it takes the default data and encrypt it using `xor_encrypt` function after encoding to json format and later converts to base64 format. Then it sets the final value as cookie value. This cookie value can be obtained by using intercept mode in burpsuite.<br>

	The trick was to find the key that `xor_encrypt` function uses. <br>
	The beautiful thing about xor is that it works in both ways. Its reversible. It works in any order.<br>
	i.e., if<br>
	a ^ b = c;<br>
	=> b ^ c = a<br>
	=> a ^ c = b<br>

	`xor_encrypt` here uses the `$key` to encrypt the value passed as `$in`. This returns the output as `$outText`<br>
	If we can get the `$in` value and `$outText` value, we could decipher the `$key` by simply xor-ing them together.<br>
	As per the code, data in `$in` is json encoded version of `$defaultdata` .<br>
	And the value of `$outText` is base64 decoded version of the cookie value.<br>
	Since we know both of these values, we can find the `$key` by xor-ing them. <br>

	We can then modify the data to make "showpassword" as yes, json-encode it and encrypt it using the newly found key, then base64-encode it and set it as the new cookie value.<br>

	On passing this modified request, the response contains the password for level 12.

	The tricky part was where I had the notion of `$key` being actually the password. So the repeating 4 letter word I got while printing `$key` didn't make sense. Later, from JohnHammond's walkthrough video, I came to know that, that was just the key and not actually the password.



Password for level 12 - YWqo0pjpcXzSIl5NMAVxg12QxeC1w9QG