# AWS CLI EC2 Inventory
Export metadata of instances (id, Private IP, Public IP, AMI and Tag Name) of multiple regions and accounts to csv

```
for profile in $(awk -F"[" '{gsub(/\].*/,"",$2);print $2}' ~/.aws/credentials); do  
  for region in \ eu-north-1 \ ap-south-1 \ eu-west-3 \ eu-west-2 \ eu-west-1 \ ap-northeast-2 \ ap-northeast-1 \ sa-east-1 \ ca-central-1 \ ap-southeast-1 \ ap-southeast-2 \ eu-central-1 \ us-east-1 \ us-west-1 \ us-west-2; do
    aws ec2 describe-instances --region $region --profile $profile --query "Reservations[*].{Instance:Instances[0].InstanceId,Image:Instances[0].ImageId,PrivateIP:Instances[0].PrivateIpAddress,PublicIP:Instances[0].PublicIpAddress,AZ:Instances[0].Placement.AvailabilityZone,Account:OwnerId,Name:Instances[0].Tags[?Key=='Name']|[0].Value}" --output text  ; 
  done ; 
done > ec2-inventory.csv
