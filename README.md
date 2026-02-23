# Active Directory — Managing Users, Groups, and Password Policies

## Overview

In this lab, I will be using a domain controller that I have already configured. I will start by creating organizational units for the users I will be creating. Note that I have already created an OU for "Users."

---

## Stage 1: Creating Organizational Units

First, I navigated to **Active Directory Users and Computers**. I then right-clicked `mydomain.com` and selected **New → Organizational Unit**. I labeled the OU as `Groups`.

---

## Stage 2: Creating Groups

Once the `Groups` OU was created, I right-clicked **Groups → New → Group** and was brought to a new window. In the window below, I named the group `HR-Team`, kept the scope as **Global**, and kept the group type as **Security**. I followed the same steps to create another group called `IT-Helpdesk`, as shown below.

---

## Stage 3: Creating Users

To create a new user, I right-clicked on `_USERS` **→ New → User**. Once brought to the **New Object - User** window, I populated the first name, last name, full name, and logon name. I created two users called John Doe and Alice Smith and configured passwords for both of them.

---

## Stage 4: Assigning Permissions

Next, I will be assigning permissions to groups. It is important to assign permissions to groups rather than individual users. In Identity and Access Management, there is a principle called **Role-Based Access Control (RBAC)**. It is best practice to avoid assigning permissions directly to a person. Instead, permissions are assigned to a group that represents a role, and users are then added to that group. For example, if someone changes roles, you can simply move them between groups rather than modifying every individual permission.

I will add `jdoe` to the `HR-Team` group and `asmith` to the `IT-Helpdesk` group.

From the `Groups` OU, I double-clicked the `HR-Team` group. Once the **HR-Team Properties** window opened, I navigated to the **Members** tab and clicked **Add**. I typed in `jdoe`, clicked **Check Names**, and then clicked **OK → Apply**. `jdoe` is now part of the `HR-Team` group. I repeated the process to add `asmith` to the `IT-Helpdesk` group. Their identities are now linked to a role.

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
