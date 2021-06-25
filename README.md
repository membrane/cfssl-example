#cfssl Usage Demo

## Workflows
### CA: generate own cert
```
cfssl gencert -initca ca-csr.json | cfssljson -bare ca -
```

### server: generate own CSR 
```
cfssl genkey server.json | cfssljson -bare server
```

### CA: sign server's CSR
```
cfssl sign -config=ca-config.json -profile=server -csr=server.csr -ca=ca.pem -ca-key=ca-key.pem | cfssljson -bare server
```

### client+CA: generate client's CSR & sign client's CSR 
```
cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=client client.json | cfssljson -bare client
```


## Viewing
### PEM key
```
openssl rsa -in ca-key.pem -text
```

### PEM cert
```
openssl x509 -in ca.pem -text
```

## Conversions

### PEM cert to DER
```
openssl x509 -in ca.pem -inform PEM -out ca.crt -outform DER
```

### PEM key+cert to PKCS12
```
openssl pkcs12 -export -out client-key.pfx -inkey client-key.pem -in client.pem
```

## Notes

* Do not issue certificates for 'localhost'.
* Do not issue certificates for '127.0.0.1'.