# Active Directory — Managing Users, Groups, and Password Policies

## Summary

In this lab, I expanded on my Active Directory home lab by practicing core identity and access management tasks that are commonly performed in real-world IT environments. Using a pre-configured domain controller (Check out [Mini Corporate Lab](https://github.com/AdrianSVT/Active-Directory-Mini-Corporate-Environment-Lab)), I demonstrated how to organize directory objects by creating organizational units, provisioning user accounts, and setting up security groups. I then applied Role-Based Access Control (RBAC) principles by assigning users to groups that reflect their designated roles within the organization rather than assigning permissions directly to individuals. Finally, I reinforced the domain's security posture by configuring a centralized password policy through Group Policy Management, enforcing minimum password length and complexity requirements across all domain users. Each step was verified using PowerShell to confirm that all configurations were applied correctly. . Note that I have already created an OU for "Users" since it is the exact same process.

---

## Stage 1: Creating Organizational Units

First, I navigated to **Active Directory Users and Computers**. I then right-clicked `mydomain.com` and selected **New → Organizational Unit**. 

<img width="311" height="272" alt="riight click domain new OU" src="https://github.com/user-attachments/assets/b02b1cdb-05c3-4e2f-b12d-5e77f5338705" />

In the next window, I labeled the OU as `Groups`.

<img width="224" height="199" alt="naming OU Groups" src="https://github.com/user-attachments/assets/f3f9a591-bec3-448b-8767-8cf31c59f43c" />

---

## Stage 2: Creating Groups

Once the `Groups` OU was created, I right-clicked **Groups → New → Group**. This is important because by creating a dedicated OU for groups, it becomes much easier to locate, manage, and apply policies to them without interfering with other objects in the domain.      After creating the **Group** , I was brought to a new window. In the window below, I named the group `HR-Team`, kept the scope as **Global**, and kept the group type as **Security**. 

<img width="223" height="198" alt="new group in group HR-team" src="https://github.com/user-attachments/assets/2bed18c0-af06-4feb-a4d7-e21aa2bffb48" />

I followed the same steps to create another group called `IT-Helpdesk`, as shown below.

<img width="219" height="199" alt="New group in group It-helpdesk" src="https://github.com/user-attachments/assets/a32e0e9e-7103-4f3c-8b3e-9024aea7732b" />

---

## Stage 3: Creating Users

To create a new user, I right-clicked on `_USERS` **→ New → User**. Once brought to the **New Object - User** window, I populated the first name, last name, full name, and logon name. I created two users called John Doe and Alice Smith and configured passwords for both of them on the next window.

<img width="224" height="197" alt="new user john doe" src="https://github.com/user-attachments/assets/aa29e00f-af1a-4d93-bff4-abdbc06cda02" />
<img width="225" height="196" alt="new user alice smiith" src="https://github.com/user-attachments/assets/6c1f7544-20a5-4fbd-9a63-97d16468023c" />

I have now successfully created two new users.

---

## Stage 4: Assigning Users to Groups

Next, I will be assigning users to groups. It is important to assign permissions to groups rather than individual users. In Identity and Access Management, there is a principle called **Role-Based Access Control (RBAC)**. It is best practice to avoid assigning permissions directly to a person. Instead, permissions are assigned to a group that represents a role, and users are then added to that group. For example, if someone changes roles, you can simply move them between groups rather than modifying every individual permission.

I will add `jdoe` to the `HR-Team` group and `asmith` to the `IT-Helpdesk` group.

From the `Groups` OU, I double-clicked the `HR-Team` group. Once the **HR-Team Properties** window opened, I navigated to the **Members** tab and clicked **Add**. I typed in `jdoe`, clicked **Check Names**, and then clicked **OK → Apply**. `jdoe` is now part of the `HR-Team` group. 

<img width="245" height="233" alt="typed jdoe check names" src="https://github.com/user-attachments/assets/0d3b5d19-fed1-4fc9-b20c-420ff1104b4d" />
<img width="209" height="245" alt="Jdoe is part of Hr team click apply" src="https://github.com/user-attachments/assets/db28b42e-21a2-473d-b5fc-2300e9381c9a" />


I repeated the same process to add `asmith` to the `IT-Helpdesk` group. Their identities are now linked to a role.

<img width="245" height="236" alt="asmith check name" src="https://github.com/user-attachments/assets/99b80cdf-ccb4-4697-946e-9ae96cd22592" />
<img width="216" height="236" alt="asmith part of it helpdesk" src="https://github.com/user-attachments/assets/d89ea8eb-eebb-4d3e-82b0-e6b4b1590e56" />





---

## Stage 5: Configuring a Password Policy

Next, I will lock down the security baseline by editing the Default Domain Policy in Group Policy Management. I will set a policy requiring all passwords to have a minimum length of 12 characters with complexity enabled.

Since this will be configured in the Default Domain Policy, every user in the domain will automatically be held to these rules. The domain controller enforces it, making it a centralized security policy.

First, I navigated to:

**Server Manager Dashboard → Tools → Group Policy Management**

I then expanded **Forest: mydomain.com → Domains → mydomain.com**, right-clicked **Default Domain Policy**, and chose **Edit**.

Next, I navigated the following path:

**Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy**

I double-clicked **Minimum Password Length**, set it to `12`, and clicked **Apply**. I then double-clicked **Password must meet complexity requirements** and set it to **Enabled**.

---

## Stage 6: Verification

Lastly, I confirmed everything was configured correctly using PowerShell. I ran the following command to load the Active Directory module:

```powershell
Import-Module ActiveDirectory
```

I then ran the following commands to verify each user's group membership:

```powershell
Get-ADUser jdoe
Get-ADGroupMember "HR-Team"

Get-ADUser asmith
Get-ADGroupMember "IT-Helpdesk"
```

As shown below, both users were configured correctly.
