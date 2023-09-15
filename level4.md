<h2>🔰 Level - 4</h2>

- username - natas4
- password - tKOcJIbzM4lTs8hbCmzn5Zr4434fGZQm

	Text given : 'Access disallowed. You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"' and a link to Refresh the page.

	It meant that the request should look like it came from natas5.natas.labs.overthewire.org. 
	I clicked the Refresh link.
	Then I used burpsuite to intercept the `GET` request and modified the `Referer` header's value from `http://natas4.natas.labs.overthewire.org` to `http://natas5.natas.labs.overthewire.org/`

	Note: When you click a link, the Referer contains the address of the page that includes the link.

Password for level 5 - Z0NsrtIkJoKALBCLi5eqFfcRN82Au2oD