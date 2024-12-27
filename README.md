# VMware-Home-Lab
Guide on setting up a VMware-based home lab with Windows Server

# Building a Home Lab: Setting Up a Windows Server 2022 Environment with VMware

Creating a home lab is an excellent way to hone your IT skills and experiment with enterprise-level technologies. In this post, I’ll document the process of setting up a virtualized Windows Server 2022 environment using VMware Workstation Pro. This guide will cover configuring networking, installing Active Directory (AD), setting up DHCP, and more. Let’s dive in!

---

## **Step 1: Install VMware Workstation on Your Home PC**

1. Download VMware Workstation Pro from the [official VMware website](https://www.vmware.com/).
2. Install it on your home PC following the installation prompts.

---

## **Step 2: Configure Networking in VMware**

1. Create two network devices:
   - **NAT**: This device will provide external internet access for your virtual machines.
   - **LAN Segment**: Used to create a private internal network for communication between virtual machines.

---

## **Step 3: Set Up the Windows Server 2022 VM**

1. Download the Windows Server 2022 ISO from Microsoft’s website.

2. Create a new virtual machine in VMware Workstation:

   - Select the downloaded ISO image as the installation media.
   - Allocate sufficient resources (e.g., 2 CPUs, 4 GB RAM, 50 GB disk space).

3. Start the VM and complete the Windows Server 2022 installation.

4. Once installed, install VMware Tools in the server VM to enhance performance and add features like drag-and-drop.

---

## **Step 4: Configure Network Adapters in the Server VM**

1. Open the **Network and Sharing Center** in the Windows Server VM.
2. Rename the network adapters:
   - External Adapter: `External`
   - Internal Adapter: `Internal`
3. Configure the internal network adapter:
   - **IP Address**: `172.16.0.1`
   - **Subnet Mask**: `255.255.255.0 (/24)`
   - **DNS Server**: Loopback (`127.0.0.1`)
   - Leave the **Default Gateway** blank since the Domain Controller (DC) will serve as the gateway.

---

## **Step 5: Set Up the Domain Controller**

1. Rename the server to something descriptive like `DC-PCName`.
2. Install the **Active Directory Domain Services (AD DS)** role:
   - This will also install the necessary NAT functionality.
3. Create a new forest and root domain named `mydomain.com`.
4. After the configuration is complete, create a network administrator account and add it to the **Domain Admins** group.

---

## **Step 6: Configure Routing and Remote Access**

1. Add the **Remote Access (RAS)** role:
   - Enable **Routing** and set it to use the internet interface (external adapter).
2. If the **Routing and Remote Access services** stop unexpectedly:
   - Go to **Services**, locate these services, and manually start them.

---

## **Step 7: Configure DHCP**

1. Add the **DHCP Server** role.
2. Create a new IPv4 scope:
   - Name the scope after the range, e.g., `Scope_172.16.0.x`.
   - Enter the IP range, subnet mask (`255.255.255.0`), and other options.
3. Configure DHCP options for clients:
   - **Router (Default Gateway)**: `172.16.0.1`
   - **Parent Domain**: `mydomain.com`
   - **DNS Server**: `172.16.0.1`
4. Authorize the DHCP server:
   - Refresh the DHCP console after authorization to ensure it’s active.

---

## **Step 8: Allow Domain Controller Internet Access**

> **Note**: Allowing a DC to access the internet is typically not recommended in production environments but is acceptable for a home lab setup.

1. In the **Local Server** settings, turn off **IE Enhanced Security Configuration** to enable easier internet browsing.

---

## **Step 9: Automate User Creation with PowerShell**

1. Create a script to automate:
   - Adding multiple users to the domain.
   - Setting up passwords.
   - Creating a shared `USERS` folder.
2. Save and run the script within the server environment.

---

## **Step 10: Add a Windows 11 Client to the Domain**

1. Install Windows 11 on another VM.
2. Configure the client’s network adapter to use the internal network.
3. Ensure DHCP is assigning IP addresses correctly:
   - Run `ipconfig /renew` on the client.
   - Check that the client’s default gateway is `172.16.0.1`.
4. Rename the client PC to something descriptive.
5. Join the domain:
   - Go to **System Properties** > **Change Settings** > **Domain**.
   - Enter `mydomain.com` and use any network account to log in.

---

## **Testing Your Setup**

- Verify that:

  - The client can communicate with the server.
  - Domain users can log in to the client machine.
  - DNS and DHCP are working as expected.

- Troubleshoot any remaining issues, such as services failing to start, and ensure all configurations are correct.

---

By following these steps, you’ll have a fully functional Windows Server 2022 environment in your home lab. This setup is a great foundation for exploring more advanced configurations like Group Policy, virtualization, and additional roles. Happy learnin=
