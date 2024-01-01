# Installation

## Requirements

* Docker API > 1.41
* Chrome or Firefox
* cgroup2fs
* max_user_watches: 2147483647
* max_user_instances: 2048
* max_queued_events: 2147483647
* selinux: disabled
* firewalld: inactive

## Download
```bash
git clone https://git.eadem.com/alex/qbo-home.git
cd qbo-home
```

##  Configure
> You can use the `check_config` script to see if your system is ready for Qbo

```bash
./check_config
OS = Linux
info: reading kernel config from /boot/config-6.6.4-100.fc38.x86_64 ...

- cgroup hierarchy: cgroup2fs
- /proc/sys/fs/inotify/max_user_watches: 2147483647
- /proc/sys/fs/inotify/max_user_instances: 2048
- /proc/sys/fs/inotify/max_queued_events: 2147483647
- selinux: disabled
- firewalld: inactive
- OS: fedora 38
- Docker: 1.43
```


## Linux

> Star the API
```bash
./qbo start api 
```

> Acces Web Interface

ADDRESS=`docker inspect qbo | jq -r .[].NetworkSettings.Networks.bridge.IPAddress`
http://$ADRESS:9601

## Windows
### Windows Subsystem for Linux (WSL)
TODO