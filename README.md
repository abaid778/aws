# aws
## Upload SSL certificate to Elastic Load Balancer

* configure AWS tool `aws configure`
* format the pem file `openssl rsa -in server.key -out pem_server.key`
* Upload the Certificate `aws iam upload-server-certificate --server-certificate-name mySSL --certificate-body file://server.crt  --private-key file://pem_server.key`
