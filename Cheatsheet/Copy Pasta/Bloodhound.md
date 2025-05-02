# From Kali
```bash
bloodhound-python -u username -p password -d domain -ns 192.168.100.200 -c All --zip
```
# From target
## SharpHound.exe
```powershell
.\SharpHound.exe
```
## SharpHound.ps1
```powershell
Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\ProgramData\ -OutputPrefix "target_sh"
```

```powershell
copy SharpHoundOutput.zip z:\
```
> Don't need the bin file

```bash
sudo neo4j start
```

```bash
bloodhound
```

Drag and drop the zip file
Password on SharpHound generated zip files can be set via the `-zipPassword` flag

## Loop if required, CTRL+C to stop collecting data
Option is `--Loop`

More info in the documentation : 
https://bloodhound.readthedocs.io/en/latest/data-collection/sharphound.html