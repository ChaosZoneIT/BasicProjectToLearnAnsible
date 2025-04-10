FROM vps-alma-9.5-min-image:latest

USER root
RUN microdnf update -y
RUN microdnf install -y python3

RUN adduser ansible
RUN usermod -aG wheel ansible
RUN usermod -aG root ansible
RUN echo 'ansible:ansible' | chpasswd
RUN echo 'ansible ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

USER ansible