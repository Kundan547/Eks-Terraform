# Terraform Configuration for EKS Cluster with Fargate and Node Groups

This repository contains Terraform configurations for deploying an AWS Elastic Kubernetes Service (EKS) cluster with support for Fargate profiles and EC2 node groups. The configuration also sets up the necessary VPC, subnets, IAM roles, and networking resources.

## Prerequisites

Before you begin, ensure that you have the following:

- Terraform v1.4.0 or later.
- AWS CLI installed and configured with appropriate permissions.
- An AWS account with sufficient privileges to create EKS clusters and associated resources.

## Files Overview

- **`provider.tf`**: Configures the AWS provider.
- **`variables.tf`**: Defines input variables and their defaults.
- **`main.tf`**: Sets up the EKS cluster and associated IAM roles.
- **`vpc.tf`**: Creates the VPC, subnets, route tables, and NAT gateway.
- **`nodegroup.tf`**: Configures the EC2-based node group for the EKS cluster.
- **`fargate.tf`**: Sets up Fargate profiles for the EKS cluster.

## Features

- **EKS Cluster**:
  - Creates an EKS cluster with support for both Fargate and EC2-based nodes.
  - Configures IAM roles and policies for the cluster and node groups.

- **Fargate Profiles**:
  - Enables pods to run on AWS Fargate in specific namespaces (e.g., `kube-system` and `default`).

- **Node Group**:
  - Configures an EC2-based node group with scaling options.

- **Networking**:
  - Creates a VPC with public and private subnets.
  - Sets up an Internet Gateway and a NAT Gateway for secure networking.
  - Associates subnets with appropriate route tables.

## Usage

1. Clone this repository and navigate to the directory:

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Initialize Terraform:

   ```bash
   terraform init
   ```

3. Customize input variables (if needed) in `variables.tf` or by creating a `terraform.tfvars` file.

4. Validate the configuration:

   ```bash
   terraform validate
   ```

5. Apply the Terraform plan:

   ```bash
   terraform apply
   ```

   Confirm the action by typing `yes` when prompted.

6. After the deployment is complete, you can retrieve the kubeconfig file to interact with your cluster using `kubectl`:

   ```bash
   aws eks update-kubeconfig --region <region> --name <cluster-name>
   ```

## Variables

The following variables are defined in `variables.tf`:

| Variable         | Type   | Default       | Description                         |
|------------------|--------|---------------|-------------------------------------|
| `region`         | string | `us-east-1`   | AWS region for deployment.          |
| `cidr_block`     | string | `10.10.0.0/16`| CIDR block for the VPC.             |
| `tags`           | map    | Predefined    | Tags to apply to all resources.     |
| `eks_version`    | string | `1.31`        | Version of the EKS cluster.         |
| `cluster_name`   | string | `demo-eks-cluster` | Name of the EKS cluster.       |

## Outputs

This configuration outputs the following:

- EKS Cluster endpoint.
- IAM Role ARNs for the cluster and node groups.
- Subnet IDs.

## Notes

- Ensure that the subnets you create are tagged with `kubernetes.io/cluster/<cluster-name>=owned` for proper EKS functionality.
- Customize the scaling and update configurations in `nodegroup.tf` based on your workload requirements.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
