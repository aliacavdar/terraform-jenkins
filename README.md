
### This module is used for creating Jenkins environment
## Prerequisites
1. Terraform 0.11.14

### Steps

```
git clone https://github.com/hakten/infrastructure.git
cd infrastructure/jenkins/
ls configurations
```

### Region
Choose the region you would like to work with. In my case I chose to work with us-east-1. However this is not required you can choose any region. Change below items according to your own AWS account


```
vi configurations/YOUR_REGION/jenkins.tfvars

s3_bucket                       =   "YOUR_BUCKET"         
s3_folder_region                =   "YOUR_BUCKET_REGION"               
vpc_id                          =   "YOUR_VPC_ID"            
zone_id                         =   "YOUR_Z32OHGRMBVZ9LR"       
domain                          =   "YOUR_DOMAIN"
region                          =   "YOUR_REGION"
```





### Environment Setup
Once above changes are done save the file and run 
```
source setenv.sh configurations/YOUR_REGION/jenkins.tfvars
```

It will set a proper backend.tf file for us. Next run (by changing region of course):


```
terraform apply -var-file configurations/YOUR_REGION/jenkins.tfvars
```



### If you only want to create jenkins_master, target below resources

module.jenkins_master.aws_ami.centos
module.jenkins_master.aws_iam_instance_profile.jenkins_admin_profile
module.jenkins_master.aws_iam_role.jenkins_admin
module.jenkins_master.aws_iam_role_policy.jenkins_admin_policy
module.jenkins_master.aws_instance.jenkins_master
module.jenkins_master.aws_key_pair.jenkins
module.jenkins_master.aws_route53_record.jenkins_master
module.jenkins_master.aws_security_group.allow_ssh_and_jenkins
module.jenkins_master.null_resource.jenkins_passwd

terraform apply --target=module.jenkins_master.aws_ami.centos --target=module.jenkins_master.aws_iam_instance_profile.jenkins_admin_profile --target=module.jenkins_master.aws_iam_role.jenkins_admin --target=module.jenkins_master.aws_iam_role_policy.jenkins_admin_policy --target=module.jenkins_master.aws_instance.jenkins_master --target=module.jenkins_master.aws_key_pair.jenkins --target=module.jenkins_master.aws_route53_record.jenkins_master --target=module.jenkins_master.aws_security_group.allow_ssh_and_jenkins --target=module.jenkins_master.null_resource.jenkins_passwd
