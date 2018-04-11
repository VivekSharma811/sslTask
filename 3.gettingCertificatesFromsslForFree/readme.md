# 3. Getting certificates from ssl for free

## a) Obtaining certificates
   * Go to sslforfree.com
   * Select manual obtaining of certificates
   * Now in your folder create a folder name '.well-known'
   * Create another folder name 'acme-challenge'
   * Paste the downloaded file from sslforfree in acme-challenge

## b) Creating the Simulink
Without it we cannot verify our domain name

```bash
sudo ln -s /home/ubuntu/openFireNew/.well-known/ /var/www/html
```  

## c) Verify the website and copy the certificates

 >.crt and .pem extension are interchangable
 
  * private.key
  * certification.crt
  * ca_bundle.crt 

## d) Converting the primary.key to rsa key

```bash
   openssl rsa -in /Users/zakirsaifi/Downloads/private.key -out /Users/zakirsaifi/Downloads/privateNoPass.key
```