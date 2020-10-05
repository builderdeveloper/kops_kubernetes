# kops_kubernetes

(Perform below tasks on centos7 instance on aws)

1. Download below kops link and select kops-linux-amd64
   https://github.com/kubernetes/kops/releases/tag/v1.16.2

2. - mv kops-linux-amd64 /usr/local/bin/kops
   - chmod +x /usr/local/bin/kops

3. kops create cluster --help (To check command related to kops)

3. yum install awscli -y

4. create IAM role with
      - AmazonEC2FullAccess
      - IAMFullAccess
      - AmazonS3FullAccess
      - AmazonRoute53FullAccess
   and then attach it to centos7 instance
5. Now create aws s3 bucket which stores kops cluster state
   - aws s3api create-bucket --bucket dev.k8s.rajesh.com  

   Now will do versioning of bucket, because incase if we upgrade cluster version and later it has some glitches so we can roll back to previous version
   - aws s3api put-bucket-versioning --bucket dev.k8s.rajesh.com  --versioning-configuration    Status=Enabled

   Now we are going to export kops state environment variable
   - export KOPS_STATE_STORE=s3://dev.k8s.rajesh.com

6. Create route53 dns with name rajesh.com which should be private 

7. Now create cluster using kops refer to point 3

   aws ec2 describe-availability-zones --region ap-south-1      // to check AZ


   - kops create cluster --name k8s.rajesh.com --zones us-east-1a --master-size t2.micro --node-size t2.micro --master-count 1 --node-count 2 --master-volume-size 15 --node-volume-size 12 --dns-zone=rajesh.com --dns private

