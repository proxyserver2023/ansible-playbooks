FROM ubuntu:24.04

# Prevent interactive dialogue during build
ENV DEBIAN_FRONTEND=noninteractive

# Install OpenSSH server and sudo
RUN apt-get update && apt-get install -y openssh-server sudo && \
  mkdir /var/run/sshd

# Create a user "ansible" with password "ansible" and grant sudo privileges
RUN useradd -m -s /bin/bash ansible && \
  echo "ansible:ansible" | chpasswd && \
  adduser ansible sudo

# Enable password authentication (if disabled by default)
RUN sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config && \
  sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config

# (Optional) If you want to set up SSH keys, you could copy your public key:
# COPY authorized_keys /home/ansible/.ssh/authorized_keys
# RUN chown ansible:ansible /home/ansible/.ssh/authorized_keys && chmod 600 /home/ansible/.ssh/authorized_keys

# Expose SSH port
EXPOSE 22

# Run the SSH daemon in the foreground
CMD ["/usr/sbin/sshd", "-D"]
