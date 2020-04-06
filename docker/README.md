# Gateway Install w/PAPIM - OTK - MAG

### <a name="configuration"></a>Configuration:

These steps will setup an environment to showcase the Layer7 gateway. The default FQDN references in the tutorial will be **gw2.10.aricday.net**  for the MAS/OTK external services.  The FQDNs point to your Docker Engine IP address so update your local */etc/hosts* file accordingly.

Default environment variables are in the [.env](.env) file that is automatically read from Docker Compose. Custom environment variables are exported from the Makefile for every *make* command vi the [.custom.env](.custom.env) file. These custom environment variables include application specific hostnames, certificates, and user credentials:

## <a name="installation"></a>Installation:

### <a name="bootstrap"></a>Bootstrap: 

Start the main application via the **make** command. The default option is the *run* command (docker-compose -f docker-compose.yml up -d --force-recreate). You can tail the logs in this terminal via *make log* or open a new one (docker-compose -f docker-compose.yml logs -f)
```
	make
```

## <a name="OTK"></a>OTK:

```
    #call the RESTMAN API to install OTK
    $ curl -u $SSG_ADMIN_USERNAME:$SSG_ADMIN_PASSWORD --insecure -X POST -k -H 'Content-Type: multipart/form-data' --form entityIdReplace=4432207d16a1b505e8a6ed59993eaa24::175ace8f404dd47c8cefe0a762271542 --form entityIdReplace=c2e0825b2f52dc7819cd6e68893df156::8e7af8f5fe78af7719574812da0b3c8e --form "file=@files/otk/OTK_Installers/OAuthSolutionKit-4.3.1-4141.sskar" -s -D - https://localhost:8443/restman/1.0/solutionKitManagers
    
    HTTP/1.1 100 Continue

    HTTP/1.1 200 OK
    Content-Type: text/plain
    Content-Length: 32
    Date: Mon, 06 Apr 2020 17:48:34 GMT
    Server: Layer7-API-Gateway
    
    ---
    #call the RESTMAN API to install MAG
    curl -u $SSG_ADMIN_USERNAME:$SSG_ADMIN_PASSWORD --insecure -X POST -k -H 'Content-Type: multipart/form-data' --form entityIdReplace=cbeedf1e2650e80f4d660a7fdb8fea13::175ace8f404dd47c8cefe0a762271543 --form entityIdReplace=5a97f3bdcbc049ccaebbe9ccb8379ccb::8e7af8f5fe78af7719574812da0b3c8e --form "file=@files/otk/MAG_Installers/MAGSolutionKit-4.2.00-2716.sskar" -s -D - https://localhost:8443/restman/1.0/solutionKitManagers
    ---
```

## <a name="development"></a>Development:

### Custom certificates:

Create custom x509 server certificates and keys for the MAS/OTK services. This requires **openssl** to be installed on your local system.

Create the self-signed certificate for the MAS/OTK using 'mas.docker.local' as the subject and SAN

	openssl req -new -x509 -days 730 -nodes -newkey rsa:4096 -keyout config/certs/mas.key -subj "/CN=gw2.10.aricday.net" -config <(sed 's/\[ v3_ca \]/\[ v3_ca \]\'$'\nsubjectAltName=DNS:gw2.10.aricday.net/' /etc/ssl/openssl.cnf) -out config/certs/mas.cert.pem

Convert the PEM certificate to PKCS12:

	openssl pkcs12 -export -clcerts -in config/certs/mas.cert.pem -inkey config/certs/mas.key -out config/certs/mas.cert.p12


## <a name="Teardown"></a>Teardown:

### <a name="clean-all"></a>clean-all: 

Stop the application stack via the **make** command. The default option is the *clean* command (source .custom.env; docker-compose -f docker-compose.yml stop && docker-compose -f docker-compose.yml rm -f && docker volume prune -f). 

```
  make clean
```
