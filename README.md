# Self-hosted Prisma Cloud Compute with k3s Cluster
The contents of this repo allow you to deploy the Prisma Cloud Compute Console as a Docker container, a Kubernetes cluster using k3s, and a Prisma Cloud Defender, all in a single virtual machine that you can run on your computer. The idea is to provide a fully self-contained environment that you can use to test the Cloud Workload Protection funcionalities of Prisma Cloud.

# Requirements
- Multipass
- Linux virtual machine
  - Ubuntu 22.04 with 2 vCPU, 20GB HDD, 4GB RAM
- Tar file for the Prisma Cloud Compute Edition.
  - You can download this file from https://support.paloaltonetworks.com
- Prisma Cloud Compute license

### Download and uncompress the tar file from https://support.paloaltonetworks.com.

### Create VM and mount the folder where the uncompressed version of the tar file is located.

```
multipass launch -c 2 -d 20G -m 4G -n my-cwp-lab --mount "<absolute_path>/prisma_cloud_compute_edition_30_00_140" 22.04
```

### SSH back into my-cwp-lab and navigate to the folder you just mounted.

```
multipass shell my-cwp-lab
```

### Clone this repo to your VM

```
git clone https://github.com/bmppa/pcs-cwp-onebox.git
```

### Install Docker by running the provided script.

```
cd pcs-cwp-onebox
./install_docker.sh
```

### Deploy the Prisma Cloud Compute console.

```
cd /Users/bralmeida/Documents/LAB/PAN/prisma_cloud_compute_edition_30_00_140/
sudo ./twistlock.sh -s console
```

Access the console at https://<IP_address_of_your_vm>:8083. Create an account and provied a license.

### Install k3s

```
curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION="v1.26.1+k3s1" sh -
```

### Deploy defender

```
sudo su
```

Go to Manage > Defenders > Manual deploy and select the following options:
- Method: Orchestrator
- Orchestrator type: Kubernetes
- Select your workstation platform: Linux x86_64
- Advanced Settings > Specify a cluster name: k3s

Copy the install script in the right side of the window and past it in your vm. This will install the defender daemonset. Go back to Manage > Defenders and see if you can see a connected fedener.

### Deploy an application. For example:

```
sudo kubectl run nginx --image=nginx
```
