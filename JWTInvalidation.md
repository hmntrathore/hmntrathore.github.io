## Invalidating JWT with Azure API Management (APIM)

Imagine a world where you can seamlessly and securely exchange information between parties without compromising on data integrity or confidentiality. JSON Web Tokens (JWT) make this vision a reality. JWTs are compact, URL-safe means of representing claims to be transferred between two parties. They are widely used across the industry for authorization and information exchange, making them an essential tool in the modern developer's arsenal. In this blog post, we will delve deep into what JWTs are, how they work, and why they are invaluable in ensuring secure communication.

## Background Information
### 
What is a JSON Web Token (JWT)?

A JSON Web Token (JWT) is a compact, URL-safe token that is used to represent claims between two parties. These claims are encoded as a JSON object and can be digitally signed or encrypted to ensure the integrity and confidentiality of the data. JWTs are commonly used for authorization purposes, enabling secure and efficient communication between a client and a server.\n\n### Structure of a JWT

A JWT consists of three parts:

1. **Header**: Contains metadata about the token, such as the type of token (JWT) and the signing algorithm used.
2. **Payload**: Contains the claims, which are statements about an entity (typically the user) and additional data.\n3. **Signature**: Ensures the token's integrity by verifying that the payload has not been tampered with.\n\nThese three parts are encoded using Base64Url and concatenated with dots (.) to form a JWT, which looks like this:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### How JWT Works ###
1. **Creation**: A server generates a JWT when a user logs in. The server creates the header, payload, and signature, and sends the token back to the client.
2. **Storage**: The client stores the JWT, usually in local storage or a cookie.
3. **Sending Requests**: The client includes the JWT in the Authorization header of subsequent HTTP requests to the server.
4. **Validation**: The server validates the JWT by checking the signature and ensuring it has not expired. If valid, the server processes the request; otherwise, it rejects the request.


##  Enhancing JWT Security: Strategies for Invalidation and Mitigation

JWTs are designed to be **stateless**, allowing users to utilize them until their expiration or until their signatures are verified correctly by the server. This design eliminates the need for persistent storage (like a database) to manage issued tokens, which streamlines authorization processes and improves efficiency.

However, a significant drawback of JWTs is their **inability to be invalidated once issued**. This means that once a JWT is created and until it expires, it remains valid for accessing resources. This poses security concerns, especially when users log out or if tokens are compromised. Many customers have expressed the need for a solution to ensure that JWTs become inactive immediately upon user logout to mitigate security risks. Audit teams also highlight this as a potential vulnerability.

Despite the these challenges, traditional JWT mechanisms do not provide a direct method to invalidate tokens once they are issued. This limitation has prompted discussions and brainstorming sessions to explore alternative approaches to enhance JWT security and mitigate risks associated with unauthorized access.

Unfortunately, with JWT, there is no direct approach to invalidate the token, with brainstorming we identified the following approach:
#### 1. Delete/Invalidate Token in Browser

One approach to mitigate the risk of active JWTs post-logout is to delete or invalidate tokens directly in the user's browser:

- **Implementation**: Upon user logout, send an invalidation request to delete the JWT stored in the browser.
- **Limitations**: While effective for immediate sessions, it doesn't prevent misuse if the JWT has been captured or stored elsewhere.

#### 2. Reducing Expiry Time

- **Implementation**: Shorten the expiry time of JWTs to minimize the window of opportunity for potential misuse.
- **Considerations**: While reducing the expiry time helps, it doesn't address the need for immediate invalidation upon logout or compromise.

#### 3. Server-Side Token Management

To address the limitations of client-side token handling, consider server-side token management:

- **Implementation**: Store JWTs on the server and maintain an active/inactive status for each token.
- **Benefits**: Allows for immediate invalidation upon logout or detection of unauthorized access attempts.
- **Challenges**: Requires additional infrastructure and introduces complexity in managing token states.

  

### Leveraging Azure API Management for Enhanced Security

Azure API Management provides robust features to manage JWT lifecycles, particularly focusing on immediate invalidation upon user logout:

- **Logout API Integration**: Upon user logout, a "Limit call rate by key" policy in APIM increments the JWT credentials count by 1. Subsequent API calls are then subject to a rate limit set to 1 call, effectively invalidating the JWT.
  
  - **Policy Configuration**:
    - **Key**: JWT is used as the key for rate limiting.
    - **Calls**: Limit is set to 1 to restrict further API access.
    - **Increment Condition**: Enabled (`true`) to increase count upon logout.
  
  ![Logout Policy](/logout.svg)

- **Validation on API Calls**: For ongoing validation, a global or higher-level "Limit call rate by key" policy checks the JWT status with no increment on validation requests.
  
  - **Policy Configuration**:
    - **Key**: JWT is used as the key for rate limiting.
    - **Calls**: Limit is set to 1 to ensure single execution per token.
    - **Increment Condition**: Disabled (`false`) to maintain count without modification during validation.
  
  ![Validation Policy](/validate.svg)

#### Benefits and Considerations

##### Pros of APIM Approach:
- **Built-in Functionality**: Leverage APIM's native capabilities without extensive custom coding.
- **Scalability**: Handle enterprise-level loads efficiently with Azure's robust infrastructure.
- **Low Latency**: Introduce minimal additional latency by enforcing policies within APIM.

##### Cons:
- **API Dependency**: All API interactions must route through APIM to enforce policies effectively.
- **Platform Dependence**: Requires integration with Azure ecosystem for seamless management and scalability.

#### Conclusion

While JWTs offer efficiency and scalability in modern authentication mechanisms, their stateless nature poses challenges for immediate invalidation upon logout or compromise. By implementing thoughtful strategies and leveraging Azure API Management's capabilities, organizations can mitigate these risks effectively. Whether through client-side actions like token deletion or server-side enhancements using APIM, securing JWTs requires a layered approach tailored to organizational security requirements.


For further insights into implementing JWT security strategies and utilizing Azure API Management features, refer to the [Azure API Management documentation](https://learn.microsoft.com/en-us/azure/api-management/).


#### Reference 
- [JWT](https://www.rfc-editor.org/rfc/rfc7519)
- [APIM Policy](https://learn.microsoft.com/en-us/azure/api-management/api-management-access-restriction-policies#LimitCallRateByKey)


