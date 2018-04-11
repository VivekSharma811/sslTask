src :-> [https://blog.bigdinosaur.org/openfire-and-ssl-slash-tls-certificates/]
# 4. Updating Certificates on Openfire
1. Run the following commands

```bash
	  sudo /bin/bash
	  cd /etc/openfire/security
```


## 1. Convert the Certificates and private key saved in some location to **_.der_** format

### a). Certificate.crt to  => cert.der

	```
	openssl x509 -in /home/ubuntu/openFireNew/sslbigMem/certification.crt -inform PEM -out ./cert.der -outform DER
	```
### b). private.pem(non-rsa one) or private.crt to => key.der

	 ```
	 openssl pkcs8 -topk8 -nocrypt -in /home/ubuntu/openFireNew/sslbigMem/private.pem -inform PEM -out ./key.der -outform DER
	 ```
### c). ca_bundle.crt to =>  c2-intermed.der	
	
	```bash
	  openssl x509 -in /home/ubuntu/openFireNew/sslbigMem/ca_bundle.crt -inform PEM -out ./c2-intermed.der -outform DER
	```


### d) . Concat the 'cert.der' & 'c2-intermed.der' to 'chaincert.der'  
   ```bash
       cat cert.der c2-intermed.der > chaincert.der
   ```


## 2. Copying the generated certifcates to '/etc/ssl/private' 
  ### a) cert.der and chaincert.der to  "/etc/ssl"
    ```bash
    	sudo cp  /home/ubuntu/openFireNew/sslbigMem/cert.der /etc/ssl/
    ```

    ```
     sudo cp  /home/ubuntu/openFireNew/sslbigMem/chaincert.der  /etc/ssl/
    ```

### b) key.der to  "/etc/ssl/private"
```bash
   	   sudo cp /home/ubuntu/openFireNew/KeyStoreImport.java /etc/openfire/security
  ```

## 3. Copy KeyStoreImport.java to /etc/openfire/security

### a). Run the following command
   ``` java
  		javac -encoding UTF-8 KeyStoreImport.java 
   ```

## 4. Now stop the openfire
 
  ```bash
   /etc/init.d/openfire stop
  ```

## 5. Backup the keystore and truststore

  ```bash
     cp truststore truststore-knowngood
     cp keystore keystore-knowngood
  ```


## 6. Now run the command to list the trust store
You'll be prompted for a password, which by default is `changeit`,

   ```bash 
   keytool -list -keystore truststore     
   ```
   ### a) Now search the following string  in above list by pasting it in sublime text
   >isrg_root_x1   (This is CA name for letsencrypt)
   
 ### b) if not found then 
 1. Download the root.pem file or root CA certificate for letsencrypt
 2. Run the following command *if string not found*
    
  ``` bash
       /usr/bin/keytool -import -keystore /etc/openfire/security/truststore -alias bigmem.raxa.ninja  -file /home/ubuntu/openFireNew/root.pem
  ```
  3. Check the **CA** name in truststore using command 
       You'll be prompted for a password, which by default is `changeit`,
      ```bash
      keytool -list -keystore truststore   
     ```


## 7. Run command for listing the truststore and password is changeit

    ```bash
      keytool -list -keystore keystore     
    ```

### a) Delete previous keystore entries using their alias name
   
   ```bash
		keytool -delete -keystore keystore -alias <alias of first entry>
		keytool -delete -keystore keystore -alias <alias of second entry>
   ```

### b) import the certificate and key 

   ```bash
   java KeyStoreImport keystore /etc/ssl/chaincert.der /etc/ssl/private/key.der "alias-you-want-for-this-entry"
  ```
  eg
   ```bash
    java KeyStoreImport keystore /etc/ssl/chaincert.der /etc/ssl/private/key.der "bigmemChat"
  ```

## 8. Start openfire

   ```bash
    /etc/init.d/openfire start
   ```    

###  a) Log in to https://<servername>:9091 and check it.