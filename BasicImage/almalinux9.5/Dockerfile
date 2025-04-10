# Build image with SSHD.
FROM almalinux:9.5 AS alma-with-sshd
RUN echo 'root:secret' | chpasswd
USER root

# Install required tools.
RUN dnf clean all -y
RUN dnf update -y
RUN dnf install -y vim
RUN dnf install -y openssh-server
RUN dnf install -y sudo.x86_64
RUN dnf install -y openssh-clients

# netstat
RUN dnf install -y net-tools

# editor nano
RUN dnf install -y nano


# Configure SSHD.
# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN bash -c 'install -m755 <(printf "#!/bin/sh\nexit 0") /usr/sbin/policy-rc.d'
RUN ex +'%s/^#\zeListenAddress/\1/g' -scwq /etc/ssh/sshd_config
RUN ex +'%s/^#\zeHostKey .*ssh_host_.*_key/\1/g' -scwq /etc/ssh/sshd_config

RUN sed 's@#PermitRootLogin.*@PermitRootLogin Yes@g' -i /etc/ssh/sshd_config

RUN ssh-keygen -A -v

EXPOSE 22
CMD ["/usr/bin/sudo", "/usr/sbin/sshd", "-D"]