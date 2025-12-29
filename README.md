# Active-Directory-Domain-GPO-Hardening-Lab

## Objective 

The objective of this lab was to demonstrate core Windows system administration skills, including deploying and configuring a Windows Server as a Domain Controller, managing Active Directory users and Organizational Units, creating and applying Group Policy Objects, and enforcing domain-wide password policies. Additionally, the lab involved joining a Windows 10 client to the domain and validating policy enforcement, simulating real-world enterprise administration tasks.



### Skills Learned
- Active Directory Domain Services 
- User Creation and management in Active Directory
- Group Policy Objects creation and enforcement 
- Identity security 
- least privilege




### Lab Environment 
- Virtual box 7.2
- Domain Controller : Windows sever 2022
- Client Machines : Windows 10
- DC01: 192.168.56.10
- CLIENT1 : 192.168.56.20



## Steps

To save time here, I have configured the Windows server 2022 and Windows 10 home in Oracle's VirtualBox. 

The Windows Server in this lab will serve as the Domain Controller (DC). To manage users and resources in Active Directory, the DC must be reachable by client machines on the network. For this lab, I configured the server with a static IP address of 192.168.56.10, ensuring that clients can reliably communicate with it.

<img width="640" height="560" alt="Screenshot 2025-12-28 125131" src="https://github.com/user-attachments/assets/1ffdfeb1-1e83-4351-8f33-3f5006218639" />

In server manager, I will install Active Directory on the Domain Controller by clicking manage → Add Roles and features → and select active directory in server roles. Then proceed with the install.

<img width="898" height="689" alt="Screenshot 2025-12-28 130655" src="https://github.com/user-attachments/assets/4ce98ce6-b666-4890-ac60-fb15388418ca" />

After the installation completes, there will be a notification with a flag icon. i will click on "Promote this server to a domain controller" which is indicative in the name.

<img width="588" height="412" alt="Screenshot 2025-12-28 131045" src="https://github.com/user-attachments/assets/212b042d-9aad-41f3-939c-fa5616532357" />

From there, I will select New forest and enter a root domain name which will be lab.local.

<img width="894" height="629" alt="Screenshot 2025-12-28 131340" src="https://github.com/user-attachments/assets/62d979e7-4414-433f-bf27-8c3bd358f30d" />

I'll let that install and restart. Once finished, I will go to the command line and check if the domain is set. As shown below, the domain is set to lab.local

<img width="379" height="132" alt="Screenshot 2025-12-28 134421" src="https://github.com/user-attachments/assets/019045b2-0e2b-47ad-8c8d-c0956dfd176c" />


I can go on to create users. I will do so by heading to Active Directory Users and Computers, expanding the domain, right-click users → add user. From there, i can add the users name and set a temporary password. 

<img width="476" height="504" alt="Screenshot 2025-12-28 134912" src="https://github.com/user-attachments/assets/7f61c682-02c3-4883-a1eb-5e059dc59c65" />

<img width="617" height="448" alt="Screenshot 2025-12-28 134944" src="https://github.com/user-attachments/assets/737f7dd9-1191-4774-9056-ebbdbbfc4e4f" />

I will now create 2 Group Policy Objects (Desktop restrictions and Password Policy) and apply those to this user. In order to do that, I have to create an Organizational Unit for the HR department, that way- i can link the policy to everyone in the HR department rather than the whole domain. This is the magic of group policy; in-that it prevents GPO chaos, and can force a policy to thousands of users in just a few steps. To get started, i will head to the ADUC, and right-click the domain, and select new Organizational Unit. I'll name it HR. 


<img width="533" height="392" alt="Screenshot 2025-12-28 142525" src="https://github.com/user-attachments/assets/1f498b93-cc12-4e73-81cd-ea8be721f5c1" />

In Group Policy Management, I will create a new GPO for the HR OU and name it Desktop Restrictions.

<img width="303" height="116" alt="Screenshot 2025-12-28 142720" src="https://github.com/user-attachments/assets/ea15bbfa-8cba-4750-882e-b06c5af56a1c" />

<img width="583" height="249" alt="Screenshot 2025-12-28 143212" src="https://github.com/user-attachments/assets/3ca34a5f-4a48-48ef-b8fa-f69af78eff1f" />

Once created, I will edit said GPO and essentially tell it what to enforce for this policy by navigating to User Configuration → Policies → Administrative Templates → Control Panel → Personalization and enabling this setting which restricts users from changing the desktop background.

<img width="852" height="550" alt="Screenshot 2025-12-28 170448" src="https://github.com/user-attachments/assets/2f95356c-dd39-4d49-bcfb-32f2432ffac6" />


I will set the GPO for the password policy. I will not create a new GPO because password policies are set at the the domain level. To set the password policy, I will right-click the default domain policy → edit

<img width="564" height="194" alt="Screenshot 2025-12-28 144450" src="https://github.com/user-attachments/assets/235f49ea-0f79-4236-b544-8cb0b3482d5e" />


Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy 


<img width="785" height="395" alt="Screenshot 2025-12-28 145027" src="https://github.com/user-attachments/assets/7e250727-371b-440b-af08-acee8192adb7" />

I will change the minimum password requirements from 7 to 14.

<img width="743" height="483" alt="Screenshot 2025-12-28 175320" src="https://github.com/user-attachments/assets/31d2e756-6569-4d61-9079-cfd356553737" />



Once i've finished editing the GPO's, I will force an update by going to the command line and typing    gpupdate /force

<img width="464" height="172" alt="Screenshot 2025-12-28 145517" src="https://github.com/user-attachments/assets/7cf8a73f-4167-481c-b362-dacdc5f0beab" />

I will now get the client attached to the network by giving it a static IP address of 192.168.56.20. and set the preferred DNS to the DC's IP so that the client can locate the DC.


<img width="571" height="361" alt="Screenshot 2025-12-28 152924" src="https://github.com/user-attachments/assets/2ae6a684-3ae6-4a4b-840a-28a295c13b82" />


Before we can login with the HR admin's account, I will have to add this machine to the domain. Since the IP is set, i can go to system properties and add this client1 machine to the lab.local domain. I will be prompted for the login credentials for the DC to make these changes. 

<img width="524" height="503" alt="Screenshot 2025-12-28 162526" src="https://github.com/user-attachments/assets/a789e083-6470-4252-8c06-a76a86213daa" />

<img width="391" height="206" alt="Screenshot 2025-12-28 163014" src="https://github.com/user-attachments/assets/4d225cc7-8074-4e12-bc16-64671c7dc8f1" />

 I'll restart and go login to the HR admin account to verfy that the GPO's have been applied. Here below, is the HR admin's background. A stock photo of a person snorkeling in the sea.

<img width="905" height="599" alt="Screenshot 2025-12-28 173701" src="https://github.com/user-attachments/assets/9c6d7f3c-c7b0-4723-a91c-301d980e23d7" />
 
I will try to change the backgroung 

<img width="703" height="672" alt="Screenshot 2025-12-28 174447" src="https://github.com/user-attachments/assets/881e5ecd-a2b8-4ca5-b005-9b4907e8bc1c" />

Nice! the GPO went through and now you can see that the desktop background policy is applied and the user (HrAdmin) cannot change the background now that the changes were forced through. 

I will go ahead and test my password policy by trying to change the password for this account to something under 14 characters. 
Note- the existing password will remain the same until the user tries to change the password or when it has expired. 


<img width="754" height="374" alt="Screenshot 2025-12-28 180431" src="https://github.com/user-attachments/assets/ab55c0ea-43e0-49ec-a042-9d93074f01b4" />


<img width="814" height="386" alt="Screenshot 2025-12-28 180439" src="https://github.com/user-attachments/assets/ba731e8f-5de5-4656-a38e-e29a272de9b3" />


And it works! I now have to meet the minimum of 14 characters.


## Conclusion

This lab successfully demonstrated the setup and management of a Windows Server domain environment. Key skills practiced included Active Directory administration, OU and user management, GPO creation and linking, client domain join, and verification of policy enforcement. The lab highlights the ability to configure a secure, organized, and scalable network environment
