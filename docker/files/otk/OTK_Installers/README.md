## Add solution kit files here

### Add file - OAuthSolutionKit-4.4.00-4346.sskar

### Ensure JDBC and Cassandra bundles have been loaded and DB schema is imported

### Execute OTK install via cURL
```
curl -u admin:CAdemo123 --insecure -X POST -k -H 'Content-Type: multipart/form-data' --form entityIdReplace=4432207d16a1b505e8a6ed59993eaa24::175ace8f404dd47c8cefe0a762271542 --form entityIdReplace=c2e0825b2f52dc7819cd6e68893df156::8e7af8f5fe78af7719574812da0b3c8e --form "file=@//Users/aricday/Projects/PublicGithub/otk-gateway/docker/files/otk/OTK_Installers/OAuthSolutionKit-4.4.0-4346.sskar" -s -D - https://localhost:8443/restman/1.0/solutionKitManagers
```
