# Mikrotik

## Netinstall cli linux
Install/reInstall routeros with netinstall-cli.

- Download netinstall and main package based on your hardware from [mikrotik.com](https://mikrotik.com/download)
- Set ip address 192.168.88.2 on your host
- Run netinstall-cli

```
./netinstall-cli -r -a 192.168.88.1 ./routeros-7.12.1-mipsbe.npk 
```

- Hold the reset botton and poweron the device
- Wait till end of the process
