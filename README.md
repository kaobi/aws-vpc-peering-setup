# Configure AWS VPC Peering Connection

This project illustrates the process of setting up a VPC peering connection between two VPCs in AWS, adjusting route tables to enable seamless communication between them, and conducting a ping test to confirm connectivity.

## Prerequisites

- Two existing VPCs:
  - **VPC A (default-vpc):** 172.31.0.0/16 (AWS Default VPC)
  - **VPC B (test-vpc):** 129.7.0.0/16 (AWS test VPC)
- CIDR blocks must not overlap.

## EC2 Instances

- **default-instance:** Launched in VPC A (default-vpc) in the public subnet.
  - Attach a security group (`default-sg`) with the following rules:
    - Open SSH port (22).
    - Open All ICMP - IPv4 port.
- **test-instance:** Launched in VPC B (test-vpc) in the public subnet.
  - Attach a security group (`test-sg`) with the following rules:
    - Open SSH port (22).
    - Open All ICMP - IPv4 port.
      
![VPC Peering Diagram](https://github.com/user-attachments/assets/5563dad4-4288-45c0-8b27-6f0157cac85c)

## Steps

### Step 1: Create VPC Peering Connection

1. **Create Peering Connection:**
   - Navigate to the VPC Dashboard in the AWS Management Console.
   - Select "Peering Connections" from the left-hand menu.
   - Click "Create Peering Connection".
   - **Requester VPC:** Select VPC B (test-vpc).
   - **Accepter VPC:** Select VPC A (default-vpc).
   - Confirm the peering connection request.

2. **Accept Peering Connection:**
   - In the VPC Dashboard, select "Peering Connections".
   - Find the newly created peering connection.
   - Click "Actions" and select "Accept Request".

### Step 2: Modify Route Tables

1. **VPC A (default-vpc):**
   - Navigate to the Route Tables section within the VPC Dashboard.
   - Select the route table associated with VPC A.
   - Edit the route table to add a route:
     - **Destination:** 129.7.0.0/16 (CIDR block of test-vpc).
     - **Target:** Peering connection ID.

2. **VPC B (test-vpc):**
   - Navigate to the Route Tables section within the VPC Dashboard.
   - Select the route table associated with VPC B.
   - Edit the route table to add a route:
     - **Destination:** 172.31.0.0/16 (CIDR block of default-vpc).
     - **Target:** Peering connection ID.

### Step 3: Ping Test

1. **Connect to EC2 Instances:**
   - Use SSH to connect to `default-instance` in VPC A and `test-instance` in VPC B.

2. **Ping Test:**
   - From `test-instance` (in VPC B), attempt to ping the private IP address of `default-instance` (in VPC A):
     ```sh
     ping <private-ip-of-default-instance>
     ```
   - From `default-instance` (in VPC A), attempt to ping the private IP address of `test-instance` (in VPC B):
     ```sh
     ping <private-ip-of-test-instance>
     ```

