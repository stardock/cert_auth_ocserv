# cert_auth_ocserv
Set up Certificate Authentication in OpenConnect VPN Server (ocserv)

## Setting up Your Own CA (Certificate Authority)  
```
certtool --generate-privkey --outfile ca-privkey.pem
nano ca-cert.cfg
```

```
# X.509 Certificate options

# The organization of the subject.
organization = "vpn.example.com"

# The common name of the certificate owner.
cn = "Example CA"

# The serial number of the certificate.
serial = 001

# In how many days, counting from today, this certificate will expire. Use -1 if there is no expiration date.
expiration_days = -1

# Whether this is a CA certificate or not
ca

# Whether this certificate will be used to sign data
signing_key

# Whether this key will be used to sign other certificates.
cert_signing_key

# Whether this key will be used to sign CRLs.
crl_signing_key
```

`certtool --generate-self-signed --load-privkey ca-privkey.pem --template ca-cert.cfg --outfile ca-cert.pem`

## Generating Client Certificate
```
certtool --generate-privkey --outfile client-privkey.pem
nano client-cert.cfg
```

```
# X.509 Certificate options
# The organization of the subject.
organization = "vpn.example.com"

# The common name of the certificate owner.
cn = "John Doe"

# A user id of the certificate owner.
uid = "username"

# In how many days, counting from today, this certificate will expire. Use -1 if there is no expiration date.
expiration_days = 3650

# Whether this certificate will be used for a TLS server
tls_www_client

# Whether this certificate will be used to sign data
signing_key

# Whether this certificate will be used to encrypt data (needed
# in TLS RSA ciphersuites). Note that it is preferred to use different
# keys for encryption and signing.
encryption_key
```

`certtool --generate-certificate --load-privkey client-privkey.pem --load-ca-certificate ca-cert.pem --load-ca-privkey ca-privkey.pem --template client-cert.cfg --outfile client-cert.pem`  

`certtool --to-p12 --load-privkey client-privkey.pem --load-certificate client-cert.pem --pkcs-cipher aes-256 --outfile client.p12 --outder`  



Ref:  
https://www.linuxbabe.com/ubuntu/certificate-authentication-openconnect-vpn-server-ocserv  
https://stackoverflow.com/questions/21141215/creating-a-p12-file  
