# Build image with SSHD.
FROM fedora:41 AS fedora-with-sshd
RUN echo 'root:secret' | chpasswd
USER root

# Install required tools.
RUN dnf -y update 
RUN dnf -y install vim
RUN dnf -y install openssh-server

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
CMD [ "/usr/bin/sudo", "/usr/sbin/sshd", "-D"]