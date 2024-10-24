```markdown
# EKS Cluster Setup with Cluster Autoscaler

This guide covers setting up an EKS (Elastic Kubernetes Service) cluster with autoscaling capabilities using `eksctl` and AWS IAM roles. We'll create a cluster, define minimum and maximum node limits, and configure a cluster autoscaler to dynamically manage nodes based on resource demand.

## Step 1: Creating the EKS Cluster

While creating the cluster, you can decide whether you need autoscaling based on the minimum and maximum nodes. This is controlled by specifying the `--nodes-min` and `--nodes-max` parameters. For this guide, we'll use `eksctl` to create a cluster:

```bash
eksctl create cluster \
  --name ecommerce-cluster \
  --region ap-south-1 \
  --nodegroup-name t2-medium-nodegroup \
  --node-type t2.medium \
  --nodes 2 \
  --nodes-min 2 \
  --nodes-max 4 \
  --managed
```

- **Cluster Name**: `ecommerce-cluster`
- **Region**: `ap-south-1`
- **Node Group Name**: `t2-medium-nodegroup`
- **Node Type**: `t2.medium`
- **Node Count**: Minimum of 2 and a maximum of 4 nodes to allow autoscaling

## Step 2: When and Why to Use Cluster Autoscaler

Horizontal Pod Autoscaler (HPA) is a great tool for scaling the number of pods based on CPU or memory usage. However, there are limitations since HPA scales pods within the existing nodes only. When traffic surges, or the number of API calls and computations increase, it may hit resource limits on the nodes, causing pods to enter a "Pending" state due to insufficient resources.

This is where **Cluster Autoscaler** comes in. It automatically increases or decreases the number of nodes in the cluster based on the current pod scheduling requirements. 

## Step 3: Deploy Cluster Autoscaler

Within the cluster, you'll need to apply a `cluster-autoscaler.yml` file. This file includes:

- **Deployment**: Defines the Cluster Autoscaler deployment specifications
- **RoleBinding** and **ClusterRoleBinding**: Manage permissions for the autoscaler
- **Role** and **ClusterRole**: Define necessary permissions for managing node scaling
- **ServiceAccount**: Allow Cluster Autoscaler to perform actions in the cluster

You can find a sample YAML file for Cluster Autoscaler in the official [Cluster Autoscaler GitHub Repository](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler). 

## Step 4: Configure IAM Role for Node Scaling

In AWS, the cluster nodes need permissions to interact with the Auto Scaling service. You need to:

1. Go to the **IAM** service in the AWS console.
2. Navigate to the **Roles** section.
3. Search for your **Node Group name** (`t2-medium-nodegroup`).
4. Attach the `AutoScalingFullAccess` policy to the role associated with your node group. This permission is crucial for allowing the cluster to scale nodes as needed.

## Step 5: Create a Load Test

After configuring the cluster and setting up the Cluster Autoscaler, you can run a load test to verify autoscaling. You can create a script that sends a large number of requests to your pods to simulate high traffic. This will trigger the Cluster Autoscaler to provision more nodes if necessary.

---

By following these steps, you'll have a scalable EKS cluster that automatically adjusts node count based on traffic and resource demands. This setup ensures efficient utilization of resources while maintaining the performance of your applications.
```