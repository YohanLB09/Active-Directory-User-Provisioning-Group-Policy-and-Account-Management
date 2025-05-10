<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory - User Provisioning and Group Policy Object Configuration</h1>
In this guided lab, we will demonstrate how to enable Remote Desktop access for non-administrative users, automate user creation with PowerShell, and manage group policies. Additionally, we will cover account lockouts and log monitoring to simulate a real-life IT environment.<br />

<br />This project is a continuation of [Active Directory: Deployment and Configuration](https://github.com/YohanLB09/Active-Directory-Deployment-and-Configuration), so this project picks up where we left off.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop 
- PowerShell ISE
- Active Directory Users and Computers 

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro, version 22H2

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Enable Remote Desktop Connection for Domain users on the Client VM
- Step 2: Provision multiple Domain users
- Step 3: Test login as a Domain user
- Step 4: 
- Step 5: 
- Step 6: 
- Step 7: 

<h2>Project Walkthrough</h2>

<h3>Step 1: Enable Remote Desktop Connection for Domain users on the Client VM</h3>

<p>
<img src="https://i.imgur.com/itskafQ.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
Login to the Client VM as the Domain admin user created previously (katy_ admin in my case) -> right-click on the Start menu, and click on "System" to access the properties -> "Remote Desktop" -> "Select users that can remotely access this PC" -> "Add" ->  type "Domain Users" -> click on "Check Names" -> "OK" -> "OK".

  In this step, we logged in as the Domain admin user because administrative privileges are required to modify system-level settings, such as configuring who can remotely access the computer. Subsequently, we enabled Remote Desktop Connection for Domain users on the Client VM to allow non-administrative Domain users to remotely log into this specific client machine for accessing their newly created accounts and basic domain functionality. Domain users will be provisioned in the following step. 

Keep in mind that in a real-world scenario, this step would typically be done centrally using Group Policy to apply to many systems simultaneously. In this lab, we configure it locally on the Client VM for simplicity and to demonstrate the fundamental permission setting.
</p>
<br />




<h3>Step 2: Provision multiple Domain users</h3>

<p>
<img src="https://i.imgur.com/eZI1rtJ.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-Login to the Domain controller VM as the Domain admin user -> navigate to "PowerShell ISE" as an admin -> click on "New Script" (top left) -> save the file on the "Desktop" -> Paste the content of this                           
[script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) in the script input window -> modify the number of account provisioning from 10000 to 1000 -> click on "Run Script (F5)"; you should see users being created on the PowerShell window.

-Take note of the password in the script as we will need it in the Step 3.

</p>
<br />


<p>
<img src="https://i.imgur.com/ge7kfUv.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>

</p>
<br />





<h3>Step 3: Test login as a Domain user</h3>

<p>
<img src="https://i.imgur.com/KqwQLRc.png" height="30%" width="60%" alt="Configuration step"/>
</p>
<p>

</p>
<br />




<h3>Step 4:</h3>

<p>
<img src="" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>

</p>
<br />




<h3>Step 5:</h3>

<p>
<img src="" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>

</p>
<br />


<p>
<img src="" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>

</p>
<br />




<h2>Active Directory User Provisioning and Group Policy Object Configuration completed!</h2>

<b>We've successfully configured Remote Desktop for non-administrative users, automated user creation with PowerShell, and managed group policies. Additionally, we covered account lockouts and log monitoring to simulate a real-life IT environment. This marks the end of the Active Directory Lab serie! Remember to stop the VMs in the Azure Portal when not in use to manage costs effectively.</b>
<br />
<br />
</p>
