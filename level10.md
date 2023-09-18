<h2>🔰 Level - 10</h2>

- username - natas10
- password - D44EcsFkLxPIkAAKLosx8z3hxX1Z4MCE

	This was really good. Given situation was like the previous level. An input field, where we can enter a word, and the output will display all the occurences of that word within the file called "dictionary.txt".

	But this time, they are filtering the input. If `&`, `|`, or `;` is present, input is considered to be illegal. This effectively prevents normal shell code injections as we used in the previous level.

	So, I randomly tried different inputs for fun. During one of those, I gave a word with a space. This time output changed a bit.
	Usually its just the words that prints out. But this time, the output was `dictionary.txt: web`. That is, each word occurence is preceded by `dictionary.txt: `. This implies that `grep` is searching for the word I passed in some other files as well.
	When I looked up more on grep, came to know that we could search for stuff in more than one file at a time.


	So when I gave the input as : `web pass`, command executed was `grep -i web pass dictionary.txt`.
	<br>
	i.e., grep searches the word "web" in files called `pass` (non-existent) and `dictionary.txt`

	Thus I crafted an input to search in /etc/natas_webpass/natas11 also and gave a common letter as input(pure guess work here)<br>
	Final input: `l /etc/natas_webpass/natas11`<br>
	Final command executed was `grep -i l /etc/natas_webpass/natas11 dictionary.txt`<br>
	This command searches for occurences of the letter "l" in both `/etc/natas_webpass/natas11` and `dictionary.txt`.<br>

	For my luck, the password contained the letter "l". So the output included the password.
Password for level 11 - 1KFqoJXi6hRaPluAmk8ESDW4fSysRoIg