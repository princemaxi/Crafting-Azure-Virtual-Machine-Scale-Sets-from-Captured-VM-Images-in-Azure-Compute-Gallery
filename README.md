![image](https://github.com/user-attachments/assets/2613cd4c-e35a-4d85-83f2-9645e4416891)

# Introduction

In the ever-evolving world of cloud computing, **scalability is key**. Whether you're a startup gearing up for rapid growth or an established enterprise managing variable workloads, **Azure Virtual Machine Scale Sets (VMSS)** can be a game-changer. This versatile Azure service allows you to effortlessly manage and scale multiple virtual machines (VMs) while ensuring high availability and ease of management. In this award-winning blog, we'll walk you through creating a VMSS from a compute gallery in the Azure Portal, unlocking the potential for your applications to scale dynamically.

## Why VMSS and Compute Gallery?

Before we dive into the steps, let's understand the significance of using VMSS with a compute gallery.

**Azure Compute Gallery** is a powerful feature that allows you to create and manage custom virtual machine images. These images can include your desired operating system, applications, and configurations. **VMSS**, on the other hand, is an Azure service that lets you deploy and manage a set of identical VMs. Combining these two technologies allows you to scale applications horizontally by deploying VMs based on custom images from the gallery.

[Click here to learn how to create a compute gallery and capture a VM image](https://docs.microsoft.com/azure/virtual-machines/windows/instant-vm-gallery)

# Benefits of Azure Virtual Machine Scale Sets (VMSS)

Azure Virtual Machine Scale Sets (VMSS) offer several benefits for managing and deploying virtual machines in Microsoft Azure:

- **High Availability**: VMSS automatically distributes VM instances across multiple fault domains and update domains to ensure high availability. If a VM becomes unhealthy, it can be automatically replaced with a new one, reducing downtime.

- **Scalability**: VMSS allows you to easily scale your applications up or down based on demand. You can define scaling rules to add or remove VM instances automatically, ensuring your application can handle varying workloads efficiently.

- **Automatic Updates**: Configure VMSS to perform rolling updates, ensuring that your VM instances are always up-to-date with the latest patches and software updates without causing downtime.

- **Custom Images**: VMSS supports custom VM images, allowing you to create and configure a single VM, capture its image, and then use that image to scale out instances. This simplifies deployment and ensures consistency across VM instances.

- **Managed Disks**: VMSS can use managed disks, providing features like automatic backups, replication, and enhanced reliability.

- **Auto Scaling**: Define auto-scaling rules based on performance metrics, such as CPU utilization or memory usage, to automatically adjust the number of VM instances to match your application's needs.

- **Integrated Monitoring**: VMSS integrates with Azure Monitor, allowing you to collect and analyze performance data, set up alerts, and gain insights into the health and performance of your VM instances.

- **Cost-Efficiency**: VMSS can help optimize costs by automatically scaling down when demand is low and scaling up when demand increases, ensuring you only pay for the resources you need.

- **Ease of Management**: VMSS simplifies the management of a group of VMs. You can define a common configuration for all instances in the set and make changes centrally, reducing administrative overhead.

- **Rolling Upgrades**: When you update the underlying image or configuration of your VM instances, VMSS can perform rolling upgrades to minimize application downtime and ensure a smooth update process.

- **Template-Based Deployment**: Use Azure Resource Manager (ARM) templates to define and deploy VMSS instances, making it easier to replicate your application environments consistently.

- **Integration with Azure DevOps**: VMSS can be integrated into your CI/CD pipeline, allowing you to automate the deployment and scaling of your applications.

- **Global Deployment**: Deploy VM instances across multiple Azure regions, improving the resilience and availability of your applications.


## Prerequisites

Before you begin, ensure that you have the following prerequisites in place:

1. **Azure Subscription**: You need an active Azure subscription. If you don't have one, you can [sign up for a free trial at the Azure Portal](https://azure.microsoft.com/free).

2. **Azure Resource Group**: Create an Azure Resource Group to organize and manage your gallery resources.

3. **Azure Compute Gallery**: Make sure you have created a compute gallery and have captured the image of the VM you want to scale. [Click here to learn how to create a compute gallery and capture a VM](https://docs.microsoft.com/azure/virtual-machines/windows/instant-vm-gallery).

# Creating a VM Scale Set

1. **Log in to the Azure Portal**: Go to the [Azure Portal home page](https://portal.azure.com/) and sign in with your credentials.

![image](https://github.com/user-attachments/assets/ec114d4a-512c-40e5-b043-4a3bfe4404e8)

2. Click on 'Create a resource' and search for 'Virtual Machine Scale Set'. Select it.

![image](https://github.com/user-attachments/assets/e9caf624-d31d-43ea-89e7-295ac02b8b19)

![image](https://github.com/user-attachments/assets/ed1808cb-2352-4256-b33e-a1d14b0c5e49)

3. Click on 'Create'.

![image](https://github.com/user-attachments/assets/caf39d95-89ab-43b8-b65a-ee00bf92e0b6)

4. Basics: Fill out the basic information, such as the subscription, resource group, and region. Give your VMSS a unique name.

![image](https://github.com/user-attachments/assets/616b3436-53f0-4384-9c2e-cd1dff72262e)

# Orchestration Modes in Azure VM Scale Sets

The **orchestration mode** is defined when you create the scale set and cannot be changed later. Azure provides two types of orchestration modes:

a. **Uniform Orchestration**: Optimized for large-scale stateless workloads with identical instances.
b. **Flexible Orchestration**: Used to achieve high availability at scale, supporting identical or multiple virtual machine types.

### Flexible Orchestration

When selecting **Flexible orchestration**, Azure provides a unified experience across the Azure VM ecosystem. Flexible orchestration offers high availability guarantees (up to 1000 VMs) by distributing VMs across fault domains within a region or Availability Zone. This enables you to scale out your application while maintaining fault domain isolation, essential for running quorum-based or stateful workloads.

**Use cases for Flexible orchestration include**:

- **Quorum-based workloads**
- **Open-source databases**
- **Stateful applications**
- **Services requiring high availability and large scale**
- **Applications that mix VM types** or leverage both Spot and on-demand VMs

Flexible orchestration is ideal for applications needing high availability or complex deployment requirements, including existing applications previously deployed in Availability Sets.

[Learn more about Azure VM Scale Set Orchestration](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-orchestration-modes)

![image](https://github.com/user-attachments/assets/eb3cfb32-55d9-4da9-86f1-ffa7ccb4f40d)

5. Instance Details: Configure instance details, such as the VM image, VM architecture and instance size.
select the 'gallery image' you created earlier. This is where the power of Compute Gallery comes into play. You can choose the specific version you want to deploy.

![image](https://github.com/user-attachments/assets/6a4089f4-ff6b-42c4-a53c-743d682fd532)

This will automatically select the VM architecture and VM size that was used in the VM we captured in our compute gallery.

![image](https://github.com/user-attachments/assets/7131e07a-18cd-498c-bc37-c238515c08d4)

Administrator Account: This is where you setup the username and password of your VMSS, but because we selected the Specialised operating system state when we captured the VM image it is grayed out because we no longer need authentication when we want to scale out.

Make sure you choose a 'licensing type' and check the box to confirm you have an eligible windows license. For the purpose of the VMSS we are to scale, we optioned for 'windows server' since we are scaling out to manage network traffic.

![image](https://github.com/user-attachments/assets/edd970a8-ac34-461a-bfd2-6638d68ee24b)

6. Scaling: Configure scaling options based on your application needs.

![image](https://github.com/user-attachments/assets/c8dc90cb-2685-4e94-929c-80fbcd2a0c33)

7. Select 'Custom scaling' so that you can define scaling rules for automatic scaling. Here we define:

- Initial instance count to **2**.
- Minimum number of instances to **1**.
- Maximum number of instances to **10**.
- The VMSS to scale out in **5 minutes** once the CPU threshold gets to **75%** by **2 instances**.
- The VMSS to scale in, in **5 minutes** once the CPU threshold gets to **25%** by **2 instances**.

![image](https://github.com/user-attachments/assets/50016795-d02f-4474-adfb-e939cbe88e3a)
![image](https://github.com/user-attachments/assets/a251122f-13c5-4926-a3bd-53572f537e30)

8. Management: Specify monitoring, diagnostics, and auto-shutdown settings as per your requirements or leave it on default just like I did.

9. Tags: Ensure that your resources are tagged and you can add tags based on your preference.

![image](https://github.com/user-attachments/assets/bb3f904d-4e82-430b-bb4e-5e82d8c648ea)

10. Review + Create: Review your settings, and when everything looks good, click 'Create' to deploy your VM Scale Set.

![image](https://github.com/user-attachments/assets/bb16350a-c993-4bd2-a05d-37eb2e8d4483)

![image](https://github.com/user-attachments/assets/d3cd3296-08c1-4041-9a1f-41217ecbf307)

11. This will take a while, Once deployment is completed, click on 'Go to resource' to access the newly created VMSS.

![image](https://github.com/user-attachments/assets/efc4566f-6717-4fc3-bf08-52d76f05c410)
    
This is the newly deployed Virtual Machine Scale Set and you can see 2 initial instance defined during configuration are already running.

![image](https://github.com/user-attachments/assets/de3f4d85-c352-45b8-8028-8a317adea09b)
![image](https://github.com/user-attachments/assets/c4a485a0-c596-4ea5-b58e-72e1c152da89)

# Monitor and Manage Your VMSS

Once your VMSS is deployed, you can use Azure Monitor and Azure Autoscale to ensure your application scales efficiently and is highly available. Azure Monitor helps you gain insights into your VMSS's performance, while Autoscale allows you to automate scaling based on metrics and schedules.

# Conclusion

In this comprehensive guide, we've explored the process of creating a Virtual Machine Scale Set (VMSS) from a custom image in Azure Compute Gallery. By leveraging these Azure services, you can optimize the scalability and availability of your applications, ensuring they can handle varying workloads with ease.

VMSS and Compute Gallery are indispensable tools in your cloud infrastructure toolbox, empowering you to deploy, manage, and scale virtual machines efficiently. As you embark on your cloud journey, remember that flexibility and scalability are your allies, and Azure is your trusted partner in achieving your goals.

Embrace the power of Azure VMSS and Compute Gallery to unlock the true potential of your cloud applications. Scale up, scale out, and scale confidently with Microsoft Azure.
