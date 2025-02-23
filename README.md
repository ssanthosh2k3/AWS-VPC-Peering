# AWS VPC Peering Setup

This guide provides step-by-step instructions to create and configure AWS VPC Peering between two Virtual Private Clouds (VPCs): **Production VPC** and **Staging VPC**.

## Prerequisites
- AWS account with necessary permissions
- AWS CLI or AWS Management Console access

## Steps to Setup VPC Peering

### Step 1: Create VPC 1 (Production VPC)
- CIDR Block: `10.0.0.0/24`

### Step 2: Configure Production VPC
- Create a subnet within `10.0.0.0/24`
- Create and associate a Route Table
- Attach an Internet Gateway (if public access is needed)

### Step 3: Create a Virtual Machine in Production VPC
- Launch an EC2 instance
- Select **Production VPC** in networking settings
- Install Nginx and edit `index.html` for reference:
  ```sh
  sudo apt update && sudo apt install -y nginx
  echo "This is Production VPC" | sudo tee /var/www/html/index.html
  sudo systemctl restart nginx
  ```

### Step 4: Create VPC 2 (Staging VPC)
- CIDR Block: `13.0.0.0/24`

### Step 5: Configure Staging VPC
- Create a subnet within `13.0.0.0/24`
- Create and associate a Route Table
- Attach an Internet Gateway (if public access is needed)

### Step 6: Create a Virtual Machine in Staging VPC
- Launch an EC2 instance
- Select **Staging VPC** in networking settings
- Install Nginx and edit `index.html` for reference:
  ```sh
  sudo apt update && sudo apt install -y nginx
  echo "This is Staging VPC" | sudo tee /var/www/html/index.html
  sudo systemctl restart nginx
  ```

## Create Peering Connection
### Step 7: Initiate Peering Connection
- **Requester:** VPC1 (Production VPC)
- **Accepter:** VPC2 (Staging VPC)
- Accept the peering request from VPC2

### Step 8: Update Route Tables
- **In VPC1:** Add a route to `13.0.0.0/24` via Peering Connection
- **In VPC2:** Add a route to `10.0.0.0/24` via Peering Connection

## Testing Connectivity
- Use **ping** and **curl** commands to test private IP connectivity:
  ```sh
  ping <private-ip-of-staging-vm>
  curl http://<private-ip-of-staging-vm>
  ```
  ```sh
  ping <private-ip-of-production-vm>
  curl http://<private-ip-of-production-vm>
  ```

If everything is set up correctly, the VMs in both VPCs should communicate via their private IPs.

## Conclusion
You have successfully configured AWS VPC Peering between two VPCs, enabling private communication between instances in separate networks.

---

### Author
[Your Name]
