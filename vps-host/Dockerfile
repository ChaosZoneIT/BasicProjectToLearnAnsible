FROM vps-ubuntu-22-image:latest

USER root

#  Install ansible
RUN apt install -y software-properties-common
RUN add-apt-repository --yes --update ppa:ansible/ansible
RUN apt install -y ansible
RUN apt install -y python3

RUN useradd -rm -d /home/ansible -s /bin/bash -g root -G sudo ansible
RUN echo 'ansible:ansible' | chpasswd
USER ansible
# to sprawdzić czy trzeba generować na dockerze ssh czy już tylko ssh-copy-id
RUN ssh-keygen -A -v