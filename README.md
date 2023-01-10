# Aws-Highly-Available-Application-Architecture

## Introduction

> ## Diagram link

```
https://lucid.app/lucidchart/25bac433-d042-43bd-a90d-56f33a62dfc9/edit?invitationId=inv_b10355e9-2d62-4f93-974e-fa3e0a1a9dd6
```

This is a simple architecture that can be used for highly available application to deploy in aws.
The components used for this application are VPC, subnets, EC2 instances, Internet Gateway, security groups, public and private subnets, and Application Load Balancer.

> ## VPC

> logical and isolated section of the AWS cloud in which to launch resources. This allows you to have full control over the network configuration, including IP address range, subnets, routing tables, and network gateways. Using a VPC, you can launch resources in a virtual network that closely resembles a traditional on-premises network.

The VPC's IP address range is specified in CIDR notation (e.g. 10.0.0.0/16) and defines the range of IP addresses that are available for use within the VPC. This IP address range is divided into smaller blocks called subnets, which are used to organize and segment the network.

> ## Subnets

> The purpose of subnets in a VPC is to segment the network into smaller, more manageable sections. This allows you to control access to resources in the VPC, improve security, and increase availability.

- There are two main types of subnets that are used in a VPC: public and private subnets.

- Public subnets are subnets that have a direct route to the Internet via an Internet Gateway. Resources launched in a public subnet are typically those that need to be accessible from the Internet, such as web servers or application servers.

- Private subnets are subnets that do not have a direct route to the Internet. Resources launched in a private subnet are typically those that do not need internet access, such as databases or backend systems. Access to resources in a private subnet is typically controlled through a network address translation (NAT) gateway or a VPN connection.

- The main difference between public and private subnets is the level of access to the internet. Public subnets are directly accessible via internet while private subnets do not have direct internet access but can connect to the internet via NAT gateway. Public subnets are more exposed to internet and have higher security risk while private subnets have more isolation and security, but less flexibility.

By creating a public and private subnet, you can create a secure, segmented network environment that improves security and availability of your resources.

> ## Elastic Compute Cloud (EC2)

> Amazon Elastic Compute Cloud (EC2) instances are virtual servers that are launched on the AWS cloud. They are used in the architecture to run applications and services.

EC2 instances are used in the architecture because they provide a flexible and scalable way to run compute resources on the AWS cloud. They can be easily created, configured, and scaled as needed, allowing you to quickly adapt to changing business requirements.

- EC2 instances can be launched in either public or private subnets, depending on the specific requirements of your architecture.

When EC2 instances are launched in public subnets, they are directly accessible from the Internet via an Internet Gateway. These instances typically run web servers, application servers, or other resources that need to be accessible from the Internet.

When EC2 instances are launched in private subnets, they do not have a direct route to the Internet. To connect to the Internet or other resources in public subnet, these instances typically use a NAT gateway or VPN connection. These instances typically run databases or backend systems that do not need internet access.

Launching instances in public and private subnets allows for a more secure and highly available architecture by isolating instances that do not require internet access from the public internet and by providing a direct route to the internet for instances that require it.

> ## Internet Gateway (IGW)

> An Internet Gateway (IGW) is a VPC component that allows communication between resources in your VPC and the Internet. It provides a way for resources in your VPC to connect to the Internet, and for Internet traffic to reach resources in your VPC.

An Internet Gateway is used in the architecture to provide a way for resources in a public subnet to communicate with the Internet. This allows resources such as web servers and application servers to be accessible from the Internet, which is necessary for many types of online services and applications.

An Internet Gateway is attached to a VPC by creating an Internet Gateway resource and then attaching it to the VPC using the Amazon VPC console, the AWS CLI, or the SDKs. Once attached to the VPC, the Internet Gateway creates a route in the VPC route table that points all Internet-bound traffic to the Internet Gateway.

Once the Internet Gateway is attached to the VPC and the necessary routes are in place, resources in the public subnet can communicate with the Internet. Traffic sent to the Internet is sent through the Internet Gateway and then forwarded to the Internet via the Internet service provider (ISP) associated with the VPC. Similarly, traffic sent from the Internet to the VPC is directed to the Internet Gateway and then routed to the appropriate resources within the VPC based on the VPC's routing table.

To allow communication between the VPC and the internet, the Internet Gateway needs to be properly configured and attached to the VPC, and the appropriate routes needs to be set up in the VPC route table to direct internet-bound traffic to the Internet Gateway.

> ## Security groups

> Security groups in AWS are a virtual firewall for your Amazon Elastic Compute Cloud (EC2) instances. They control inbound and outbound traffic to one or more instances by allowing or denying traffic based on a set of rules.

Security groups are used in the architecture to control access to resources and improve security. They can be used to restrict access to specific ports and IP addresses, and to control which resources can communicate with each other. This allows you to segment your network and control access to resources, helping to protect them from unauthorized access or malicious activity.

Each security group has a set of rules that specify which inbound and outbound traffic is allowed or denied. These rules can be used to allow or deny traffic based on various criteria such as IP address, port, and protocol. For example, you could create a security group that only allows traffic on port 80 (HTTP) from specific IP addresses, and denies all other traffic.

When an EC2 instance is launched, it is associated with one or more security groups. These security groups are used to control the inbound and outbound traffic to the instances. The rules defined in the security group control which incoming traffic is allowed to reach the instance, and which outgoing traffic is allowed to leave the instance. Incoming traffic that is not explicitly allowed by the security group's rules will be blocked, and outgoing traffic that is not explicitly allowed will be blocked.

Security groups can be used in a highly available architecture to control access to resources and improve security by limiting the traffic that is allowed to reach the resources, and by controlling the traffic that is allowed to leave the resources. They provide an essential layer of security, which can be configured based on the specific needs of the architecture.

> ## Public and private subnets

> Public and private subnets are used in a VPC to segment the network into smaller, more manageable sections and to control access to resources.

Public subnets are intended for resources that need to be directly accessible from the Internet, such as web servers or application servers. These subnets have a direct route to the Internet via an Internet Gateway, which allows resources in the public subnet to communicate with the Internet.

On the other hand, Private subnets are intended for resources that do not need Internet access, such as databases or backend systems. These subnets do not have a direct route to the Internet, so resources in the private subnet must use a NAT gateway or VPN connection to communicate with the Internet.

Instances are launched in public subnets when they need to be accessible from the Internet and in private subnets when they do not need internet access. For example, a web server that needs to be publicly accessible would be launched in a public subnet, whereas a database that only needs to be accessible from within the VPC would be launched in a private subnet.

Public and private subnets are used to control inbound and outbound traffic in the architecture by routing the traffic through different network paths. Traffic bound for the Internet goes through the Internet Gateway, while traffic bound for the private subnet goes through a NAT Gateway or VPN connection. This allows you to control access to resources and improve security by limiting the traffic that is allowed to reach the resources, and by controlling the traffic that is allowed to leave the resources.

It should also be noted that instances in private subnet can communicate with each other in the same subnet, while instances in public subnet can communicate with both the internet and instances in the same subnet. Additionally, instances in public subnet can be accessed remotely by a ssh session or RDP session, while instances in private subnet can only be accessed using VPN or Direct Connect.

> ## Application Load Balancer (ALB)

> An Application Load Balancer (ALB) is a Layer 7 load balancer that routes incoming HTTP/HTTPS traffic to one or more backend instances. The primary purpose of an ALB is to distribute incoming traffic across multiple instances, improving the scalability and availability of your applications.

> Some of the features of the ALB include:

- Health checks: The ALB can perform health checks on backend instances to ensure that only healthy instances receive traffic.
- SSL/TLS offloading: The ALB can handle SSL/TLS encryption and decryption, allowing you to offload this task from the backend instances.
- Routing: The ALB can route incoming traffic to different backend instances based on the hostname or path in the request.
- Sticky sessions: The ALB can maintain "sticky" sessions, ensuring that requests from a particular user are sent to the same backend instance.

> By distributing incoming traffic across multiple instances, the ALB provides several benefits:

- Scalability: The ALB allows you to scale your applications horizontally by adding or removing instances as needed.
- Availability: The ALB can route traffic to healthy instances, ensuring that your applications remain available even if some instances fail.
- Performance: The ALB can distribute incoming traffic across multiple instances, reducing the load on any one instance and improving the overall performance of your applications.

The Application Load Balancer uses routing algorithms such as Round Robin and Least Outstanding Request, to route the traffic between available instances. This ensures that the load is distributed evenly across the instances, reducing the risk of any one instance becoming a bottleneck.

Additionally, the ALB can perform health checks on the instances, monitoring their availability and routing traffic only to healthy instances. If an instance becomes unavailable, the ALB will stop routing traffic to that instance, ensuring that traffic is directed to available instances, thus providing high availability.

The Application Load Balancer is an essential component in any highly available and scalable architecture, by distributing the incoming traffic among available instances, it provides better performance, scalability and availability.

> ## Route table

> In a VPC, each subnet is associated with a route table that determines where network traffic is directed. The routing table for a public subnet typically includes a route that directs all Internet-bound traffic to an Internet Gateway, allowing resources in the public subnet to communicate with the Internet. The routing table for a private subnet typically does not include a route to the Internet Gateway and may instead include a route that directs all Internet-bound traffic to a NAT gateway, allowing resources in the private subnet to communicate with the Internet while remaining inaccessible from the Internet.

The routing table for a public subnet typically has a default route which directs all internet bound traffic to the Internet Gateway, allowing all the instances in that subnet to communicate with the internet.

The routing table for a private subnet typically has a default route which directs all internet bound traffic to a NAT gateway. This allows instances in the private subnet to initiate outbound traffic to the Internet, but it does not allow inbound traffic from the Internet to reach instances in the private subnet. This makes the instances in the private subnet more secure and less exposed to the Internet.

Additionally, the routing table for a VPC can be configured with custom routes, allowing you to control traffic flow in more complex ways. For example, you can use custom routes to direct traffic between different subnets or to specific instances based on IP addresses or ports.

It is important to note that the routing table is associated with a subnet, if you want to apply the same routing rules to multiple subnets you can do so by associating that routing table with those subnets.
