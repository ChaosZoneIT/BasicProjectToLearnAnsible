FROM vps-debian-bookworm-image:latest

USER root

RUN useradd -rm -d /home/ansible -s /bin/bash -g root -G sudo ansible
RUN echo 'ansible:ansible' | chpasswd
USER ansible