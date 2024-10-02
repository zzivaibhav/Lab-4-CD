# Deployment Guide for DAL VMs

This guide provides step-by-step instructions for setting up a self-hosted runner and deploying a Java application using Docker on DAL VMs.

## 1. Setting up the Runner

### 1.1 Docker Account Setup
1. Register for a Docker account at [https://app.docker.com/](https://app.docker.com/)
2. Save your Docker credentials (username and password) securely

### 1.2 Repository Secrets
Add the following secrets to your GitHub repository:
- `DOCKER_USERNAME`: Your Docker username
- `DOCKER_PASSWORD`: Your Docker password

### 1.3 VM Setup
Log in to your assigned VM and execute the following commands:

```bash
# Update package lists
sudo apt update

# Install Docker
sudo apt install docker.io -y

# Install Maven
sudo apt install maven -y

# Install Net-Tools
sudo apt install net-tools -y

# Set appropriate permissions for Docker's control socket
sudo chmod 666 /var/run/docker.sock
```
### 1.4 Self-Hosted Runner Setup

1.  Navigate to your GitHub repository settings
2.  Go to "Actions" > "Runners"
3.  Click "New self-hosted runner"
4.  Follow the provided instructions to download and configure the runner on your VM

### 1.5 Start the Runner

Use  `tmux`  to keep the runner running in the background:
```
# Start a new tmux session
tmux

# Start the runner (use the command provided by GitHub)
./run.sh

# Detach from the tmux session (press Ctrl+B, then D)
```

## 2. Workflow Script

Use the workflow script from  [this repository](https://github.com/mehilshah/Lab-4-CD/blob/main/.github/workflows/maven.yml)  as a template for your CI/CD pipeline. The script includes steps for:

-   Building the Java application with Maven
-   Building and pushing a Docker image
-   Pulling the Docker image on the VM
-   Starting the container

## 3. Verification of Deployment

After the deployment is complete, verify the application's accessibility:
```
# Get the VM's IP address
ifconfig

# Look for the 'ens3' interface and note the 'inet' address
```
The deployed application will be available at:  
`http://<VM_IP_ADDRESS>:8090/<endpoint>`

Replace  `<VM_IP_ADDRESS>`  with the actual IP address of your VM, and  `<endpoint>`  with the appropriate application endpoint.
