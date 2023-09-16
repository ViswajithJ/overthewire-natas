<h2>🔰 Level - 5</h2>

- username - natas5
- password - Z0NsrtIkJoKALBCLi5eqFfcRN82Au2oD

	Site just stated : 'Access disallowed. You are not logged in'
	I intercepted the request in Burpsuite. On refreshing, the new request included a cookie : `loggedin` with value set to `0`
	I edited that to `1` in the intercept mode and made the request. Sure enough there was the reply stating "Access granted" and the password for level 6.

Password for level 6 - fOIvE0MDtPTgRhqmmvvAOt2EfXR6uQgR