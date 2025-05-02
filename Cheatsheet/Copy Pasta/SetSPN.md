Enumerates domain user for SPNs linked to them
```powershell
setspn -L accountname
```
> `-L` flag runs setspn on both servers and clients in the domain

Can use PowerView for this, but setspn is installed on Windows by default