# skypilot-kubernetes-finetuning

This repository contains instructions on how to perform distributed, multi-node fine-tuning of 
Meta-Llama-3.1-8B-Instruct for function calling using Managed Kubernetes and SkyPilot on [Nebius](https://nebius.com/).


## Prerequisites

1. Install the [Nebius AI Cloud CLI](https://docs.nebius.com/compute/quickstart#cli)

2. Clone the nebius-solutions-library repository:
   ```
   git clone https://github.com/nebius/nebius-solution-library.git
   ```

3. Install the following packages to your local machine:
   ```
   kubectl socat netcat
   ```

4. Install SkyPilot with Kubernetes support:
   ```
   pip install "skypilot[kubernetes]"
   ```
   
5. Create a Hugging Face account and obtain developer API token.


### Hardware Prerequisites
```
    2 H100 GPU cards
    32 vCPU
    400GB RAM
    2TB SSD network disk
    2TB SSD shared filesystem
```


## Instructions

### Set up a managed kubernetes cluster

1. Open the nebius-solutions-library repository you just cloned in your favorite editor
    (it will be easier to work in an editor than vim or nano).

2. Navigate to the `k8s-training` directory (this will be our working dir) as it contains everything we need to set up 
our managed cluster:
    ```
    cd nebius-solution-library/k8s-training
    ```
   
3. Open the `environment.sh` file uncomment and set up the following vars:
    ```
    # NEBIUS_TENANT_ID='tenant-...'
    # NEBIUS_PROJECT_ID='project-...'
    # NEBIUS_REGION='eu-north1' # Change the region here if yours is a different one
    ```
   
4. Open the `terraform.tfvars` file uncomment and set up the following vars:
    ```
   ssh_user_name  = "admin" # You can set this based on your preference
    ssh_public_key = {
    path = "~/.ssh/id_rsa.pub"
    }
    ```
   
    In the same file replace the `K8s nodes` block with the following one:
    ```
    # K8s nodes
    cpu_nodes_count = 0
    gpu_nodes_count = 2
    gpu_nodes_preset = "1gpu-16vcpu-200gb"
    enable_k8s_node_group_sa = true
    ```
    
    Also set the `filestore_disk_size` to be 2TB:
    ```
    filestore_disk_size  = 2048 * (1024 * 1024 * 1024)
    ```

5. In `main.tf` find and comment the following line of code:
    ```
    gpu_cluster = nebius_compute_v1_gpu_cluster.fabric_2
    ```

6. Open a terminal and navigate to our working directory `k8s-training`, and run the following commands 
in the provide order:
   ```
   source ./environment.sh
   terraform init
   terraform apply
   ```
   
   On `terraform apply` you will be prompt with the following:
   ```
   Do you want to perform these actions?
   Terraform will perform the actions described above.
   Only 'yes' will be accepted to approve.
   
   Enter a value:
   ```
   Answer by typing `yes` and press `Enter`


## Cluster Setup Complete! 🎉

**Congratulations!** You've successfully initiated your managed Kubernetes cluster setup.

> **Note:** The cluster will be fully operational in approximately 20 minutes.


















