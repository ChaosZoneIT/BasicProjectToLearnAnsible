FROM debian:bookworm-slim AS debian-base

# Setup the default user.
RUN useradd -rm -d /home/admin -s /bin/bash -g root -G sudo admin
RUN echo 'admin:admin' | chpasswd
USER admin
WORKDIR /home/admin

# Build image with SSHD.
FROM debian-base AS debian-with-sshd
USER root

# Install required tools.
RUN apt-get -qq update \
    && apt-get -qq --no-install-recommends install vim-tiny \
    && apt-get -qq --no-install-recommends install sudo \
    && apt-get -qq --no-install-recommends install openssh-server \
    && apt-get install -y iputils-ping \
    && apt-get install -y openssh-client \
    && apt install net-tools \
    && apt install -y nano \
    && apt-get -qq clean \
    && rm -rf /var/lib/apt/lists/*

# Configure SSHD.
# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN mkdir /var/run/sshd
RUN bash -c 'install -m755 <(printf "#!/bin/sh\nexit 0") /usr/sbin/policy-rc.d'
RUN ex +'%s/^#\zeListenAddress/\1/g' -scwq /etc/ssh/sshd_config
RUN ex +'%s/^#\zeHostKey .*ssh_host_.*_key/\1/g' -scwq /etc/ssh/sshd_config
RUN RUNLEVEL=1 dpkg-reconfigure openssh-server
RUN ssh-keygen -A -v
RUN update-rc.d ssh defaults

# Configure sudo.
RUN ex +"%s/^%sudo.*$/%sudo ALL=(ALL:ALL) NOPASSWD:ALL/g" -scwq! /etc/sudoers

USER admin

EXPOSE 22
CMD ["/usr/bin/sudo", "/usr/sbin/sshd", "-D", "-o", "ListenAddress=0.0.0.0"]