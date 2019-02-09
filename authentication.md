# Authentication

The goal for authentication mechanism is verifying identity of the user on the other side of secure connection.  

This protocol:
- would allow online systems to identify same user connecting over multiple connections (i.e. devices) at possibly different times
- would not allow authentication from one service to be used anywhere else
- would not provide any user identifying information without user consent
- would not allow different services to compare user identifiers to find matching users using both services
- would provide offline identity verification; ability of service server to verify user identity without contacting any other entity
- optional: would allow services to request and retrieve personal/company certificate

## Proposal

Actors:
- DRS (digital representation server)
- user device
- service (the server of service user is using)

Requirements:
- DRS must have static IP address
- DRS must have large number of users to ensure privacy (see below)

Set up:
- establish or register at the DRS
- once for every device user owns DRS would generate a key and transfer it to the device (via any method, preferably QR code)


Authentication process:
1. user request a resource/action within of the service
2. the service deternines that authentication is needed and requests identification from user device
3. after user consent, user device requests service bound token from DRS
4. DRS issues a SB token to the user device
5. user device sends SB token and DRS IP address to the service 
6. the service sends SB token to DRS
7. DRS authenticates SB token and returns TTL for the issued authentication
8. service then grants authentication to the user for all their future requests until TTL of the autentication has passed.
9. after that service has to request authentication again (using SB token)

### Observations

1. SB tokens must be unique within DRS
2. DRS can issue SB token to different services all within same namespace
3. combination of SB token and DRS IP address is unique user indentfier service can use
4. The security of the whole process is bound to the security of the SB token. If the service discloses its the SB tokens to a third-party only account at that particual service is at risk.
5. Service can use any approach to link future user requests to the authentication process. Issuing a JWT token to user device is recommended. 
6. DRS can revoke user device key at any given moment, thus preventing it from requesting any new SB tokens from DRS.
7. DRS can revoke any SB token, thus frozing corresponding service account in at most TTL time.
8. If DRS has only few accounts at some service, this service could:
   - compare their user's DRS IP address with some other service and match user's accounts using both services thus tracking user accros the internet.
   - detect that a user has duplicate accounts at the same service. This may be privacy concern is some cases.

### Possible improvements

#### Negating effects of SB token leaks

Hashing the SB token at the service server as soon as the user device submits it as a form of indetification.

This would almost negate security risks emposed by leak of hashed SB tokens and DRS IP address, since hashed SB tokens would not be useful to attackers in any way. Cracking the hashed SB tokens would also be harder that cracking passwords because dictionary attacks would have no significant effect on SB tokens.