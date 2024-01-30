### Map an IP address to a Cloud Service Provider

```
edge -ip /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt -prefix
```

### Masscan
```
sudo /opt/masscan/bin/masscan --adapter tun0 --open-only --source-port 40000-41023 -p 1-1024,3000,5000,6379,27017 -oB /home/sec588/Coursefiles/workdir/masscan.bin -iL /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt
```

### Masscan - Read the scans
```
/opt/masscan/bin/masscan --readscan /home/sec588/Coursefiles/workdir/masscan.bin
```
