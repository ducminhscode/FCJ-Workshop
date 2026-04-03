---
title : "Worklog Week 2"
date :  "`r Sys.Date()`" 
weight : 2 
pre: <b> 1.2 </b>
chapter : false
---

### Week 2 Objectives:

- Gain a solid understanding of AWS Networking fundamentals, including the architecture, operational mechanisms, and core components of Amazon VPC.
- Master Hybrid and Multi-VPC connectivity solutions, and practice secure connection methods between on-premises environments and AWS such as VPN, Direct Connect, as well as inter-VPC connectivity through Peering and Transit Gateway.
- Optimize DNS resolution and load balancing, using Route 53 Resolver for hybrid architectures and understanding different Elastic Load Balancer types (ALB, NLB, GWLB) to ensure high availability.
- Study defense-in-depth security architecture for modern services such as serverless and microservices through real-world case studies from AWS Blogs.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Study AWS networking services and how to set up and manage a virtual private networking environment on AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ VPC (Virtual Private Cloud)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Core VPC components: Subnet, Route Table, ENI (Elastic Network Interface)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Internet connectivity: Internet Gateway (IGW), NAT Gateway<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Advanced networking services: VPC Peering, Transit Gateway, VPN, Direct Connect, VPC Endpoint<br>- Understand VPC security and multi-VPC connectivity features:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Security Groups (SG)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Network Access Control Lists (NACL)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ VPC Flow Logs<br>&nbsp;&nbsp;&nbsp;&nbsp;+ VPC Peering<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Transit Gateway (TGW)<br>- Focus on Hybrid connectivity solutions and Elastic Load Balancing services:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ VPN Site-to-Site: Virtual Private Gateway, Customer Gateway<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Client VPN<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Direct Connect<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Application Load Balancer (ALB)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Network Load Balancer (NLB)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Classic Load Balancer (CLB)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Gateway Load Balancer (GWLB)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Health Check<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sticky Session | 02/03/2026 | 02/03/2026 | [AWS Virtual Private Cloud](https://youtu.be/O9Ac_vGHquM?si=_aG8YUGohIVlaDPZ)<br>[VPC Security and Multi-VPC features](https://youtu.be/BPuD1l2hEQ4?si=po48SGsVTvdnuACl)<br>[VPN - DirectConnect - LoadBalancer - ExtraResources](https://youtu.be/CXU8D3kyxIc?si=P7v3T0wo4oYdROvi) |
| Tue | - Hands-on practice: Create the first Amazon VPC and AWS Site-to-Site VPN ([Module 02 - Lab 03](https://drive.google.com/drive/folders/1bAvUbXWHi7QfQw2QimYraeJYIwYDyM8s?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the VPC environment<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Deploy EC2 resources<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure AWS Site-to-Site VPN<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Use Transit Gateway for centralized VPN connection management<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 03/03/2026 | 03/03/2026 | [Amazon VPC and AWS Site-to-Site VPN Workshop](https://000003.awsstudygroup.com/) |
| Wed | - Study and translate the blog **Building an AI-powered defense-in-depth security architecture for serverless microservices**<br>- Analyze the defense-in-depth security architecture for serverless workloads on AWS<br>- Review AWS terminology and refine the translation to ensure technical accuracy and clarity | 04/03/2026 | 04/03/2026 | [Blog 01](https://aws.amazon.com/vi/blogs/security/building-an-ai-powered-defense-in-depth-security-architecture-for-serverless-microservices/) |
| Thu | - Hands-on practice: Build a Hybrid DNS system using Amazon Route 53 ([Module 02 - Lab 10](https://drive.google.com/drive/folders/1s_nuYrVH6RYskrV3jYz513TpLr-Ko4ct?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Environment preparation: Create keypairs, deploy base infrastructure using CloudFormation, configure Security Groups<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Set up a simulated on-premises environment: Connect to RDGW and deploy Microsoft Active Directory<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure Route 53 Resolver: Create Outbound Endpoint, Resolver Rules, and Inbound Endpoint<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Test DNS resolution<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 05/03/2026 | 05/03/2026 | [Set up Hybrid DNS with Route 53 Resolver](https://000010.awsstudygroup.com/) |
| Fri | - Hands-on practice: Configure VPC Peering to allow communication via Private IP ([Module 02 - Lab 19](https://drive.google.com/drive/folders/1xvC3ypW4yMa0oaOL_IKSL3bs6pA3-XwR?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Environment preparation: Use CloudFormation to quickly create VPCs and subnets, configure Security Groups, launch EC2 instances<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Update Network ACL rules (Inbound and Outbound)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Establish VPC Peering Connection: Send and accept peering requests between VPCs<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure Route Tables<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure Cross-Peer DNS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Validate connectivity by pinging between VPCs using Private IP<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 06/03/2026 | 06/03/2026 | [Setting up VPC Peering](https://000019.awsstudygroup.com/) |
| Sat | - Rest and prepare for the upcoming week | 07/03/2026 | 07/03/2026 | |
| Sun | - Rest and prepare for the upcoming week | 08/03/2026 | 08/03/2026 | |

---

### Week 2 Achievements:

- Theoretical Knowledge:
  - Clearly distinguished the functional differences and use cases of various Load Balancer types (ALB, NLB, GWLB) and network security components (Security Group vs NACL).
  - Gained a solid understanding of how Route 53 Inbound and Outbound Endpoints operate in hybrid environments.
  - Learned the deployment workflow of Site-to-Site VPN and the role of Transit Gateway in centralized network management.
- Hands-on Labs:
  - **Lab 03:** Successfully deployed a VPC environment, configured a Site-to-Site VPN, and integrated Transit Gateway for centralized connectivity management.
  - **Lab 10:** Built a complete Hybrid DNS architecture, enabling seamless domain name resolution between simulated on-premises Microsoft Active Directory and AWS.
  - **Lab 19:** Successfully configured VPC Peering, updated routing tables, and enabled Cross-Peer DNS for communication between VPCs using Private IP addresses.
- Research & Analysis:
  - Completed the translation and analysis of an AWS blog on defense-in-depth security architecture for serverless microservices.
  - Organized AWS Networking and Security terminology to ensure accuracy in technical documentation.
- Resource Management:
  - Properly cleaned up AWS resources after each lab session to optimize operational costs and maintain a well-managed cloud environment.