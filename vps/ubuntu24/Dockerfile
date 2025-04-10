FROM vps-ubuntu-24-image:latest

USER root
RUN apt update -y
RUN apt install -y python3

RUN useradd -rm -d /home/ansible -s /bin/bash -g root -G sudo ansible
RUN echo 'ansible:ansible' | chpasswd
USER ansible
RUN ssh-keygen -A -v