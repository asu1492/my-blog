
In the **native / database login mode** of Manhattan Active:
1. Client calls **token endpoint** (OAuth2) with `username` + `password` (Resource Owner Password Credentials or similar direct login flow).
2. **Auth Server / Access Management**:
    - Delegates identity check to **Organization component** (user table) for that username.
    - Org validates the stored password hash.
3. If Org returns “user is valid & active”, the Auth Server:
    - Issues a **JWT access token** (and optionally ID/refresh tokens) with claims derived from Org (user_name, authorities, orgs, etc.)
4. Downstream components (Zuul, microservices) validate that JWT on every request
5. **Org** as the source of truth for _who you are_ and _what you can do where_, and **Auth Server / AM** as the thing that turns that into a JWT.
6. Org entities also checks user's Role and based on role, grants provided to the users. 

```
{{authurl}}/oauth/token?grant_type=password&username=&password=
Response 
access_token
token_type
refresh_token
expires_in
scope
userOrgs
edge
organization
userLocations
accesstoAllBUs
tenantId
contractTypes
locale
excludedUserBusinessUnits
userDefaults
userBusinessUnits
userTimeZone
jti
```



7. do not use password secrets to validate signature. Sign the token with a private key. The client will validate the signature using the corresponding public key available at the jwks URI and identified by a key id (kid). The key should be cached client side.