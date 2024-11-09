# Palo-Alto-Firewall-AWS-Deployment
<h2>Description</h2>
<br/> In this demo we will deploy and configure the Palo Alto firewall VM series in AWS
<br />
<br/> <br/>
<img src="https://github.com/user-attachments/assets/9f7da006-4d76-44c3-845a-ced7418ec642"/>


<h2>Languages and Utilities Used</h2>

AWS Services, Palo Alto VM series

<h2>Environments Used </h2>

- <b>EC2, PAYG </b>

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


## Step 5: Connect to the Palo Alto instance
<br/> Use an ssh client and enter the public IP, the private key, and the username as "admin" (Note: for this demo I am using MobaxTerm for the ssh client) <br/>
<img src="https://github.com/user-attachments/assets/adfabb8d-71c6-49c6-a6f9-e786d3cf805e"/>
<img src=""/>


## Step :
<br/> <br/>
<img src=""/>


## Step : 
<br/> <br/>
<img src=""/>


## Step :
<br/> <br/>
<img src=""/>

