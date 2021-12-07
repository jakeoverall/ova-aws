From VirtualBox to AWS
======================

[![https://i.ytimg.com/vi/Jmjcl-MWC6w/hqdefault.jpg](https://i.ytimg.com/vi/Jmjcl-MWC6w/hqdefault.jpg)](https://www.youtube.com/watch?v=Jmjcl-MWC6w)

[Related Article](https://aws.amazon.com/premiumsupport/knowledge-center/import-server-ec2-instance/)

1. Follow the guidelines in [Required configuration for VM export](https://docs.aws.amazon.com/vm-import/latest/userguide/vmie_prereqs.html#prepare-vm-image).

2. [Install the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) on an on-premises client and [configure it with the AWS credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) generated for the VM import user.

3. [Create a new S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the same AWS Region where you plan to run your EC2 instance
 
4. Run the following bash command
  - > `aws iam create-role --role-name vmimport --assume-role-policy-document ./trust-policy.json`
       
5. Update `role-policy.json` with your bucketname then run the following command
  - > `aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy-document ./role-policy.json`

6. [Upload the image to the S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) with the tool of your choice.

7. Edit `containers.json` to include your S3 bucket and key then run the following command
 - > `aws ec2 import-image --description "DESCRIPTION_HERE" --license-type byol --disk-containers ./containers.json` 

8. From the client machine, run the AWS CLI command [import-image](https://docs.aws.amazon.com/cli/latest/reference/ec2/import-image.html).
 
9. To check the import task status, run the AWS CLI command [describe-import-image-tasks](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-import-image-tasks.html).
 - > `aws ec2 describe-import-image-tasks --import-task-ids import-ami-TASK_ID` 

10. After the image is imported as an AMI, follow the instructions for [Launching an Instance using the Launch Instance Wizard](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/launching-instance.html#launch-instance-console).

* * *

Related information
-------------------

[Import your VM as an image](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html#import-vm-image)

[Creating an IAM user in your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)

[Programmatic access](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)

[Amazon Machine Images (AMI)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)
