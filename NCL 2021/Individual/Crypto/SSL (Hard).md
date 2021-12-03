# SSL (Hard)

## Flavor Text

We've obtained several SSL certificates from our servers, but one seems to not trusted by our root certificate. Can you find which one it is?

## Solve

### What is the Common Name in the root certificate? (15 pts)

```openssl x509 -noout -subject -in rootCA.crt```

> cityinthe.cloud

### What is the Common Name in the servers' certificates? (15 pts)

```openssl x509 -noout -subject -in cert1.crt```

> liber8.cityinthe.cloud

### What is the full filename of the invalid certificate? (50 pts)

```for i in *; do openssl verify -CAfile rootCA.crt $i ; done```

> cert62.crt