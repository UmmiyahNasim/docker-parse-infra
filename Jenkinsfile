Parse Server deployment on Docker Swarm cluster with  Jenkins + Promethous+Grafan for CI/CD and  monitoring 

1. Install Docker
```
curl -sSL https://get.docker.com/ | sh

```

2. Add `ubuntu` user to `docker` group
```
sudo usermod -aG docker ubuntu
newgrp docker
```

3. install Docker machine 
```
base=https://github.com/docker/machine/releases/download/v0.14.0 &&
  curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
  sudo install /tmp/docker-machine /usr/local/bin/docker-machine

```
4. Install Docker compose -

```
curl -L https://github.com/docker/compose/releases/download/1.23.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```


5. Run Jenkins container and tail logs
```
mkdir ~/jenkins
cd ~/jenkins
docker pull jenkins/jenkins:lts
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v $(pwd):/var/jenkins_home --restart always jenkins/jenkins:lts
docker logs -f jenkins
```

6. Open web UI to jenkins and install following plugins:
- AnsiColor
- Blue Ocean
- Self-Organizing Swarm Plug-in Modules
- Throttle Concurrent Builds Plug-in

7. Initialize Docker Swarm.  Make note of the `docker swarm join` instructions that are listed (this can be accessed at any time by running `docker swarm join-token worker` on a swarm manager.
```
docker swarm init
```

8. Expose Docker TCP Socket.  Run `sudo systemctl edit docker` and paste the following: (replace `0.0.0.0` with internal IP address if this node has a public IP address)
```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
```

9. Restart Docker service and check that it is running
```
sudo systemctl restart docker
sudo systemctl status docker
```
