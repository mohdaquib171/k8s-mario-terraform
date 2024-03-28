# Deploying Super Mario on Kubernetes

In this guide, I am deploying the beloved Super Mario game on Amazon's Elastic Kubernetes Service (EKS). By leveraging Kubernetes, we can orchestrate the deployment on AWS EKS, ensuring scalability, reliability, and easy management.

## Prerequisites

- An Ubuntu Instance
- IAM role
- Terraform installed on the instance
- AWS CLI and Kubectl installed on the instance

## Let's Deploy!

### Step 1: Launch Ubuntu Instance

Create an EC2 instance and connect to it using SSH.

### Step 2: Create IAM Role

Create an IAM role with Administrator Access and attach it to the EC2 instance.


### Step 3: Cluster Provision

1. **Clone this Repo.**
   ```shell
   git clone https://github.com/mohdaquib171/k8s-mario-terraform.git
   ```
2. **Change directory.**
   ```shell
   cd k8s-mario-terraform
   ```
3. **Provide the executable permission to script.sh file and run it.**
   ```shell
   sudo chmod +x script.sh
   ./script.sh
   ```
   This script will install Terraform, AWS CLI, Kubectl, and Docker.
4. **Check versions.**
   ```shell
   docker --version
   aws --version
   kubectl version --client
   terraform --version
   ```
5. **Change directory into the EKS-TF and run Terraform init.**
   ```shell
   cd EKS-TF
   terraform init
   ```
6. **Run Terraform validate and Terraform plan.**
   ```shell
   terraform validate
   terraform plan
   ```
7. **Run Terraform apply to provision the cluster.**
   ```shell
   terraform apply --auto-approve
   ```
8. **Update the Kubernetes configuration.**
   ```shell
   aws eks update-kubeconfig --name EKS_CLOUD --region ap-south-1
   ```
9. **Change directory back to k8s-mario.**
   ```shell
   cd ..
   ```

### Deployment and Service

1. **Apply the deployment and service.**
   ```shell
   kubectl apply -f deployment.yaml
   # Check the deployment
   kubectl get all
   ```
2. **Apply the service.**
   ```shell
   kubectl apply -f service.yaml
   kubectl get all
   ```
3. **Describe the service and copy the LoadBalancer Ingress.**
   ```shell
   kubectl describe service mario-service
   ```
   Paste the ingress link in a browser and start playing the Mario game.

### Destruction

1. **Remove the service and deployment first.**
   ```shell
   kubectl get all
   kubectl delete service mario-service
   kubectl delete deployment mario-deployment
   ```
2. **Destroy the cluster.**
   ```shell
   terraform destroy --auto-approve
   ```
   Resources that are provisioned will be removed after 10 minutes.