# Palo-Alto-Firewall-AWS-Deployment
<h2>Description</h2>
<br/> In this demo we will deploy and configure the Palo Alto firewall VM series in AWS
<br />
<br/> <br/>
<img src="https://github.com/user-attachments/assets/9f7da006-4d76-44c3-845a-ced7418ec642"/>


<h2>Languages and Utilities Used</h2>

AWS Resources, Palo Alto VM series

<h2>Environments Used </h2>

- <b> AWS </b>

<h2>Project walk-through:</h2>
<br/>
<p align="center">


### **Prerequisites**  
- Have an [AWS account](https://aws.amazon.com/console/).   
- Acess to a managerial account to access the [AWS Marketplace for the Palo Alto VM series](https://aws.amazon.com/marketplace/pp/prodview-mn63yjbq37n4c).

## Step 1: Create VPC
<br/> Navigate to the AWS console and create a VPC. <br/>
<img src="https://github.com/user-attachments/assets/d4793a17-95e0-47b6-a25f-e97eedf53b64"/>
<img src="https://github.com/user-attachments/assets/224cba1e-ddd7-4603-90a8-ad3725390bdd"/>

## Step 2: Create 3 subnets
<br/> Subnets will allow us to segregate traffic between different resources and control them using route tables. We need to create 3 different subnets to be our management, public, and private subnet. <br/>
<br/> When creating the subnets choose the vpc that was just created: <br/>
<img src="https://github.com/user-attachments/assets/94f354d4-ba6a-4357-8fb4-49073fbacb4f"/>
<br/> Name the subnet, choose a large Availability Zone, and give it a large IP for this demo and future scalability <br/>
<img src="https://github.com/user-attachments/assets/eef72a5b-0f52-47b9-8754-0be379adbc3d"/>
<img src="https://github.com/user-attachments/assets/3036cbb2-6472-4b33-b728-d15897705154"/>
<br/> Repeat the process for the Private and Management subnet <br/>
<img src="https://github.com/user-attachments/assets/06fcd897-d6c4-4ca8-9731-7e80b334fd36"/>



## Step 3: Create route tables

<br/> We will need to create 2 different route tables to control our traffic between the PA Firewall and our private network <br/> 

<br/> Name the public and private route table and choose the VPC for both <br/>
<img src="https://github.com/user-attachments/assets/8acdb9cc-abe2-4ca8-a33f-7ada415b2f54"/>
<img src="https://github.com/user-attachments/assets/5fcc6438-e039-4c31-a48f-c39d42b6b2e5"/>
<br/> Add the following route to the public route table to be able to access the internet <br/>
<img src="https://github.com/user-attachments/assets/6034dabe-1802-404a-9c2a-6c19e318e34b"/>
<br/> Note: for this you will need an Elastic IP and an internet gateway <br/>
<img src="https://github.com/user-attachments/assets/513a7a74-dc41-49a2-a19a-0a7cb8ec0a25"/>
<img src="https://github.com/user-attachments/assets/b213d6e8-72f6-458d-a29d-81acb63eb20e"/>
<br/> Next add the subnets to the public route table <br/>
<img src="https://github.com/user-attachments/assets/a95b6fda-5c27-40c4-9fdd-b7ba0c413072"/>
<br/> Now the firewall will be able to control the data traffic between the virtual machines and the internet through the interface <br/>
<br/> Next lets associate the private subnet witht the private route table <br/>
<img src="https://github.com/user-attachments/assets/90d35746-118e-4be6-9337-e4ef3560eca0"/>
<br/> Now we have all of our VPC requirements ready next we can move onto EC2 <br/>

## Step 4: Lanuch the Palo Alto EC2 instance 
<br/> Navigate to the EC2 resource and go to launch an instance with the Palo Alto VM series AMI <br/>
<img src="https://github.com/user-attachments/assets/4a75a777-2e49-4023-b6ba-15ae2bae5289"/>
<img src="https://github.com/user-attachments/assets/24f8e259-298e-4d74-bbe3-17551691b9c1"/>
<br/> Next configure the instance size, keypair, subnet, IP, and VPC. Make sure to select the management subnet to follow the documentation order to create the instance. <br/>
<img src="https://github.com/user-attachments/assets/7052d47e-642f-4c0b-9ef7-38c1244022c8"/>
<br/> leave the following for demo purposes <br/>
<img src="https://github.com/user-attachments/assets/d4a34a42-9532-4835-bb9f-983d4283dda4"/>
<br/> Now we can finally connect to our EC2 instance. Any error messages that appear after connect is pressed mean that its possible the route table does not have an IP so retrace the first steps. Now click on connect  <br/>


## Step 5: Configure the password
<br/> Use an ssh client and enter the public IP, the private key, and the username as "admin" (Note: for this demo I am using MobaxTerm for the ssh client) <br/>
<img src="https://github.com/user-attachments/assets/adfabb8d-71c6-49c6-a6f9-e786d3cf805e"/>
<br/> Now let's configure the admin password <br/>

```Bash
# Enter configuration mode
configure
# Change admin password
set mgt-config users admin password
# Save configuration
commit
```

<img src="https://github.com/user-attachments/assets/996099b5-e6f3-4399-bd52-1718fdeafae6"/>



## Step 6: Connect to the management console
<br/> Now enter https://the-public-ip of the management subnet into a browser and login <br/>
<img src="https://github.com/user-attachments/assets/2de19887-441c-4ebf-bb12-d4b4336a91cc"/>
<br/> If there is a "warning connection is not private message" click advanced and proceed <br/>
<img src="https://github.com/user-attachments/assets/ccec5c3d-89a1-4b04-9eb0-9f424e76fb28"/>
<img src="https://github.com/user-attachments/assets/980ba748-9ae1-4012-acd8-789ec56dc155"/>
<br/> The management IP address is visible and it matches the one in AWS <br/>
<img src="https://github.com/user-attachments/assets/79ad2e89-3011-4ca2-a18d-d7f2eccdd7c9"/>


## Step 7: Create additional interfaces
<br/> Navigate back to AWS and rename the management interface <br/>
<img src="https://github.com/user-attachments/assets/790bf5d2-4b48-42d1-b456-f019be7ff35e"/>
<br/> Now we will create addition interfaces to attach to the virtual machine. Assign each interface to the related subnet and recommended security group. <br/>
<img src="https://github.com/user-attachments/assets/702b7185-5ca0-4691-b17f-c41241bde04d"/>
<br/> Rename <br/>
<img src="https://github.com/user-attachments/assets/6dc8d782-e1c8-45ed-a7cf-e0ee3591a0a0"/>
<br/> Each network interface must be attached to the Palo Alto firewall. To do this click actions, attach, then select the subnet and Palo Alto instance. <br/>
<img src="https://github.com/user-attachments/assets/f878a9e7-d7d1-4bef-995a-5f2f9ef55bba"/>
<br/> The next step is to disable the source/destination check. This is just a security check put in place by AWS to make sure only certain traffic is hitting an interface by making sure the ip address of the interface itself is either the source ip or the destination ip in the packet. This is important to disable because if a packet hit the firewall with a source ip and destination ip that do not match then it will be blocked automatically. Source destination checks should almost always be disabled when deploying firewalls in AWS for this reason. <br/>
<br/> Click actions, choose source/dest check, and uncheck the box <br/>
<img src="https://github.com/user-attachments/assets/cc110205-fe33-4023-9e6c-6b0921c733c7"/>
<br/>  <br/>
<img src=""/>

## Step 8:
<br/> <br/>
<img src=""/>

