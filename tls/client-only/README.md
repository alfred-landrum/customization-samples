### Overview

These scripts generate only the client-side certificates, along with their keys and configuration files. Alpine Linux and Mac are both supported in their given folders. If you're having trouble with these scripts in your local environment, take a look at our [Docker Image for generation client-side certificates](https://hub.docker.com/r/temporalio/client-certificate-generation). 

### User Instructions 

2. To generate the root certificate run the `ca.sh` script.
3. To generate an end-entity certificate, run the `end-entity` script.

### Additional Documentation 

x509v3_config:

critical --> ["If critical is present then the extension will be critical."][1]

[v3_ca]

authorityKeyIdentifier = keyid:always,issuer
keyid:always,issuer --> an attempt is made to copy the subject key identifier from the parent certificate. If the value "always" is present then an error is returned if the option fails.
[source][1]

basicConstraints = critical,CA:TRUE,pathlen:0
CA:TRUE --> This is a certificate authority 
pathlen:0 --> "indicates the maximum number of CAs that can appear below this one in a chain. So if you have a CA with a pathlen of zero it can only be used to sign end user certificates and not further CAs."
[source][1]
[source](https://stackoverflow.com/questions/6616470/certificates-basic-constraints-path-length/6617814#6617814)

keyUsage = critical,digitalSignature,cRLSign,keyCertSign
digitalSignature --> Certificate may be used to apply a digital signature
cRLSign --> Subject public key is to verify signatures on revocation information, such as a CRL
keyCertSign --> Subject public key is used to verify signatures on certificates
[source](https://superuser.com/questions/738612/openssl-ca-keyusage-extension)

subjectKeyIdentifier = hash
hash --> will automatically follow the guidelines in RFC3280
[source][1]

### Notes
- The generated certificates are hard-coded as valid for 365 days
- This requires two separate scripts to run to make it possible to both create a variable number of end-entity certificates at any point in time. 
- These certificates should be used only to inform that a connection can be made, and is not to be used in production. 

[1]: https://www.openssl.org/docs/man1.1.1/man5/x509v3_config.html