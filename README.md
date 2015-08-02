# AWS EC2 Linux  various command and tricks 
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

## AWS EBS disk resize on ubuntu 

I got issue after a backup disk volume increase size it shows the same size as before so here is solution for that, lets suppose your disk name is /dev/xvdr which you can check from this command `fdisk -l`

* `sudo resize2fs /dev/xvdr` --- after mount the size need to incrase on EBS volume

## Add the SWAP Disk to AWS Ec2 ubuntu 14.04

* `sudo dd if=/dev/zero of=/mnt/swapfile bs=1M count=2048` --- create a file with 2 GB size
* `sudo chown root:root /mnt/swapfile` ----give that file to root ownership
* `sudo chmod 600 /mnt/swapfile` --- change permission on the file
* `sudo mkswap /mnt/swapfile` --- make this file to a swapfile system
* `sudo swapon /mnt/swapfile` ---- add to swap
* `/mnt/swapfile swap swap defaults 0 0` --- make a entry in the /etc/fstab
* `swapon -a` --- on the swap
