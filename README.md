<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory - User Provisioning, Group Policy, and Account Management</h1>
In this guided lab, we will demonstrate how to enable Remote Desktop access for non-administrative users, automate user creation with PowerShell, and manage group policies. Additionally, we will cover account lockouts, password reset, and log monitoring, to simulate a real-life IT environment.<br />

<br />This project is a continuation of [Active Directory: Deployment and Configuration](https://github.com/YohanLB09/Active-Directory-Deployment-and-Configuration), so this project picks up where we left off.<br />
<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop 
- PowerShell ISE
- Group Policy Management
- Active Directory Users and Computers 

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro, version 22H2

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Enable Remote Desktop Connection for Domain users on the Client VM
- Step 2: Provision multiple Domain users
- Step 3: Test login as a Domain user
- Step 4: Configure account lockout in Group Policy Management
- Step 5: Update the Account Lockout Policy on the Client VM
- Step 6: Test Account Lockout Policy
- Step 7: Test the Password reset functionality
- Step 8: Test Disabling and Re-enabling an account
- Step 9: Observe the logs

<h2>Project Walkthrough</h2>

<h3>Step 1: Enable Remote Desktop Connection for Domain users on the Client VM</h3>

<p>
<img src="https://i.imgur.com/itskafQ.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-Login to the Client VM as the Domain admin user created previously (katy_ admin in my case) -> right-click on the Start menu, and click on "System" to access the properties -> "Remote Desktop" -> "Select users that can remotely access this PC" -> "Add" ->  type "Domain Users" -> click on "Check Names" -> "OK" -> "OK".

In this step, we logged in as the Domain admin user because administrative privileges are required to modify system-level settings, such as configuring who can remotely access the computer. Subsequently, we enabled Remote Desktop Connection for Domain users on the Client VM to allow non-administrative Domain users to remotely log into this specific client machine for accessing their newly created accounts and basic domain functionality. Domain users will be provisioned in the following step. 

Keep in mind that in a real-world scenario, this step would typically be done centrally using Group Policy to apply to many systems simultaneously. In this lab, we configure it locally on the Client VM for simplicity and to demonstrate the fundamental permission setting.
</p>
<br />




<h3>Step 2: Provision multiple Domain users</h3>

<p>
<img src="https://i.imgur.com/eZI1rtJ.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-Login to the Domain controller VM as the Domain admin user -> navigate to "PowerShell ISE" as an admin -> click on "New Script" (top left) -> save the file on the "Desktop" -> Paste the content of the provided script (see below ↓) -> modify the number of account provisioning from 10000 to 1000 -> click on "Run Script (F5)"; you should see users being created on the PowerShell window below.

You can find the PowerShell script [here.](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) 

-Take note of the user's password in the script as we will need it in the Step 3.
</p>
<br />


<p>
<img src="https://i.imgur.com/ge7kfUv.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-Go to the "_EMPLOYEE" Organizational Unit (OU) in Active Directory Users and Computers (ADUC) to ensure that domain users are being provisioned. If you don't see the account being created, refresh the OU.

Step 2 involves using a PowerShell script, run with Domain admin privileges on the Domain Controller VM, to automatically create multiple domain user accounts. The purpose is to efficiently generate a batch of users within the Active Directory (_EMPLOYEE OU), which will be used for testing remote access in the following step. Checking ADUC confirms the successful execution of the script and the creation of the new user accounts. 

Do you think that this step would have also worked if we executed it on the Client VM as the Domain Admin user?
</p>
<br />





<h3>Step 3: Test login as a Domain user</h3>

<p>
<img src="https://i.imgur.com/KqwQLRc.png" height="30%" width="60%" alt="Configuration step"/>
</p>
<p>
-Attempt to log into the Client VM with one of the accounts (use the password from script to login).

Step 3 involves attempting to log into the Client VM using the credentials of one of the non-administrative domain users created in Step 2. The purpose is to verify that the newly provisioned user accounts are valid, can authenticate against the domain, and that the Remote Desktop access configured in Step 1 is working correctly for standard domain users. Successfully logging in confirms the end-to-end functionality of user provisioning and remote access setup. 
</p>
<br />




<h3>Step 4: Configure account lockout in Group Policy Management</h3>

<p>
<img src="https://i.imgur.com/9p5mDN3.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-From the Domain Controller VM, type the "Windows key" + "R" and run "gpmc.msc" to access Group Policy Management Console.

-In the Group Policy Management Console, expand the following:
"Forest: mydomain.com" -> "Domains" -> "mydomain.com" -> "Group Policy Object".

-Right-click on "Default Domain Policy" -> select "Edit". 
</p>
<br />


<p>
<img src="https://i.imgur.com/R4txqca.png" height="100%" width="100%" alt="Configuration step"/>
<p>
-In the Group Policy Management Editor, expand the following:
"Computer Configuration" -> "Policies" -> "Windows Settings" -> "Security Settings" -> Account "Policies" -> "Account Lockout Policy".

-Configure the following Account Lockout Policy settings like so:
1) Account lockout duration: "30 minutes" 
2) Account lockout threshold: "5 invalid logo attempts"
3) Allow Administrator account lockout: "Enabled"
4) Reset account lockout counter after: "10 minutes" 

Step 4 involves configuring the Account Lockout Policy in Group Policy Management to enhance security by defining the conditions under which user accounts will be locked out to prevent brute-force attacks. By setting the "Account lockout duration" to 30 minutes, accounts locked due to too many failed login attempts will automatically unlock after this period. The "Account lockout threshold" of 5 invalid login attempts determines that after five incorrect password entries, the account will be locked. Enabling "Allow Administrator account lockout" applies this security measure to administrator accounts as well. Lastly, setting "Reset account lockout counter after" 10 minutes means that if a user has a failed login attempt, the counter will reset to zero after 10 minutes of no further failed attempts, preventing accidental lockouts from a few scattered incorrect attempts over a longer duration.
</p>
<br />




<h3>Step 5: Update the Account Lockout Policy on the Client VM</h3>

<p>
<img src="https://i.imgur.com/F1mc1ZP.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-Log back in the Client VM as the Domain Admin user -> open "Command Prompt" as an admin -> run the "gpupdate /force" command.
</p>
<br />


<p>
<img src="https://i.imgur.com/O3iM5ae.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-To verify the policy, you can use the rsop.msc (Resultant Set of Policy) tool on a client machine to see the applied settings.

Step 5 ensures that the Account Lockout Policy, configured on the Domain Controller, is immediately applied to the Client VM. Running gpupdate /force forces a refresh of all Group Policy settings on the client, overriding the default background refresh interval. Subsequently, using rsop.msc allows us to verify that the Account Lockout Policy settings have been successfully applied to the Client VM by displaying the resultant set of policies for the logged-on user and computer.
</p>
<br />




<h3>Step 6: Test Account Lockout Policy</h3>

<p>
<img src="https://i.imgur.com/xyBMxNn.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-Attempt to log in to a randomly chosen Domain user on the Client VM 6 times with a bad password. 
After 6 failed logins, you should get an error pop-up window indicating that the account has been locked, proving that the Account Lockout Policies have been correctly applied.
</p>
<br />


<p>
<img src="https://i.imgur.com/3E7IRIk.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
To unlock the account, log back in the Domain Controller VM as the Domain Admin User -> navigate to the ADUC Console -> right-click on the "_EMPLOYEE" OU, select "Find" -> enter the name of the domain user -> click on "Find Now" -> double-click on the Domain user's name.

-From within the Domain user's properties, navigate to the "Account" tab and check the box for "Unlock account. This account is currently locked out on this Azure Directory Domain Controller" -> "Apply" -> "OK".

-Attempt to log back in the Client VM as the Domain user.

Step 6 serves to verify that the configured security measure against brute-force attacks on user accounts is functioning correctly. By intentionally failing to log in multiple times, you should trigger the account lockout, as indicated by an error message. Subsequently, the process of unlocking the account through the Domain Controller demonstrates the administrative control over user access and confirms the lockout policy can be reversed by authorized personnel. Finally, successfully logging in after unlocking confirms the account is no longer restricted.
</p>
<br />




<h3>Step 7: Test the Password reset functionality</h3>

<p>
<img src="https://i.imgur.com/UuCzsjj.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-To reset a password for a Domain user, from within the Domain Controller VM -> go back to the ADUC Console -> right-click on the "_EMPLOYEE" OU and select "Find" -> enter the name of the Domain user -> click on "Find Now" -> right-click on the Domain user's name -> "Reset Password" -> enter a new password for the Domain user (take note of it).

-Attempt to log back in the Client VM as the Domain user with the new password. 

-Log out of the Client VM.

Step 7 aims to verify that the password reset mechanism within the Active Directory environment is functioning correctly. By navigating to the ADUC console on the Domain Controller, locating a specific domain user within the "_EMPLOYEE" OU, and then successfully resetting their password, you confirm that administrative password resets are being applied. Subsequently, logging into the Client VM with the newly set password validates that the changes made on the Domain Controller are being properly propagated and authenticated for domain users.
</p>
<br />




<h3>Step 8: Test Disabling and Re-enabling an account</h3>

<p>
<img src="https://i.imgur.com/HAuWI7H.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-To disable an account for a Domain user, from within Domain Controller VM, go back to the ADUC Console -> right-click on the "_EMPLOYEE" OU and select "Find" -> enter the name of the Domain user -> click on "Find Now" -> right-click on the Domain user's name -> "Disable Account".
</p>
<br />


<p>
<img src="https://i.imgur.com/f5eunsH.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-When you attempt to login the Client VM as the Domain user, you will notice that the account is disabled.
</p>
<br />


<p>
<img src="https://i.imgur.com/i7i5G8R.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-To re-enable the account, go back to the Domain Controller VM as the Domain Admin user  -> navigate to the same location as the one where we disabled the Domain user's account  -> click on "Enable Account".

-You should be able to successfully log back in the Domain user's account on the Client VM.

-Log out of the Client VM.

Step 8 verifies the functionality of disabling and enabling domain user accounts within Active Directory. By disabling a specific user account in the Domain Controller and observing the inability to log in on the Client VM, you confirm that the disable operation is effective. Subsequently, re-enabling the account and successfully logging in validates the enable operation and the restoration of account access. This process ensures the administrator can effectively control user access to the domain resources.

FYI: Disabling the domain user's account will not disconnect the current session and disable the account right away. It will only prevent the user from logging in the next time they attempt to do so. The current session will remain active until the user logs out or the system is restarted.
</p>
<br />




<h3>Step 9: Observe the logs</h3>

<p>
<img src="https://i.imgur.com/bspW3zi.png" height="100%" width="100%" alt="Configuration step"/>
</p>
<p>
-To observe the logs for the Domain user's login attempts, login to the Client VM as the Domain admin user  -> type the "Windows key" + "R", and run "eventvwr" to access the Event Viewer Console.

-Expand "Windows Logs" -> right-click on "Security" and select "Find" -> type "Audit Failure" and search for subsequent failed login attempts.

Step 9 aims to examine the security logs on the Client VM to observe records of failed login attempts by the domain user. By accessing the Event Viewer and filtering for "Audit Failure" entries within the Security logs, you can verify that attempts to log in are being recorded. This confirms that the system is correctly auditing and logging unsuccessful/successful authentication attempts, which is crucial for security monitoring and troubleshooting.
</p>
<br />




<h2>Active Directory - User Provisioning, Group Policy, and Account Management completed!</h2>

<b>We've successfully configured Remote Desktop for non-administrative users, automated user creation with PowerShell, and managed group policies. Additionally, we covered account lockouts, password reset and log monitoring to simulate a real-life IT environment. This marks the end of the Active Directory Lab series! Remember to stop the VMs in the Azure Portal when not in use to manage costs effectively.</b>
<br />
<br />
</p>
