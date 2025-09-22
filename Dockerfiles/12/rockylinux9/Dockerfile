# Utilisation d'une image de base plus légère
FROM rockylinux:9

# Metadata
LABEL org.opencontainers.image.vendor="Lacrif" \
    org.opencontainers.image.url="https://docs.ansible.com/" \
    org.opencontainers.image.source="https://github.com/lacrif/docker-linux-ansible" \
    org.opencontainers.image.title="Ansible" \
    org.opencontainers.image.description="Ansible is a suite of software tools that enables infrastructure as code." \
    org.opencontainers.image.version="11" \
    org.opencontainers.image.documentation="https://docs.ansible.com/"

# Installation optimisée en une seule couche RUN
RUN set -eux; \
    # Configuration systemd
    rm -f /lib/systemd/system/multi-user.target.wants/* \
          /etc/systemd/system/*.wants/* \
          /lib/systemd/system/local-fs.target.wants/* \
          /lib/systemd/system/sockets.target.wants/*udev* \
          /lib/systemd/system/sockets.target.wants/*initctl* \
          /lib/systemd/system/basic.target.wants/* \
          /lib/systemd/system/anaconda.target.wants/*; \
    \
    # Installation des paquets essentiels uniquement
    dnf install -y \
        epel-release \
        initscripts \
        sudo \
        python3.12 \
        python3.12-pip && \
    \
    # Configuration Python
    ln -sf /usr/bin/python3.12 /usr/bin/python3 && \
    ln -sf /usr/bin/pip3.12 /usr/bin/pip3 && \
    \
    # Installation d'Ansible avec cache pip optimisé
    pip3 install --no-cache-dir --upgrade pip && \
    pip3 install --no-cache-dir "ansible~=12.0" && \
    \
    # Configuration Ansible
    mkdir -p /etc/ansible && \
    echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts && \
    \
    # Configuration sudo
    sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/' /etc/sudoers && \
    \
    # Nettoyage complet
    dnf clean all && \
    rm -rf /var/cache/dnf/* \
           /tmp/* \
           /var/tmp/* \
           /root/.cache \
           /root/.local

# CMD
CMD ["/usr/lib/systemd/systemd"]
