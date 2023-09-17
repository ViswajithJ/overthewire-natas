<h2>🔰 Level - 9</h2>

- username - natas9
- password - Sda6t0vkOPkM8YeOZkAGVhFoaplvlJFd
	
	A really nice one.
	There was an input field and a link to view sourcecode. But this time, they want us to input a word and they will search for all occurences of that word in a `dictionary.txt` file. In the sourcecode file, we could see that it runs the `grep` command on given word using `passthru()` function in php.<br>

	On looking up `passthru()`, its something like `system()` in python which executes commands passed to it.
	So, instead of giving a simple word as input, we should carefully craft an input such that it would execute the command we need in their shell remotely, without crashing the program itself.

	The command they were running: 
	```php
	passthru("grep -i $key dictionary.txt");
	```
		were `$key` stores our input.

	To try out this method, I made an input which is :	`needle dictionary.txt;ls; grep -i needle`
	`;` will complete the first command, then executes `ls` and then later adds `grep -i needle` to match the end part of added string within passthru function's argument. 
	On giving this input, output was, set of words with "needle" in them, then the contents of the directory, followed by the words with "needle" again. This shows that command injection is working.

	Since we know the password for level 9 will be in `/etc/natas_webpass/natas10`, we can `cat` it out with appropriate input.
	Final input : `needle dictionary.txt;cat /etc/natas_webpass/natas10; grep -i needle`
	On execution, final command rendered will be:
	`grep -i needle dictionary.txt;cat /etc/natas_webpass/natas10; grep -i needle dictionary.txt`

Password for level 10 - D44EcsFkLxPIkAAKLosx8z3hxX1Z4MCE