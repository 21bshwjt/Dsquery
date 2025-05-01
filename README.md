## Dsquery

- [Dsquery](https://learn.microsoft.com/en-us/archive/technet-wiki/2195.active-directory-dsquery-commands![image](https://github.com/user-attachments/assets/86948d9e-a4eb-4904-989e-d72adc26b28e)
)

####
Get All Properties
```powershell
dsquery * "CN=Biswas, Biswajit,OU=Users,DC=americas,DC=contoso,DC=com" -scope base -attire *
```
#### Find out Account Expiry Date
```powershell
dsquery user -name * -limit 0 | dsget user -samid -acctexpires
```

#### Get all sAMAccount names
```powershell
dsquery user -o rdn -limit 0
```

#### Retrieve the DN of all users in the domain that are not direct members of a specified group
```powershell
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(!(memberOf=Groupname,ou=West,dc=Contoso,dc=com))) -limit 0 > NotInGroup.txt
```

#### Find all contacts from an organizational unit
```powershell
dsquery contact OU=Sales,DC=Contoso,DC=Com
```

#### List of all users with primary group "Domain Users"
```powershell
dsquery * -filter "(primaryGroupID=513)" -limit 0
(You can change the "primaryGroupID" as per your requirement)
513:Domain Users
514:Domain Guests
515:Domain Computers
516:Domain Controllers
```

#### Find all members for a particular group
```powershell
dsget group "<DN of the group>" -members
```

#### Find all groups for a particular member (including nested groups)
```powershell
dsget user "<DN of the user>" -memberof -expand
dsquery user -samid "username" | dsget user -memberof -expand
```

#### Get the Groups name form Users container
```powershell
dsquery group -o rdn cn=users,dc=contoso,dc=com
```

#### Get the members from a Group
```powershell
dsquery group -samid "CS_CLUB_ACCOUNTS" | dsget group -members -expand | dsget user -samid
```

#### Find disabled users
```powershell
dsquery user "dc=ssig,dc=com" -disabled
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))"
```

#### Find all the active users
```powershell
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(!userAccountControl:1.2.840.113556.1.4.803:=2))"
```

#### Find all groups for a OU
```powershell
dsquery group ou=targetOU,dc=domain,dc=com
```

#### To get the members status from the active directory group
```powershell
dsquery group -samid “Group Pre-Win2k Name” | dsget group -members | dsget user -disabled -display
```

#### Extract the all groups from an OU with Group Scope & Group Type
```powershell
dsquery group "ou=test,dc=gs,dc=com" -limit 0 | dsget group -samid -scope -secgrp
```

#### ADDS existing connection point objects
```powershell
dsquery * forestroot -filter (objectclass=serviceconnectionpoint)
```

#### Get Tombstonelifetime
```powershell
# Command & Output
PS C:\> $env:USERDNSDOMAIN
BSHWJT.ORG
PS C:\> dsquery * "CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=bshwjt,DC=org" -scope base -attr tombstonelifetime
  tombstonelifetime
  180
PS C:\>
```

#### Find the DNS servers from all the DNS partitions
```powershell
dsquery * "CN=Configuration,DC=contoso,DC=com" -filter "(&(objectClass=crossRef)(objectCategory=crossRef)(systemFlags=5))" -attr NcName msDS-NC-Replica-Locations
```

#### Get Forestprep , Domainprep & RodcPrep
```powershell
 dsquery * CN=ActiveDirectoryUpdate,CN=ForestUpdates,cn=configuration,dc=bshwjt,dc=org -scope base -attr revision
 dsquery * CN=ActiveDirectoryRodcUpdate,CN=ForestUpdates,cn=configuration,dc=bshwjt,dc=org -scope base -attr revision
```

#### Get distinguished names of all directory partitions in the current forest
```powershell
dsquery partition
```

#### Get Subnet with associated site
```powershell
dsquery subnet -name <CIDR> | dsget subnet
```

#### Get Sites
```powershell
dsquery site -name * -limit 0
dsquery server -s <server> | dsget server -site
```

#### Get Site name by server name
```powershell
dsquery server -name test1 | dsget server -site
dsquery server -name (provide the server name for DN) | dsget server -site 
```

#### Get Schema version
```powershell
dsquery * cn=schema,cn=configuration,dc=domainname,dc=local -scope base -attr objectVersion
```

#### Get How Many Times wrong Password has been entered on a specified domain controller
```powershell
dsquery * -filter "(sAMAccountName=jsmith)" -s MyServer -attr givenName sn badPwdCount
```

#### Get the 'PSO Applies to
```powershell
dsget user <user DN> -effectivepso
```

#### Get all Hyper-V hosts in your forest
```powershell
dsquery * forestroot -filter "&(cn=Microsoft Hyper-V)(objectCategory=serviceconnectionpoint)" -attr servicebindinginformation
```

#### Get all windows virtual machine in your forest
```powershell
dsquery * forestroot -filter "&(cn=windows virtual machine)(objectCategory=serviceconnectionpoint)" -limit 0 -attr *
```

#### Get the objects for DES-Only-Encryption
```powershell
dsquery * -filter "(UserAccountControl:1.2.840.113556.1.4.803:=2097152)"
```
