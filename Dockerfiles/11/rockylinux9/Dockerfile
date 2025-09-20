FROM rockylinux:9

# Metadata
LABEL org.opencontainers.image.vendor="Lacrif" \
    org.opencontainers.image.url="https://docs.ansible.com/" \
    org.opencontainers.image.source="https://github.com/lacrif/docker-linux-ansible" \
    org.opencontainers.image.title="Ansible" \
    org.opencontainers.image.description="Ansible is a suite of software tools that enables infrastructure as code." \
    org.opencontainers.image.version="11" \
    org.opencontainers.image.documentation="https://docs.ansible.com/"

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
      python3.12 \
      python3.12-pip \
      python3.12-pyyaml \
      iproute \
 && yum clean all

# Change Python version
RUN rm /usr/bin/python3 \
 && ln -s /usr/bin/python3.12 /usr/bin/python3 \
 && ln -s /usr/bin/pip3.12 /usr/bin/pip3

# Install Ansible via Pip.
RUN pip3 install "ansible~=11.0"

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/' /etc/sudoers

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts

VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/lib/systemd/systemd"]