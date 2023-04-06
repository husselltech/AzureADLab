# AzureADLab
Azure Active Directory Lab/Tutorial
On-site Active Directory using Microsoft Azure Cloud Services

Summary:
This is a tutorial/lab utilizing the following technologies: Microsoft Azure Cloud Services, Virtual Machines, Virtual Networks, Microsoft Server 2022, Remote Desktop Protocol, and Active Directory. Using Microsoft Azure, we will create two Domain Controllers running Active Directory that will act as DNS servers for a local network. Once the Domain Controllers are active, we will practice creating users. You can use this tutorial lab for further learning including creating users with PowerShell, testing Group Policies, VPN scenarios, and test Azure AD Connect.

Let us begin!

First you will need to create a Microsoft Azure account. Go to azure.microsoft.com to create your free account. Once that is all set up go ahead and initiate Microsoft Azure in your browser and we will get started.

Note: Due to updates from Microsoft Azure some options within the program may change over time.

Overall Configuration:

We will set up a Virtual Network on which two Virtual Machines, acting as Domain Controllers will be running Active Directory. In a real-world environment this configuration creates necessary redundancy.

 



![image](https://user-images.githubusercontent.com/114452968/230461226-e5419c22-07e4-4d41-9bcd-67218293459c.png)








Our first step is to create a Resource Group. Click Create and name your resource group. Go through the prompts and then click Create.

 

 
![image](https://user-images.githubusercontent.com/114452968/230461545-09af4624-8970-4f8c-a329-c83433c6b2a3.png)
![image](https://user-images.githubusercontent.com/114452968/230461593-cb0315ff-e8e7-44c6-af43-4a8226a42075.png)







Next, we will create a Virtual Network for our Active Directory Domain Controllers. On the side menu find Virtual Networks and click Create. 

 ![image](https://user-images.githubusercontent.com/114452968/230461630-3a9e7ee9-70ef-48bc-913c-3f082e7fff20.png)
















Give your Virtual Network a name. I have named mine “LocalOnSiteNetwork” because I want to simulate a SOHO environment. Make sure the Virtual Network is part of the Resource Group you originally created.

 ![image](https://user-images.githubusercontent.com/114452968/230461679-93d9eaea-fbdd-41e6-adf2-d308e88eefa9.png)

















Now, navigate to the IP addresses tab. Here, we will keep the default IPv4 address, but we will change the subnet. Click Add a subnet. I changed my subnet to 10.0.1.0. Click Add, then remove the “default” subnet. Finally click Review + Create > Create.
![image](https://user-images.githubusercontent.com/114452968/230461713-96ff5daf-4b09-4037-9bc3-449357f85280.png)

 ![image](https://user-images.githubusercontent.com/114452968/230461739-2147b6d4-043d-414c-8bbb-cff94bc4ebae.png)



 
Our Virtual Network is now set up, and we are now ready to start up a Virtual Machine onto which we will install Windows Server 2022.



The next step in this lab will be to create a Virtual Machine running Windows Server. Navigate to Virtual Machines on the left side menu. Click Create, and if it gives you options choose Azure Virtual Machine (the first option).	
 
![image](https://user-images.githubusercontent.com/114452968/230461806-77c45820-b4d8-4586-9f39-277542f42653.png)

 ![image](https://user-images.githubusercontent.com/114452968/230461856-76276412-5301-4a91-b4e0-fbc547955c93.png)

In the next screen add the Virtual Machine to your Resource Group. In this case my Resource Group is called ADLabHussell1. Name your Virtual Machine; I named my VM DomainController1. Next, change Availability options to Availability set, and under Availability set click Create new, name it, and keep the default settings. Click OK. We are doing an Availability set for maximum uptime for our two Domain Controllers running Active Directory. For the Image I chose Microsoft Server 2022, although you may choose to run a different version. The rest of this lab uses Microsoft Server 2022. 
 ![image](https://user-images.githubusercontent.com/114452968/230461912-2e403dc2-535c-40e1-81d6-0293bc915aba.png)

(Note: Although named DomainController1, for brevity it will hereafter be referred to as DC01.)
Create a username and password and practice good account security! Allow RDP and click Next.
 

![image](https://user-images.githubusercontent.com/114452968/230462052-9ffe5045-98b3-4301-a29c-c14d0a7f4119.png)





On the Disks screen you may choose what type of storage you want. I changed the disk type to Standard HDD as there is no need for anything more for the purposes of this lab. Scroll down and click Create and attach a new disk, as this is where we will store the Active Directory Services.
 
![image](https://user-images.githubusercontent.com/114452968/230462120-7a0a3caf-6188-4ef6-a9d8-469609255684.png)




We do not need a large storage space for this lab so under size click Change size to Standard HDD. Set the custom disk size to 10 GiB. This allows more than enough space. Click OK, then OK again. Now our first Virtual Machine running Windows Server 2022 has been created and it is time to click Next: Networking. 
 
 ![image](https://user-images.githubusercontent.com/114452968/230462193-3d32fc72-992e-4b56-b353-6370d5f727c9.png)

 ![image](https://user-images.githubusercontent.com/114452968/230462239-05cfba9e-1a44-4cf1-900a-080d689ca37e.png)

![image](https://user-images.githubusercontent.com/114452968/230462268-fd35e6bf-3d3d-4c7b-a8e2-a87b1853f52b.png)







On the Networking menu leave the defaults and click Next: Management. On the Management menu leave the defaults and click Next: Monitoring. On the Monitoring menu disable boot diagnostics as they are not needed for this lab. Click Next: Advanced > Next: Tags > Next: Review + Create. 
 
![image](https://user-images.githubusercontent.com/114452968/230462304-e914fa4f-58c8-4b60-86b6-c452b1bb8fbd.png)




Once at this screen click Create and Azure will show you the deployment in progress. This will not happen immediately, but should take no more than a few minutes.
 ![image](https://user-images.githubusercontent.com/114452968/230462353-04221647-bf32-4eec-ba89-a3ccf70199dd.png)

Once that you see that the deployment of your Virtual Machine running Windows Server 2022 is complete click Go to resource.
 
![image](https://user-images.githubusercontent.com/114452968/230462389-b2229bac-7ae0-4cf7-b588-a834e669c914.png)





Now, we are going to connect to DC01. Click connect in the upper-left corner and choose Remote Desktop Protocol (RDP) connection and download the RDP file for DC01. 
 
![image](https://user-images.githubusercontent.com/114452968/230462446-3108c16e-8a34-49f8-b691-63156f1c2bc6.png)












Locate the RDP file in your downloads and open it. Windows will go through UAC prompts and we will input the credentials we added earlier for DC01.
 
![image](https://user-images.githubusercontent.com/114452968/230462487-07a9cba7-50fd-4116-a265-d5fcfa339a5d.png)










We then see a new window/screen pop up on the system. This is DC01 running Server Manager which we will see after the Virtual Machine fully starts up.
 ![image](https://user-images.githubusercontent.com/114452968/230462519-044c75ee-eb07-4570-ad2e-6663fd346722.png)


Next, in the top right portion of the screen, Click Tools > Computer Management > Disk Management. Change the partition style to MBR (Master Boot Record) if it is not already the default setting. Do not use GPT. We also need to format the disk we are using. Right click on Disk 2 and do a quick format. Create a simple volume and keep the drive as F:\. Click OK and exit Disk Management. 
 ![image](https://user-images.githubusercontent.com/114452968/230462549-dbee1b31-46d4-4463-b24f-05e91e082436.png)


Back at the dashboard for Server Manager click Manage > Roles and Features. This is the moment we will be installing Active Directory onto DC01. Click Next. We will be keeping “Role-based or feature-based installation.” 
 
![image](https://user-images.githubusercontent.com/114452968/230462581-cb370d8e-e163-4f71-952a-31a140ecc969.png)












Click Next until you are able to select Active Directory Domain Services. Select Active Directory Domain Services.
 
![image](https://user-images.githubusercontent.com/114452968/230462613-a99d3752-f5bb-4c1f-bb40-2a672daea7fc.png)












Keep clicking Next until you get to Confirmation. Click Restart the Destination Server if Required > Install. The installation of Active Directory might take a few minutes. When the installation is complete click Promote this server to a domain controller.
 ![image](https://user-images.githubusercontent.com/114452968/230462643-2ddba222-4316-4855-8bb4-fd87e45bab7a.png)










On the Deployment Configuration screen, we will select Add a new forest and set a new Root domain name. I called my domain hussellazure.com. You can call yours anything, just make sure you remember it for later. Click Next. Keep the functional levels at Windows Server 2016 and add a password in the rare case that we need to uninstall Active Directory. I kept the same password for this whole exercise for convenience. Click Next.
 ![image](https://user-images.githubusercontent.com/114452968/230462691-b293a8ec-4666-4539-9c42-f2b54d66a88a.png)





An error should appear regarding DNS. We will ignore this for now since this problem will be solved in a later step. Click Next. The NETBIOS name should appear and auto-fill. Click Next. At the Paths menu change the C:\ drive to F:\ drive. This is the new partition that we formatted earlier. Simply delete the “C” in the three options and type in “F.” Leave the rest of the information provided. For example, the first option should read F:\Windows\NTDS. Keep clicking Next until you get to the Prerequisites Check menu.
 ![image](https://user-images.githubusercontent.com/114452968/230462717-4cf82e91-1263-462b-8b07-c9c65fafd38c.png)




At the Prerequisites Check menu you will see some various warnings (some pertaining to DNS), but the green checkmark at the top means you have passed the checks. Click Install and more warnings will appear (again, ignore them as they will be addressed later). The Virtual Machine will then need to restart. Proceed with the restart and then we will move on to the next phase.
Note: The restart may not automatically bring up the VM again. You may have to manually RDP back into DC01.
 ![image](https://user-images.githubusercontent.com/114452968/230462795-8432658d-4c72-4a6b-a7af-334e0529ddd8.png)

After the restart we will create a second Domain Controller. Repeat the same steps as before, but give it a different name. My second Domain Controller was called DC02.



Before moving on take note of your first Virtual Machine’s (DC01) IPv4 address. In a real-world scenario we would want a static IP. We want a static IP because these servers are a constant and essential part of our network.
In Azure navigate to Networking > IP configurations and then change your IP address to a static IP address.
 ![image](https://user-images.githubusercontent.com/114452968/230462853-da7049dd-4700-4442-b2fe-d719f4f1d2c3.png)

Navigate to the Virtual Network menu in Azure. Find the DNS servers option. We will use a custom DNS server and the IPv4 address will be the one we noted from our first Virtual Machine: DC01. In this case the IPv4 address is 10.0.1.4. We are using the Active Directory servers for DNS and not Microsoft Azure.




If DC01 is not already back up go ahead start it via RDP. At Server Manager select Tools > Active Directory Users and Computers. The screen should show that Active Directory has been launched and you check this status by clicking Users. All the Domain Controller options will be present.
 
![image](https://user-images.githubusercontent.com/114452968/230462928-43716a5f-e111-4b96-87d4-212561803081.png)








Back in Azure navigate to Virtual Machines. Select DC02 (or your second VM). We need to restart the machine so that it gets DNS from our server, DC01, and not Azure. In the top menu find the Restart button and do a restart of DC02.
 ![image](https://user-images.githubusercontent.com/114452968/230462981-f5bda51b-b142-4b98-94a6-2e9f63aa3c46.png)


Now, we will RDP into DC02 using the same method as DC01. Find the file in your downloads, go through Windows UAC, and let the machine take the time to start. Once DC02 is up and running and Server Manager on-screen we will use the same steps we previously used with DC01 to set up Active Directory on DC02.










After Active Directory installs promote DC02 to a domain controller. This time we are NOT going to select the option to add a new forest. Here, we are going to select the option to add a domain controller to an existing domain. Type in the domain (website) you used for DC01. Change your credentials to something similar for your machine. The credentials I used were hussellazure.com\husselladmin with the same password that I had been using throughout. Use your Default First Site Name, then enter your username and password. Click Next until you get the option to replicate your domain controller. Choose the option to replicate from DC01 and click Next. 
Note: some users do not need to add “.com” to their credentials
 ![image](https://user-images.githubusercontent.com/114452968/230463015-3b314f37-6103-41f6-856c-8a22cd9673f1.png)





Change the drive letters from C:\ to F:\ as in previous steps. Remember, we allocated this drive for the purpose that in the rare chance that the server crashes we can still retain our Active Directory data. Click Next and Install. After the prerequisites are checked the Virtual Machine, DC02, will need to restart.
 ![image](https://user-images.githubusercontent.com/114452968/230463085-785df4ff-6382-408e-ad9f-631dcc4a1916.png)









RDP back in to DC02. The startup may take a while for the Group Policy Client to load. Once loaded click Tools > Active Directory Users and Computers > Domain Controllers. If all the steps were followed correctly we should see our two Virtual Machines acting as Domain Controllers.
 ![image](https://user-images.githubusercontent.com/114452968/230463155-e160a595-3ba4-4f46-bfa9-0de47eacabea.png)


Back in Azure click Overview for DC02 and make note of the IPv4 address. Navigate to Virtual Networks > DNS servers. Add the DC02 IPv4 address as a DNS server and click Save.
 ![image](https://user-images.githubusercontent.com/114452968/230463177-a9c7e27c-f152-4dc0-9354-1808c64d122b.png)








Go back to the Overview for DC02 and click on the Networking menu. Click on the Network Interface link > IP configuration, and change the IP to static like we did for DC01.
 ![image](https://user-images.githubusercontent.com/114452968/230463222-ac355f26-2e0d-46f9-9448-b6eb315bc1f7.png)
![image](https://user-images.githubusercontent.com/114452968/230463242-45796176-47d4-4a2c-8e1e-074b498d5ca8.png)

 



Almost there! RDP back into DC01 for our final steps. In Server Manager click Tools > Active Directory Sites and Services. I renamed my site from the default to “LocalOnsite” to simulate it being a SOHO environment. Now, we are going to add our subnet. Back in Azure find Virtual Networks > YOUR NETWORK > subnets and copy the subnet address.
 ![image](https://user-images.githubusercontent.com/114452968/230463273-989b6a82-7094-4dca-9b71-2fcb01a47388.png)


In DC01 right click on the Subnets folder and add a new subnet. Paste the subnet. Click OK.
 ![image](https://user-images.githubusercontent.com/114452968/230463289-1279f5e7-0ed8-4d95-9a6f-625cfa771b61.png)

In our final steps we will go to Tools > Active Directory Users and Computers. Right click on Active Directory Users and Computers and you will have the option to change Domain controllers. Pick one and we can practice adding users. Find the Users folder and right click to add a new user, and then fill out the form prompted on the screen. Give the user a password and finish the user setup. Check in both DC01 and DC02 for the new user you created. In this lab I used HussellTest1 as my new user.
 ![image](https://user-images.githubusercontent.com/114452968/230463317-23129503-4f8b-4d3d-b3e7-44b2fe10106e.png)
![image](https://user-images.githubusercontent.com/114452968/230463342-a6e23deb-3495-4429-ba21-e39580b1fcb7.png)
![image](https://user-images.githubusercontent.com/114452968/230463363-9140ebb9-ee00-4583-9412-7f252c837961.png)
![image](https://user-images.githubusercontent.com/114452968/230463387-db04ab20-c84d-4780-9c44-78b34ecdcf7c.png)

 
 
 

Conclusions:
We have deployed dual domain controllers running Active Directory and DNS in Microsoft Azure. You can use this tutorial lab for further learning including creating users with PowerShell, testing Group Policies, VPN scenarios, and test Azure AD Connect.


