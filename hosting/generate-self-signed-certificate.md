
## Generate Self-signed SSL

### 1. Generate new private rsa key

Encrypted with aes256. Saved in "ca-key.pem" with 4096 bits encr

```bash
openssl genrsa -aes256 -out ca-key.pem 4096
```

### 2. Generate corresponding ca certificate

We use the created key. Valid for 3650 days. Saved in ca.pem



```bash
openssl req -new -x509 -sha256 -days 3650 -key ca-key.pem -out ca.pem
```

For FDQN, it's recommended to use the ipaddress

You can view the generate cs certificate using

#### a) Openssl:
```bash
openssl x509 -in ca.pem -text
```


#### b) Linux nano command

```bash
sudo nano ca.pem
```

### 4. Generate new rsa key for the certificate

This should not be encrypted because it should be accessible to all clients.

output file: cert-key.pem

```bash
openssl genrsa -out cert-key.pem 4096
```

### 5. Generate a certificate of signed request.

The certificate needs to be signed by a CA, else it won't be valid.
The -subj can be anything, domain,ip,etc in the format ```"/CN=value,..."```

output file: cert.csr

```bash
openssl req -new -sha256 -subj "/CN=yourcn" -key cert-key.pem -out cert.csr
```

### 6. Create a config file for ip address

The certificate needs to be signed by a CA, else it won't be valid.
subjectAltName could be domain resolving to the ip or ip, comma separated
in the format ```"subjectAltName=DNS:some_domain,IP:some_ip,*some_wildcard,...```

Save this in a file, like ```extfile.cnf```


```bash
echo "subjectAltName=DNS:example.com,IP:0.0.0,*.example.com,..." >> extfile.cnf
```

### 7. Generate the certificate from certificate signed request

Output: ```cert.pem```


```bash
openssl x509 -req -sha256 -days 3650 -in cert.csr -CA ca.pem -CAkey ca-key.pem -out cert.pem -extfile extfile.cnf -CAcreateserial
```

### 8. Combine the CA and certificate files into a fullchain

This is important
Output: ```fullchain.pem```


```bash
cat cert.pem >fullchain.pem
cat ca.pem >> ./fullchain.pem
```





- 
- 

You can check out [Here]()  for some






