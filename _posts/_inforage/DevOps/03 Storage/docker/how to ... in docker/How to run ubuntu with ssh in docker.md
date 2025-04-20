# Dockerfile

```bash
# Use the official image as a parent image
FROM ubuntu

# Update the system, install OpenSSH Server, and set up users
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y openssh-server

# Create user and set password for user and root user
RUN if id -u ubuntu 2>/dev/null; then \
        echo "ubuntu:beast_" | chpasswd; \
    else \
        useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 ubuntu && \
        echo "ubuntu:beast_" | chpasswd; \
    fi && \
    echo "root:beast_" | chpasswd

# Set up configuration for SSH
RUN mkdir /var/run/sshd && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd && \
    echo "export VISIBLE=now" >> /etc/profile

# Expose the SSH port
EXPOSE 22

# Run SSH
CMD ["/usr/sbin/sshd", "-D"]
```

# Docker command

```bash
docker run -d \
    --name ubuntu_ssh_beast \
    --cpus="2" \
    --memory="4g" \
    -p 2222:22 -p 55000:5000 -p 53000:3000 -p 55173:5173 \
    ubuntu_ssh_beast
```

# Linux command

```bash
ssh -p 2222 ubuntu@194.226.49.251
```
# Resources

- [Setting Up SSH Access in an Ubuntu Docker Container](https://tecadmin.net/setting-up-ubuntu-docker-container-with-ssh-access/)
- 