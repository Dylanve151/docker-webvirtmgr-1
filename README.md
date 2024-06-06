
## Webvirtmgr Dockerfile

1. Install [Docker](https://www.docker.com/).

2. Pull the image from Docker Hub

```
$ docker pull primiano/docker-webvirtmgr
$ sudo groupadd -g 1010 webvirtmgr
$ sudo useradd -u 1010 -g webvirtmgr -s /sbin/nologin -d /data/vm webvirtmgr
$ sudo chown -R webvirtmgr:webvirtmgr /data/vm
```

### Usage

```
$ docker run -d -p 8080:8080 -p 6080:6080 --name webvirtmgr -v /data/vm:/data/vm dylanve115/webvirtmgr
```


### libvirtd configuration on the host

```
$ cat /etc/default/libvirt-bin
start_libvirtd="yes"
libvirtd_opts="-d -l"
```

```
$ cat /etc/libvirt/libvirtd.conf
listen_tls = 0
listen_tcp = 1
listen_addr = "172.17.42.1"  ## Address of docker0 veth on the host
unix_sock_group = "libvirtd"
unix_sock_ro_perms = "0777"
unix_sock_rw_perms = "0770"
auth_unix_ro = "none"
auth_unix_rw = "none"
auth_tcp = "none"
auth_tls = "none"
```

```
$ cat /etc/libvirt/qemu.conf
# This is obsolete. Listen addr specified in VM xml.
# vnc_listen = "0.0.0.0"
vnc_tls = 0
# vnc_password = ""
```

### Instructions for KVM on Peplink router to create a instance.

1. Login into the webinterface using the IP of the docker container with port 8080. (Can be used through InTouch).

2. Click on Add connection.

3. give it a label in lowercase, IP is Untagged LAN IP, Username is web admin username (default: admin) and Password is web admin password (default: admin).

4. Click on add and open it.

5. Go to networks and click on New network.

6. Give it a name, select type forwarding BRIDGE and set Bridge Name to br0.

7. Click on create.

8. Click on New instance.

9. Click choose one of the templates and click Create

10. Give it a name, use as Storage default and leave everything as is.

11. click on create.
