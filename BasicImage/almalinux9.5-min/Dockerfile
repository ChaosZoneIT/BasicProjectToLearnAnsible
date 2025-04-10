# Build image with SSHD.
FROM almalinux:9.5-minimal AS alma-with-sshd
RUN echo 'root:secret' | chpasswd
USER root

# Install required tools.
RUN microdnf clean all -y
RUN microdnf update -y
RUN microdnf install -y vim
RUN microdnf install -y openssh-server
RUN microdnf install -y sudo.x86_64

# netstat
RUN microdnf install -y net-tools

# editor nano
RUN microdnf install -y nano


# Configure SSHD.
# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN bash -c 'install -m755 <(printf "#!/bin/sh\nexit 0") /usr/sbin/policy-rc.d'
RUN sed 's@#ListenAddress\s*0.*@ListenAddress 0.0.0.0@g' -i /etc/ssh/sshd_config
RUN sed 's@#ListenAddress.*@ListenAddress ::@g' -i /etc/ssh/sshd_config
RUN sed 's@#PermitRootLogin.*@PermitRootLogin Yes@g' -i /etc/ssh/sshd_config

RUN ssh-keygen -A -v

EXPOSE 22
CMD [ "/usr/bin/sudo", "/usr/sbin/sshd", "-D"]