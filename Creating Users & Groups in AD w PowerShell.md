
# Creating Users, Groups, and Organizational Units in Active Directory Using PowerShell
A beginner-friendly guide to organizing a directory environment using PowerShell.

## Introduction
This project demonstrates how a simple company structure can be created inside Active Directory using PowerShell.
Active Directory can be imagined as a digital building. Inside this building are:
- Organizational Units (department folders)
- Users (employees)
- Security Groups (teams used for permissions)

This guide explains each command in detail so even someone with zero IT experience can understand every part of the process.

## Step 1: Opening PowerShell in Administrator Mode
PowerShell must be opened in Administrator mode before modifying Active Directory.
Administrator mode provides elevated permission, similar to holding a master key that unlocks restricted areas of the building.

## Step 2: Creating Organizational Units (OUs)
Organizational Units act as department folders. Three departments are created: HR, IT, and Finance.

### Generic OU Command
```
New-ADOrganizationalUnit -Name <OUName>
```

### Explanation
- New-ADOrganizationalUnit  
  Creates a new department folder inside Active Directory.
- -Name  
  Specifies the name of the folder.

### Commands Used
```
New-ADOrganizationalUnit -Name "HR_Department"
New-ADOrganizationalUnit -Name "IT_Department"
New-ADOrganizationalUnit -Name "Finance_Department"
```

## Step 3: Creating User Accounts
User accounts represent employees in the company.

### Generic User Command
```
New-ADUser <Name>
```

### Explanation
- New-ADUser creates a new user inside Active Directory.
- The quoted text becomes the user's name.

### Commands Used
```
New-ADUser "Petter Griffin"
New-ADUser "Thomas Jefferson"
New-ADUser "John Cena"
```

## Step 4: Retrieving the Distinguished Name (DN)
A Distinguished Name is a full address that shows where an object is located inside Active Directory.

### Generic DN Command
```
Get-ADUser -Identity <UserName> | Select-Object DistinguishedName
```

### Explanation
- Get-ADUser retrieves user information.
- -Identity specifies which user to find.
- The pipe symbol | sends results to another command.
- Select-Object DistinguishedName displays only the DN.

### Example
```
Get-ADUser -Identity "Petter Griffin" | Select-Object DistinguishedName
```

## Step 5: Moving Users Into Their Correct OUs
Users are placed into the correct department folders.

### Generic Move Command
```
Move-ADObject -Identity <CurrentDN> -TargetPath <DestinationDN>
```

### Explanation
- Move-ADObject moves an AD object to a new folder.
- -Identity is the object’s current address.
- -TargetPath is the destination folder.

### Commands Used
```
Move-ADObject -Identity "CN=Petter Griffin,CN=Users,DC=david,DC=local" -TargetPath "OU=IT_Department,DC=david,DC=local"
Move-ADObject -Identity "CN=Thomas Jefferson,CN=Users,DC=david,DC=local" -TargetPath "OU=Finance_Department,DC=david,DC=local"
Move-ADObject -Identity "CN=John Cena,CN=Users,DC=david,DC=local" -TargetPath "OU=HR_Department,DC=david,DC=local"
```

## Step 6: Creating Security Groups
Security groups allow shared permissions for department members.

### Generic Group Command
```
New-ADGroup -Name <GroupName> -GroupScope <Scope> -GroupCategory <Category> -Path <OU_DN>
```

### Explanation
- New-ADGroup creates a group object.
- -GroupScope Global is used for domain users.
- -GroupCategory Security allows permission assignments.
- -Path sets the group location.

### Commands Used
```
New-ADGroup -Name "IT_Admins" -GroupScope Global -GroupCategory Security -Path "OU=IT_Department,DC=david,DC=local"
New-ADGroup -Name "HR_Security_Group_Team" -GroupScope Global -GroupCategory Security -Path "OU=HR_Department,DC=david,DC=local"
New-ADGroup -Name "Finance_Security_Group_Team" -GroupScope Global -GroupCategory Security -Path "OU=Finance_Department,DC=david,DC=local"
```

## Step 7: Adding Users to Security Groups
Each user is added to the correct team.

### Generic Add Command
```
Add-ADGroupMember -Identity <GroupName> -Members <UserName>
```

### Explanation
- Add-ADGroupMember places an object into a group.
- -Identity specifies the group.
- -Members specifies the user.

### Commands Used
```
Add-ADGroupMember -Identity "HR_Security_Group_Team" -Members "John Cena"
Add-ADGroupMember -Identity "Finance_Security_Group_Team" -Members "Thomas Jefferson"
Add-ADGroupMember -Identity "IT_Admins" -Members "Petter Griffin"
```

## Step 8: Verification
Verifying ensures all users are correctly placed.

### Generic Verification Command
```
Get-ADUser -Identity <UserName> | Select-Object Name, DistinguishedName
```

## Final Result
The directory now contains:
- Three OUs (HR, IT, Finance)
- Three users assigned to the correct OUs
- Three security groups
- Users successfully added to their groups

This structure forms the basis of proper Active Directory organization and role-based access control.
