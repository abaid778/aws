# AWS EC2 Linux  various command and tricks 

## Kill RDS Aurora sleep queries 
```
mysql> select concat('CALL mysql.rds_kill( ',id,');') from information_schema.processlist where user='DB_user' and DB='DB_Name';
mysql> CALL mysql.rds_kill(ProcessID);
awk -F "|" '{print $1}' mysql.txt
```

## To change the certificate name for a custom domain name in API Gateway

```
aws apigateway update-domain-name \
    --domain-name api.domain.tld \
    --patch-operations op='replace',path='/certificateArn',value='arn:aws:acm:us-west-2:111122223333:certificate/CERTEXAMPLE123EXAMPLE'
```
## How to mount EBS volume into EC2 Ubuntu 14.04 Instance and Ubuntu 16.04

1: To verify disk attached or not

`fdisk -l`

Last command shows you the Volume ID and Size like this Disk
`/dev/xvdf: 10.7 GB, 10737418240 bytes`

2: make the partition 

`fdisk xvdf`

3: Next command would be this for make ext4 partition

sudo mkfs -t ext4 /dev/xvdf1

4: Now create Directory for mounting point

`mkdir /backup`

5: Now mount the new EBS volume to that Directory

`sudo mount /dev/xvdf1 /backup/`

6: Next Step add the to /etc/fstab

`/dev/xvdf1       /backup   ext4    defaults     0       0`

## How to install PhantomJS 2 EC2 Ubuntu 14.04 Instance

1: SSH to the instance

`cd /usr/local/share`
`sudo wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2`
`sudo tar xjf phantomjs-2.1.1-linux-x86_64.tar.bz2`
`sudo ln -s /usr/local/share/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/local/share/phantomjs`
`sudo ln -s /usr/local/share/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/local/bin/phantomjs`
`sudo ln -s /usr/local/share/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/bin/phantomjs`
`phantomjs --version`
## Upload SSL certificate to Elastic Load Balancer

* configure AWS tool `aws configure`
* format the pem file `openssl rsa -in server.key -out pem_server.key`
* Upload the Certificate `aws iam upload-server-certificate --server-certificate-name mySSL --certificate-body file:///server.crt  --private-key file:///pem_server.key`
* 
## Upload SSL certificate to Cloudfront

* `apt-get install python python-pip` --- install the python and pip
* `pip install awscli` ---install the awscli tool
* `aws configure` --- configure AWS tool
* `aws iam upload-server-certificate --server-certificate-name example.com --certificate-body file:///etc/ssl/example.com.crt --private-key file:///etc/ssl/example.com.key --certificate-chain file:///etc/ssl/gd_bundle-g2-g1.crt --path /cloudfront/example.com/`
* Make sure `file:///` and `--path /cloudfront/example.com/` like this

## AWS EBS disk resize on ubuntu 

I got issue after a backup disk volume increase size it shows the same size as before so here is solution for that, lets suppose your disk name is /dev/xvdr which you can check from this command `fdisk -l`

* `sudo resize2fs /dev/xvdr` --- after mount the size need to incrase on EBS volume

## version delete issue on the Elasticbeanstalk
need to configure the aws profile config
* `mkdir ~/.aws/config`
* `[profile eb-cli]`
* `aws_access_key_id = Access key`
* `aws_secret_access_key = Secert key `
* `eb labs cleanup-versions --num-to-leave 4`
reference : http://www.scriptscoop.net/t/04e7078d79be/cannot-deploy-error-you-cannot-have-more-than-500-application-versions.html


## Add the SWAP Disk to AWS Ec2 ubuntu 14.04

* `sudo dd if=/dev/zero of=/mnt/swapfile bs=1M count=2048` --- create a file with 2 GB size
* `sudo chown root:root /mnt/swapfile` ----give that file to root ownership
* `sudo chmod 600 /mnt/swapfile` --- change permission on the file
* `sudo mkswap /mnt/swapfile` --- make this file to a swapfile system
* `sudo swapon /mnt/swapfile` ---- add to swap
* `/mnt/swapfile swap swap defaults 0 0` --- make a entry in the /etc/fstab
* `swapon -a` --- on the swap

## SSL enable on ubuntu 14.04

* `sudo a2enmod ssl` --- enable mod ssl
* `sudo a2ensite /etc/apache2/sites-available/default-ssl.conf` --- enable the ssl 

## S3 IAM policy 
* Here is the s3 bucket policy with full access of 2 buckets
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets"
            ],
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": "arn:aws:s3:::first-bucket"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::first-bucket/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": "arn:aws:s3:::second-bucket"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::second-bucket/*"
        }
    ]
}
```
## S3 copy unencrypted version to encrypted version

This is bash script which I used to encrypted s3 bucket current unencrypted objects 
```
#!/bin/bash
time_start=`date +%s`

aws s3 cp s3://BucketName s3://BucketName --recursive --sse AES256

time_end=`date +%s`
time_exec=`expr $(( $time_end - $time_start ))`

echo "Execution time is $time_exec seconds"
```
DB-password=Test123
