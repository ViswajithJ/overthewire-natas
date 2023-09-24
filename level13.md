<h2>🔰 Level - 13</h2>

- username - natas13
- password - lW3jYRI02ZKDBb8VtQBU1f6eDRo6WEj9

	The site gives an option to upload a jpeg file. Link for sourcecode was also given. <br>
	But this time, they are actually checking if the data uploaded is an image or not. So we can't simply upload a php script.

	On researching how to inject php code within a jpg file, came across this <a href="https://onestepcode.com/injecting-php-code-to-jpg/">link</a><br>

	I added the following php payload as a comment to a random jpg file using the `exiftool` tool.

	```php
	<?php system('cat /etc/natas_webpass/natas14') ?>
	```

	Command used: `exiftool -comment="<?php system('cat /etc/natas_webpass/natas14') ?>" images.jpg`

	Then I uploaded this file, changed the extension of the filename in inline php generated file name(similar to previous level) and got the file link. On opening the link, a random bunch of non-readable characters(that of jpg part of the file), along with password was displayed.

Password for level 14 - qPazSJBmrmU7UQJv17MHk1PGC4DxZMEP