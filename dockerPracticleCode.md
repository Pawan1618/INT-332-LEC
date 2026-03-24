# Docker Commands & Outputs

## Run All Commands Sequentially

```bash
PS C:\Users\pawan> docker --version
Docker version 29.1.3, build f52814d

PS C:\Users\pawan> docker info
Client:
 Version:    29.1.3
 Context:    desktop-linux
 Debug Mode: false

Plugins:
 ai: Docker AI Agent - Ask Gordon (Docker Inc.)
  Version:  v1.17.1
  Path:     C:\Program Files\Docker\cli-plugins\docker-ai.exe

 buildx: Docker Buildx (Docker Inc.)
  Version: v0.30.1-desktop.1
  Path:    C:\Program Files\Docker\cli-plugins\docker-buildx.exe

 compose: Docker Compose (Docker Inc.)
  Version: v2.40.3-desktop.1
  Path:    C:\Program Files\Docker\cli-plugins\docker-compose.exe

Server:
 Containers: 5
  Running: 1
  Paused: 0
  Stopped: 4
 Images: 6
 Server Version: 29.1.3
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2

 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog

 CDI spec directories:
  /etc/cdi
  /var/run/cdi

PS C:\Users\pawan> docker history httpd
IMAGE          CREATED        CREATED BY                              SIZE    COMMENT
331548c5249b   7 days ago     CMD ["httpd-foreground"]                0B      buildkit.dockerfile.v0
<missing>      7 days ago     ENV HTTPD_PREFIX=/usr/local/apache2     0B      buildkit.dockerfile.v0
<missing>      8 days ago     # debian.sh --arch 'amd64' ...          87.4MB  debuerreotype 0.17

PS C:\Users\pawan> docker images
IMAGE                                   ID             DISK USAGE   CONTENT SIZE   EXTRA
httpd:latest                            331548c5249b   177MB        47.6MB         U
nginx:latest                            dec7a90bd097   240MB        65.8MB         U
nutrition-tracker:latest                219bd8f87ae9   296MB        71.6MB
pawansingh1618/nutrition-tracker:v1.1   e8276542def3   296MB        71.6MB
ubuntu:latest                           0d39fcc8335d   119MB        31.7MB         U
