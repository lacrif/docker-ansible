ARG ANSIBLE_VERSION=">11, <12"

FROM rockylinux:9

# Metadata
LABEL org.opencontainers.image.vendor="Lacrif" \
    org.opencontainers.image.url="https://docs.ansible.com/" \
    org.opencontainers.image.source="https://github.com/lacrif/docker-linux-ansible" \
    org.opencontainers.image.title="Ansible" \
    org.opencontainers.image.description="Ansible is a suite of software tools that enables infrastructure as code." \
    org.opencontainers.image.version="11" \
    org.opencontainers.image.documentation="https://docs.ansible.com/"

ENV container=docker

ARG ANSIBLE_VERSION

# Install systemd -- See https://hub.docker.com/_/centos/
RUN rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Install requirements.
RUN yum -y install rpm dnf-plugins-core \
 && yum -y update \
 && yum -y install \
      epel-release \
      initscripts \
      sudo \
      which \
      hostname \
      libyaml \
      python3 \
      python3-pip \
      python3-pyyaml \
      iproute \
 && yum clean all

# Upgrade pip to latest version.
RUN pip3 install --upgrade pip

# Install Ansible via Pip.
RUN pip3 install "ansible~=11.0"

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/' /etc/sudoers

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts

VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/lib/systemd/systemd"]