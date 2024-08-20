# Install EKSCTL
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

eksctl version

# Install kubectl

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.30.2/2024-07-12/bin/linux/amd64/kubectl

chmod +x ./kubectl

sudo mv kubectl /usr/local/bin/

kubectl version --client

---------------------------
## OIDC Provider

# associate the OIDC provider with your EKS cluster.
eksctl utils associate-iam-oidc-provider --region us-west-2 --cluster quick-cluster --approve

# Download an IAM policy - aws ec2 worker nodes creates load balancer dynamically
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.8.2/docs/install/iam_policy.json

# Create an IAM policy named AWSLoadBalancerControllerIAMPolicy
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json

# Create an IAM role and Kubernetes ServiceAccount for the LBC. Use the ARN from the previous step.
eksctl create iamserviceaccount \
--cluster=quick-cluster \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::992382575331:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region us-west-2 \
--approve

-----------------------------

# HELM installation
1. Download the Helm binary using wget:
wget https://get.helm.sh/helm-v3.15.4-linux-amd64.tar.gz

2. Extract the Binary - tarball
tar -zxvf helm-v3.15.4-linux-amd64.tar.gz

3. Move Helm to a Directory in Your PATH- /usr/local/bin
sudo mv linux-amd64/helm /usr/local/bin/helm

4. Verify the Installation -
helm version



