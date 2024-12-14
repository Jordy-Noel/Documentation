# Azure compute & network services

## Scale VMs in Azure

### Virtual machine scale sets
- create & manage a group of identical load-balanced VM's.
- Azure automates all of the vm configuration, network routing and monitoring (to determine upscaling).
- you can scale on demand or on schedules
- load balancer gets deployed automatically to make sure resrouces are being used efficiently.
- used for large scale services such as : 
    - compute
    - big data
    - container workloads

### Virtual machine availability sets
- ensure that VM's stagger updates and have varied power/network connectivity. This is accomplished by:
    - update domain |
        - groups VM that can be rebooted at the same time.
        - allows updates to only one domain at a time to make sure only one domain is offline at a time.
        - An update domain is allowed 30 minutes to recover before the next domain is updated.
    - fault domain |
        - groups VM by power/network switch
        - by default split up in 3 domains
        - ensure protection against power/network failure.

## Create a linux VM  and install Ngin
Create a virtual machine 

```
az vm create --resource-group "learn-d9b5343e-d8e7-4705-8612-e79b3fe16e8c" --name my-vm --public-ip-sku Standard --image Ubuntu2204 --admin-username azureuser --generate-ssh-keys
```

* The backslash is to indicate a new line. So it's optional

Run a VM extension set command to install Nginx using a bash script. The script is stored in github.

```
az vm extension set --resource-group "learn-d9b5343e-d8e7-4705-8612-e79b3fe16e8c" --vm-name my-vm --name customScript --publisher Microsoft.Azure.Extensions --version 2.1 --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
```

## Azure virtual desktop

### Security
- Centralized security management via MS Entra ID
    - MFA can be enabled
    - secure data via RBAC's
- Data & Apps are seperated from local hardware
    - Desktop & Apps are running in the cloud.
    - Reduced risk of confidential data being left on a personal device.
- Isolated user sessions in single & multi-session environments.

## Azure containers
Containers allow multiple instances of an application on a single host machine. VM's are limited to a single OS / VM.

- VM's virtualize the OS
- Containers virtualize the hardware

### what are containers
- A virtualization environment.

### Azure container instances
- PaaS
- Allows upload of containers
- Service runs containers for you.

### Azure container apps
- Similar to container instance
- PaaS
- Container management piece is removed.
- Able to incorporate load balancing and scaling

### Azure kubernetes service
- container orchestration service
- manages the life cycle of containers

### Containers in your solution

- Used to create solution by using a microservice architecture
    - solutions are broken up in to smaller, independent pieces
        - Website
            - Front-end container
            - Back-end container


## Azure functions
- event driven / serverless compute driven
- no maintenance required

### events
- REST request
- Timer
- Message from an azure service

### Benefits
- Automatic scaling on demand
- Automatic deallocation when service is done running.
- Pay for what you use.

### Stateful/Stateless
- Stateful
    - a context is passed to track prior activity
- Stateless
    - default
    - behave as a restart for any new event

## Azure virtual networking

- isolation & segmentation
- internet communication
- communication between Azure/on-premises resources
- route/filter network traffic
- connect virtual networks
- supports public/private endpoints

### private/public endpoints

- Public| have a public IP that can be accessed from anywhere in the world.
- Private| the endpoint exists whithin a vnet and has a private IP address within the address space of that vnet.

### Isolation and segmentation

- Allows you to create multiple isolated Vnets
    - a private IP address space is defined by using public or private IP address ranges.
        - the IP range only exists whithin the Vnet
        - the IP range isn't internet routable.
    - the IP address space can be divided into subnets.
- Name resolution can be done via the name resolution service built into Azure or you can also configure the Vnet to either use an internal/external DNS server.

### Internet communications

- you can enable incoming connection from the internet by assigning a public IP address to an Azure resource or by putting the resource behind a load balancer.

### communication between Azure resources

You can enable secure communcation between Azure resources in two ways:
- virutal networks can not only connect VM's but also other Azure resources such as Azure kubernetes service & Azure virtual machine scale sets.
- service endpoints can connect to azure resource types such as Azure SQL database & storage accounts. This approach enables you to link multiple Azure resources to virtual networks to improve security and provide optimal routig between resources

### communication with on-premises resources

Azure virtual networks enables you to link resources together between your on-premises environment and in your Azure subscription. In effect you can create a network that spans both your local and cloud environments. In order to do this we need three mechanisms:
- Point-to-site virtual private network connections 
    - a connection from a computer outside your organization back into your corporate network -> client computer initiates an encrypted VPN connection to the Azure virtual network.
- Site-to-site virtual private networks 
    - links your on-premises VPN device or gateway to the Azure VPN gateway in a virtual network. In effect the devices in Azure can appear as being on the local network. The connection is encrypted and works over the internet.
- Azure ExpressRoute
    - provides a dedicated private connectivity to Azure that doesn't travel over the internet. ExpressRoute is useful for environments where you need greater bandwith and even higher levels of security.

### Route network traffic

By default Azure routes traffic between subnets on any connection vnets, on-premises networks and the internet. You can also control routing and override those settings.

- Route tables
    - allows you to define rules about how traffic should be directed.
        - Custom route tables can be created that control how packets are routed between subnets.
- Border gateway protocol 
    - works with Azure VPN gateways, Azure route server or Azure ExpressRoute to propagate on-premises BGP routes to Azure virtual networks.

### Filter network traffic

- Network security groups
    - Azure resources
    - inbound/outbound security roules 
        - allow/block traffic based on factors such as:
            - source/destination IP address
            - port
            - protocol
    
### Connect virtual networks

- Virtual network peering
    - network traffic is private between peered networks
    - travels on the MS backbone network. Never entering the public network.
    - enables resources in each Vnet to communicate with each other.
    - can be in seperate regions.

User defined routes (UDR)| allow control of the routing tables between subnets within virtual networks or between vnets.

## Azure virtual private networks (VPN)

- Uses an encrypted tunnel within another network.
- Deployed to connect two or more private networks to one another over an untrusted network (public internet).
- Traffic is encrypted while travelling over the untrusted network.
    - safely and securely share sensitive information.

### VPN gateways

- A type of virtual network gateway. 
- Deployed in a dedicated of the virtual network.
- Enables following connectivity:
    - Connect on-premises datacenters to virtual networks through a site-to-site connection.
    - Connect individual devices to virtual networks through a point-to-site connection.
    - Connect virtual networks to other virtual networks through a network-to-network connection.

- You can only deploy one VPN gateway in each Vnet. But can use that one gateway to connecto to multiple locations.
    - VPN type determines how traffic is encrypted:
        - Policy-based
            - specify statically the IP address of packets that should be encrypted through each tunnel. This type of device evaluates every data package against those sets of IP address to choose the tunnel where that packet is going to be sent through.
        - Route-based
            - IPSec tunnels are modeled as a network interface or virtual tunnel interface. IP routing decides which one of these tunnel interfaces to use when sending each packet. Route-based VPN's are the preferred connection method for on-premises devices. They're more resilient to topology changes such as the creation of new subnets.
            - Route based when :
                - connections between virtual networks
                - point-to-site connections
                - multisite connections
                - Coexistence with an Azure ExpressRoute gateway.
    - Regardless of the type the authentication method is a preshared key.

### High-availability scenarios

Maximize resilience of your VPN gateway:

- Active/Standby
    - VPN gateways are by default deployed as two instances (active & standby). When the active instance is disrupted the standby instance automatically assumes responsibility for connections without any user intervention. Connections are interrupted during this failover.
- Active/active
    - since the introduction of Border gateway protocol (BGP). You can deploy VPN gateways in an active/active configuration.
        - You assign a unique public IP to each instance. 
        - You create seperate tunnels from the on-premises devices to each IP address.
        - You can extend the high availability by deploying an addition VPN device on-premises.

### ExpressRoute failover

- Configure a VPN gateway as a secure failover path for ExpressRoute connections. 
- ExpressRoute have resiliency built in.
    - They aren't immunie to physical problems.
- Provision a VPN gateway that uses the internet as an alternative method of connectivity. In this way you can ensure there's always a connection to the virtual networks.

### Zone redundant gateways

*In regions that support availability zones*

- Brings resiliency, scalability and higher availability to virtual network gateways.
- Deploying gateways in Azure availability zones physically and logically seperates gateways within a region while protectig your on-premises network connectivity to Azure from zone-level failures.
- The gateways require different stock keeping units (SKU's) and use Standard public IP addresses instead of Basic public IP addresses.

## Azure DNS

### Benefits

- reliability and performance
- security
- ease of use
- customizable virtual networks
- alias records

### reliability and performance

- Hosted on Azure's global network of DNS name servers.
    - provides resiliency and high availability
- uses anycast networking
    - the closest available DNS server-answers each DNS query 
        - fast performance
        - high availability

### Security

Based on azure resource manager
- Azure RBAC (role based access control)
- activity logs to monitor how a user in your organization modified a resource or to find an error when trouble shooting
- resource locking to lock a subscription, resource group or resource. Locking prevents other users from deleting/modifying critical resources

### Ease of use

- manage DNS records for your Azure services and for your external resources as well.
- integrated in Azure portal (same support contract/billing & credentials)
- manage from within Azure portal, using Azure Powershell cmdlets and the Azure CLI.

### customizable virtual networks with private domains

- supports private DNS domains.
    - allows use of custom domain names in your virtual private networks.

### Alias records

- supports alias record sets
    - refer to an Azure resource (public IP, Traffic manager, Content delivery network (CDN))
        - the alias records updates itself during DNS resolution in case the IP address of the underlying resource changes.
        - the alias records points to the server instance not the IP address.


