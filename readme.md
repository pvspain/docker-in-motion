# Docker in Motion

My notes for the Manning video course, [Docker In Motion][1]

## Introduction 

This course does not cover the installation of Docker. I've followed the [official installation notes][2] from Docker on Ubuntu/Debian Linux environments.

The [Linode][3]-tailored kernel required some additional tweaking (as root/sudo):

- Docker CE version: `docker-ce/bionic,now 5:18.09.0~3-0~ubuntu-bionic amd64` 
- Linux kernel: `Linux thelonius 4.19.8-x86_64-linode120 #1 SMP PREEMPT Mon Dec 10 18:25:47 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux`
```
# SERVICEDIR=/etc/systemd/system/containerd.service.d
# mkdir -p $SERVICEDIR
# cat << EOF > $SERVICEDIR/override.conf
[Service]
ExecStartPre=
EOF

# systemctl daemon-reload
# systemctl start docker
```




[1]: https://www.manning.com/livevideo/docker
[2]: https://docs.docker.com/install/linux/docker-ce/ubuntu/
[3]: https://www.linode.com/