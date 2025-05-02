
```bash
ssh username@ipaddress
```

With key
```bash
ssh -i id_rsa root@192.168.100.200
```

If require ssh-dss or any other algorithm
```bash
ssh -oHostKeyAlgorithms=+ssh-dss username@ipaddress
```

SSH-wide compatibility (system-wide)
https://github.com/vanhauser-thc/thc-hydra/issues/792