# SSL CHEAT SHEET
#### This will create the private key (private/cakey.pem) and the public key (cacert.pem, a.k.a. the certificate) of the root CA.
````
openssl req -new -x509 -extensions v3_ca -keyout private/cakey.pem -out cacert.pem -days 365 -config ./openssl.cnf
````

#### Create the Server CSR cert (certificate sign request). This will create the CSR for the server (server-req.pem) and the server’s private key (private/server-key.pem)
````
openssl req -new -nodes -out server-req.pem -keyout private/server-key.pem -days 365 -config openssl.cnf
````
#### The Certificate Authority signs the server’s CSR. This will create the server’s public certificate (server-cert.pem):
````
openssl ca -out server-cert.pem -days 365 -config openssl.cnf -infiles server-req.pem
````

#### This will create the client’s CSR (client-req.pem) and the client’s private key (private/client-key.pem):
````
openssl req -new -nodes -out client-req.pem -keyout private/client-key.pem -days 365 -config openssl.cnf
````

#### The Certificate Authority signs the client’s CSR. This will create the client’s public certificate (client-cert.pem):
````
openssl ca -out client-cert.pem -days 365 -config openssl.cnf -infiles client-req.pem
````

#### (Optional) create the PKCS12 file using the client’s private key, the client’s public cert and the CA cert. This will create client-cert.p12:
````
openssl pkcs12 -export -in client-cert.pem -inkey private/client-key.pem -certfile cacert.pem -name "Client" -out client-cert.p12
````
#### One way SSL auth (Client verifies server's certificate):
````
1. Client sends ClientHello message proposing SSL options.
2. Server responds with ServerHello message selecting the SSL options.
3. Server sends Certificate message, which contains the server’s certificate.
4. Server concludes its part of the negotiation with ServerHelloDone message.
5. Client sends session key information (encrypted with server’s public key) in ClientKeyExchange message.
6. Client sends ChangeCipherSpec message to activate the negotiated options for all future messages it will send.
7. Client sends Finished message to let the server check the newly activated options.
8. Server sends ChangeCipherSpec message to activate the negotiated options for all future messages it will send.
9. Server sends Finished message to let the client check the newly activated options.
````
Configuration files usually needed:
* SSLCertificateFile server-cert.pem
* SSLCertificateKeyFile server-key.pem


#### Two way SSL auth ( Client verifies server and vice versa):

````
1. Client sends ClientHello message proposing SSL options.
2. Server responds with ServerHello message selecting the SSL options.
3. Server sends Certificate message, which contains the server’s certificate.
4. Server requests client’s certificate in CertificateRequest message, so that the connection can be mutually authenticated.
5. Server concludes its part of the negotiation with ServerHelloDone message.
6. Client responds with Certificate message, which contains the client’s certificate.
7. Client sends session key information (encrypted with server’s public key) in ClientKeyExchange message.
8. Client sends a CertificateVerify message to let the server know it owns the sent certificate.
9. Client sends ChangeCipherSpec message to activate the negotiated options for all future messages it will send.
10. Client sends Finished message to let the server check the newly activated options.
11. Server sends ChangeCipherSpec message to activate the negotiated options for all future messages it will send.
12. Server sends Finished message to let the client check the newly activated options.
````
Configuration files usually needed:
* SSLCertificateFile server-cert.pem
* SSLCertificateKeyFile server-key.pem
* SSLCACertificateFile cacert.pem


#### Troubleshoot SSL connectivity
````
openssl s_client -connect localhost:443 -tls1 -cert client-cert.pem -key private/client-key.pem
````