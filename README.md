<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating System </h2>

- Windows 10</b> (22H2)


<h2>Installation Steps</h2>

<p>
First we just need to create a new resource group
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/30503844-02a8-4c6d-b725-9d31b3827eeb" height="80%" width="80%"/>
</p>
<br />

<p>
Create a new Virtual Network on this resource group
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/3b948c2e-157a-44f6-a795-43e8d4f72c45" height="80%" width="80%"/>
</p>
<br />

<p>
Next we need to create a virtual machine in this new resource group to run the domain controller, we need this vm to be running windows server 2022 datacenter
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/310d8b64-15e8-4080-8b9e-5e06ead68dc9" height="80%" width="80%"/>
</p>
<br />

<p>
Go to the network section of the virtual machine creation and select Active-Directory-Vnet we just created
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/9f5573f7-e9d9-4076-8ea8-8093b9059ce2" height="80%" width="80%"/>
</p>
<br />

<p>
Now while that is creating we will make another VM in the same resource group running windows 10 as well as putting it on active-directory-vnet in the network settings
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/81713b06-3437-4eb1-91ab-0d095e1ee50b" height="80%" width="80%"/>
</p>
<br />

<p>
Next we need to set the Domain controllers IP to static so it can act as a domain controller and no problems occur. To do this we need to click on dc1 in the virtual machines.

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/8ee31b4a-c995-4681-9853-69b599cd09a5" height="80%" width="80%"/>
</p>
<br />

<p>
Now we will click on the network interface/IP config button
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/3ec67baf-3512-4dee-ad87-9e60547f42c7" height="80%" width="80%"/>
</p>
<br />

<p>
Now click ipconfig1 we need to change this to static
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/10f27515-c25e-4f36-8107-b0cca354ffe4" height="80%" width="80%"/>
</p>
<br />

<p>
Change allocation to static and click save
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/ae506fa6-3fd7-4236-a9c8-f3a23313e5dd" height="80%" width="80%"/>
</p>
<br />

<p>
Next we need to log into the domain controller and disable the firewall just so that everything works smoothly for the purpose of this lab, this may not be done in a real setting. First we lRemote into the Dc1 VM and once at the desktop open run and type ws.mfc and enter to open the Windows firewall security settings
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/18e10ca7-e0c7-42c3-a35d-f2c9a68e9bbb" height="80%" width="80%"/>
</p>
<br />

<p>
We then can click Windows Defender firewall properties and disable them all.
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/364458ee-91eb-40a4-81fc-567e923ae71b" height="80%" width="80%"/>
  <img src="https://github.com/user-attachments/assets/a22fb902-5797-4abc-8292-07c3e7e9a7ee" height="80%" width="80%"/>
</p>
<br />

<p>
Next I'm going to set Client1’s dns server as Dc1’s ip address that we set to static. We can do this by first getting the private ip address for the DC1 which in my case is 10.0.0.4 and then go to the client 1 network settings in azure
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/b3d06625-0f8a-47c5-8e03-7a0ccad5fd47" height="80%" width="80%"/>
</p>
<br />

<p>
Click the network interface/ip config button again but this time go to the DNS tab
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/6afe51f1-3bfc-44ba-a50d-7998a4f0749b" height="80%" width="80%"/>
</p>
<br />

<p>
Next click custom and input the ip address from earlier and then click save

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/21b072b7-193f-4d60-aca6-5dfc4d703b6e" height="80%" width="80%"/>
</p>
<br />

<p>
Next restart client1 VM
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/55a7075a-cf86-4d42-887e-1d0a04052727" height="80%" width="80%"/>
</p>
<br />

<p>
Next I will remote into Client-1 VM
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/19d1b031-3428-4129-9a2d-8623018be7d4" height="80%" width="80%"/>
</p>
<br />

<p>
After logging into Client-1 we can open powershell and try to ping dc1’s private ip
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/19a25a86-e797-4ef7-8aef-8c954c8ce85a" height="80%" width="80%"/>
</p>
<br />

<p>
After that we can see if the DNS server is set correctly using ipconfig /all, which it is

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/caa12bcd-9398-4a11-b1d8-6fb6b7ff70e6" height="80%" width="80%"/>
</p>
<br />

<p>
Now back on the Dc1 VM we will install the Active Directory, we first open server manager
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/80f8354e-56be-4fc9-b10d-8b72ebcf2c73" height="80%" width="80%"/>
</p>
<br />

<p>
Click add roles and features
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/410c2523-e635-4928-be31-0fc6772f4855" height="80%" width="80%"/>
</p>
<br />

<p>
Click next until you get to server roles and click Active Directory Domain Features in the list then click add features
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/3317dcd6-ca0c-4033-9d50-613b66629fb7" height="80%" width="80%"/>
</p>
<br />

<p>
Then click next until you are able to click the install button
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/35ef67ef-ade2-4f8a-9049-91c3f87f0f35" height="80%" width="80%"/>
</p>
<br />

<p>
After that is done we can click the flag on the top and press promote this server to a domain controller

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/c1de7961-72b9-41b5-a2d5-b56f1d46988b" height="80%" width="80%"/>
</p>
<br />

<p>
Then select add a new forest and input a name for your domain then press next
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/a44654a5-96db-4964-b2e5-e62d82a686b6" height="80%" width="80%"/>
</p>
<br />

<p>
In the next screen you need to make a password then click next until it lets you click install
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/f31db197-0f76-4fee-bc67-e098af208257" height="80%" width="80%"/>
</p>
<br />

<p>
The Server will now be configured as a domain controller and automatically restart
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/3f8e8650-902e-446a-ac9d-01ab6681edad" height="80%" width="80%"/>
</p>
<br />

<p>
We can now remote back into the domain controller using the new domain in our username so we can login in domain context

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/02eba48f-70a6-401f-9351-853844a36cda" height="80%" width="80%"/>
</p>
<br />

<p>
Next we are going to create a Domain Admin for the domain. First we need to open Active Directory Users and Computers and i am going to make 2 Organizational units Employees and Admins

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/d3b1c8de-6949-4451-b28a-0cfee7582729" height="80%" width="80%"/>
</p>
<br />

<p>
Next we can add an employee, go to the dropdown under mydomain.com and open the admins folder left click and add a new user and fill out the users info

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/6266b605-85e5-4f19-a32c-43769e53623d" height="80%" width="80%"/>
  <img src="https://github.com/user-attachments/assets/0e1f3930-624f-4c76-aa60-edf5463a125c" height="80%" width="80%"/>
</p>
<br />

<p>
This account is not yet an admin, we first have to right click jane and press properties, then to the member of tab and press add
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/18900a82-1dd4-4906-95ef-a4cc2b8544fc" height="80%" width="80%"/>
</p>
<br />

<p>
Next type domain admins then check names and ok then apply
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/53ab1374-5b7b-4cd2-b154-74689dae7e5c" height="80%" width="80%"/>
</p>
<br />

<p>
Next we can try to log back in as the new admin account
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/22783272-247f-49a6-b99b-88475cbc616c" height="80%" width="80%"/>
</p>
<br />

<p>
After this is back on we can join client 1 to the domain first go into the client 1 VM and go to system settings and click rename this PC advanced
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/3e583fc5-c961-4e13-ac54-9b292a332d98" height="80%" width="80%"/>
</p>
<br />

<p>
Click Change…

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/7fe7c4bb-e4c9-4739-94c8-9ab0fdf72d36" height="80%" width="80%"/>
</p>
<br />

<p>
Now click the Domain under the member of section and type in your domain
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/8c637657-4b88-4ea2-bf15-fdb0e5fb9089" height="80%" width="80%"/>
</p>
<br />

<p>
Now we need to use the admin account to give permission
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/a9ccca70-10fc-423e-87bf-d329055b1fd3" height="80%" width="80%"/>
  <img src="https://github.com/user-attachments/assets/af9d901f-2aa9-4d86-84b3-7b43e6918aed" height="80%" width="80%"/>
</p>
<br />

<p>
Next it will need to restart so press ok
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/da3588c6-8713-4fcf-97ff-40e06aebdd56" height="80%" width="80%"/>
</p>
<br />

<p>
Next we can check on the domain controller if the Client 1 VM shows up on the Active Directory users and comps, we first launch Active Directory Users and Computers then clicking mydomain.com then computer and you can see it is listed
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/0581c13d-ce0a-4fe8-ae95-952ce737c2c9" height="80%" width="80%"/>
</p>
<br />

<p>
Next i'm going to make a new organizational unit for clients like this one and I will put client one in by dragging it and then clicking yes

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/d560d08b-047a-4213-ac0e-829cc55f3524" height="80%" width="80%"/>
</p>
<br />

<p>
Next we need to allow non admins to log into client 1 so we first need to log into client 1 as an admin
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/865ceefa-ad4a-4816-88d2-9c135e32fba1" height="80%" width="80%"/>
</p>
<br />

<p>
Next we need to open system settings and click the Remote Desktop tab and then the Select users that can remotely access this PC button
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/b4beb89b-29b5-4afa-8db6-c1a778d31400" height="80%" width="80%"/>
</p>
<br />

<p>
Then we can add Domain Users to this list to allow all domain users to remote into this client then click ok

  </p>
<p>
  <img src="https://github.com/user-attachments/assets/2a84ee77-60d3-46fe-9a38-6f7d494a6da4" height="80%" width="80%"/>
</p>
<br />

Next we are going to create a bunch of users in the employees folder first we need to go back to the DC1 VM and launch power shell ISE as an administrator and I am going to use this [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) to automatically create 10 thousand users
<p>
  <img src="https://github.com/user-attachments/assets/de807296-5c2f-49dd-b895-c121cc546ca1" height="80%" width="80%"/>
</p>
<br />

<p>
Now when we refresh the Employees OU it contains all of our created employees
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/00f2b368-ca42-4ca1-8106-cfe4f65b69c0" height="80%" width="80%"/>
</p>
<br />

<p>
Now we can try to login to client 1 as this user
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/48702118-2b7e-47da-9bfa-854626a72bf4" height="80%" width="80%"/>
  <img src="https://github.com/user-attachments/assets/99c1d3d6-b95c-4df8-9086-1c3d174486e1" height="80%" width="80%"/>
</p>
<br />

<p>
As we can see the account is able to login
  </p>
<p>
  <img src="https://github.com/user-attachments/assets/84fd3f2c-bd30-4755-ab40-ebe33718cfb1" height="80%" width="80%"/>
</p>
<br />


