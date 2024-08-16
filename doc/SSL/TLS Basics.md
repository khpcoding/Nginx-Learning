Introduction
TLS stands for Transport Layer Security. It is a protocol that provides privacy and data integrity between two communicating applications. It’s the most widely deployed security protocol used today replacing Secure Socket Layer (SSL), and is used for web browsers and other applications that require data to be securely exchanged over a network.

TLS ensures that a connection to a remote endpoint is the intended endpoint through encryption and endpoint identity verification. The versions of TLS, to date, are TLS 1.3, 1.2, 1.1, and 1.0.

TLS versions

| PROTOCOL | RFC                   | PUBLISHED | STATUS                             |
|----------|-----------------------|-----------|------------------------------------|
| SSL 1.0  |                       | Unpublished| Unpublished                        |
| SSL 2.0  |                       | 1995      | Deprecated in 2011 (RFC 6176) [IETF] |
| SSL 3.0  |                       | 1996      | Deprecated in 2015 (RFC 7568) [IETF] |
| TLS 1.0  | RFC 2246 [IETF]      | 1999      | Deprecation in 2020               |
| TLS 1.1  | RFC 4346 [IETF]      | 2006      | Deprecation in 2020               |
| TLS 1.2  | RFC 5246 [IETF]      | 2008      | Still secure                       |
| TLS 1.3  | RFC 8446 [IETF]      | 2018      | Still secure                       |


what is TLS handshake ? 
The TLS handshake is a crucial process that establishes a secure communication channel between a client (such as a web browser) and a server (such as a web server). This process ensures that the data exchanged between the two parties is encrypted and secure from eavesdropping or tampering.


Steps in the TLS Handshake : 
1. `ClientHello`: The handshake begins when the client sends a "ClientHello" message to the server. This message includes the TLS version supported by the client, a list of cipher suites (encryption algorithms) it supports, and a random number known as "client random.
2. `ServerHello` : The server responds with a "ServerHello" message, which includes the chosen TLS version and cipher suite from the client's list, along with its own random number ("server random") and its digital certificate.
3. `Server Authentication` : The client verifies the server's identity by checking the server's digital certificate against a trusted Certificate Authority (CA). This step ensures that the client is communicating with the legitimate server.
4. `Key Exchange` : The client generates a "premaster secret," encrypts it using the server's public key (obtained from the server's certificate), and sends it to the server. The server then decrypts this premaster secret using its private key.
5. `Session Key Generation` : Both the client and server use the premaster secret, along with the previously exchanged random numbers, to generate a session key. This session key is used for symmetric encryption of the data transmitted during the session.
6. `ChangeCipherSpec` : The client sends a message indicating that it will now use the session key for encrypting further messages.
7. `Finished Messages` : The client sends a "finished" message encrypted with the session key, indicating that its part of the handshake is complete. The server responds with its own encrypted "finished" message, completing the handshake.
8. `Secure Communication` : Once the handshake is complete, the client and server can securely exchange data using the established session key, ensuring confidentiality and integrity of the communication.

![Alt text](https://aboutssl.org/wp-content/uploads/2020/03/ssl-handshake-10-steps.svg)




