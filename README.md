# awsclikubectl-image
Docker image based on Alpine Linux and additional installed tools to manage applications on an Amazon EKS cluster.

## Included Tools

 - awscli https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html
 - kubectl - https://kubernetes.io/docs/reference/kubectl/overview/
 - eksctl - https://eksctl.io/
 - git - https://git-scm.com/download/linux

 
## Example Usage

```sh
# (OPTIONAL) unset your AWS CLI env vars
unset AWS_ACCESS_KEY_ID AWS_SECRET_KEY_ID AWS_SECURITY_TOKEN AWS_SESSION_TOKEN

# Read your env vars from an artifact eg creds.sh
source ./creds.sh

# Retrieve your "Docker login" from Amazon ECR
TOKEN=`aws ecr get-login-password --region ${CLUSTER_REGION}`

# Setup kubectl config for your EKS cluster
kubectl delete secret --ignore-not-found gitlab-registry
kubectl create secret docker-registry gitlab-registry --docker-server=https://${AWS_ACCOUNT_ID}.dkr.ecr.${CLUSTER_REGION}.amazonaws.com --docker-username=AWS --docker-password="${TOKEN}" -o yaml --dry-run=client | kubectl apply -f -

# Start deploying
kubectl apply -f example-app.yaml

```