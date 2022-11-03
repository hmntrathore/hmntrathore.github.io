# Invalidating JWT with APIM

JSON Web Token (JWT) is widely used for authorization across the industry. A compact, URL-safe means of representing claims to be transferred between two parties.  The claims in a JWT are encoded as a JSON object that is used as the payload of a JSON Web Signature (JWS) structure or as the plaintext of a JSON Web Encryption (JWE) structure, enabling the claims to be digitally signed or integrity protected with a Message Authentication Code (MAC) and/or encrypted.
  
Claim Process works as follows:
-	Users send the credentials to JST Server
-	JWT server issues JWT
-	Users send JWT along with request for authorization

**JWT is stateless**, thereby user can use the token until it has expired(which is set during the token creation time by the server) or signatures are correct. At the server side JWT are verified based on signature, thereby there is no need for a persistent a storage(db.) to store the issued tokens, which made authorization faster and efficient. 
  
The downside of this approach is, until the token is expired, you can access the resources.  Multiple customers have reached out to me  to ensure once the user has logged out, his JWWT should not be active, as active JWT will have security considerations and audit teams have also identified this as a risk. 
  
Unfortunately, with JWT, there is no direct approach to invalidate the token, with brainstorming we identified the following approach:
-	#### Delete/ Invalidate token in Browser: 
on logout we can send invalidate token, so that next request will not work. Let’s say I have stored my older JWT, or someone was snipping to user system he will still have the valid token and can misuse for its designated lifetime
-	#### Reducing the expiry time:  
Can help to an extent but still threat will exist.
-	#### Storing the tokens on server side:
Storing the tokens of server side and marking them active or inactive can help out to mitigate the risk. This will be require and additional layer of code and call to make this functional. We worked on this approach and instead of using database and additional calls used Azure API management native functionality Limit call rate by key to achieve this.
  
### How APIM helps here:
Once the user calls the log out API, his JWT credentials count is increased by 1 , and next time he want to call any other API, rate limit by key triggers as limit is set to 1. 
For other calls before log out, as the increment condition is false  it does not trigger the rete limit by key. 
#### For Enabling At logout: 
On the logout API we enable Limit call rate by key policy, 
-	using JWT as key 
-	limit-by-key calls as 1
-	A True increment condition

 ![Logout Policy](/logout.svg)
 
### For Valitdating:
A Global or higher level Limit call rate by key policy was created , which can be checked at each API calls, using
-	using JWT as key 
-	limit-by-key calls as 1
-	a false increment condition

 ![validating policy](/validate.svg)
 
#### Pros of this Approach:
-	Leveraging APIM functionality no coding is required
-	Can hand Enterprise level load 
-	Minimal addition latency introduced

#### Cons:
-	All the calls should be via API
-	Should be using APIM

#### Reference 
- https://www.rfc-editor.org/rfc/rfc7519
- https://learn.microsoft.com/en-us/azure/api-management/api-management-access-restriction-policies#LimitCallRateByKey


