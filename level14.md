<h2>🔰 Level - 14</h2>

- username - natas14
- password - qPazSJBmrmU7UQJv17MHk1PGC4DxZMEP
	
	There was two input fields for username and password, and a link to view source code. <br>
	On checking the source code, its seems that an SQL query is executed based on the username and password.<br>
	The php code was set in such a way that if this query returns more than 0 rows as result, password for next level is displayed.<br>
	Username and password were directly added to the query without any precautions. This implies SQL injection possibilities. Tried some usual SQLi payloads for the username input, as it comes first in the query.

	Payload - `" OR 1=1 #` (this worked)
	
Password for level 15 - TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB