# Nuclei

- Supports DNS, TCP Sockets and local file sharing
- Supports out of band payloads through a hosted or self-hosted server that project discovery publishes, known as Interact.sh
- Perform features such as pitchfork, cluster bomb, sniper and battering ram a la Burp Intruder

### Scan list of targets
```
nuclei -l /path/to/list-of-targets.txt
```

### Scan site with port
```
nuclei -u my.target.site:5759
```

Check out more [here](https://blog.projectdiscovery.io/ultimate-nuclei-guide/)
