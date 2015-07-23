# aws
## Upload SSL certificate to Elastic Load Balancer

* configure AWS tool `aws configure`
* format the pem file `openssl rsa -in server.key -out pem_server.key`
* Upload the Certificate `aws iam upload-server-certificate --server-certificate-name mySSL --certificate-body file://server.crt  --private-key file://pem_server.key`
* 
## Upload SSL certificate to Cloudfront

* `apt-get install python python-pip` --- install the python and pip
* `pip install awscli` ---install the awscli tool
* `aws configure` --- configure AWS tool
* `aws iam upload-server-certificate --server-certificate-name example.com --certificate-body file:///etc/ssl/example.com.crt --private-key file:///etc/ssl/example.com.key --path /cloudfront/example.com/`
* Make sure `file:///` and `--path /cloudfront/example.com/` like this

## AWS EBS disk resize 

* `sudo resize2fs /dev/xvdr` --- after mount the size need to incrase on EBS volume

