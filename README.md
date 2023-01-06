# aws-eks-app-deployment

## Steps to deploy application

1. Create the underlying infrastructure (VPC, subnets, route tables, internet gateway, nat gateway, security groups).
2. Create EKS cluster role with policies - AmazonEKSClusterPolicy, AmazonEKSVPCResourceController.
3. Create EKS node group role with policies - AmazonEKSWorkerNodePolicy, AmazonEC2ContainerRegistryReadOnly, AmazonEKS_CNI_Policy.
4. Create the EKS cluster.
5. Create the node group.
6. Apply the deployment.yaml kubectl config.

## Steps to load balance (internet facing) the above application

1. Tag the public subnets with Tag name: kubernetes.io/role/elb; Tag value: 1.
2. Create IAM OIDC identity provider for your cluster.
3. Create load balancer controller policy and load balancer trust policy.
4. Create load balancer controller role.
5. Create service account.
6. Add eks-charts helm repository and update.
7. Install load balancer controller using helm.
8. Apply the ingress.yaml kubectl config.

## Some helpful commands

1. To set kube config context

aws eks update-kubeconfig --region us-east-1 --name clusterName

2. To apply kubectl config

kubectl apply -f config.yaml

3. Add helm repo

helm repo add eks https://aws.github.io/eks-charts

4. helm install load balancer

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=clusterName \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller








