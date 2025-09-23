FROM rockylinux:9

# Metadata
LABEL maintainer="Lacrif"

ENV container=docker

# Installation et nettoyage en une seule couche pour minimiser la taille
RUN dnf install -y epel-release  \
    && dnf update -y \
    # Installation des dépendances minimales
    && dnf install -y \
        sudo wget \
        python3.12 python3.12-pip python3.12-setuptools \
        ansible

# Nettoyage complet
RUN dnf clean all && \
    rm -rf /var/cache/dnf/* \
           /tmp/* \
           /var/tmp/* \
           /root/.cache \
           /root/.local \
           /usr/share/doc \
           /usr/share/man

# Création d'un utilisateur ansible
RUN useradd -m -s /bin/bash ansible && \
    mkdir -p /home/ansible/.ssh && \
    echo -e "Host *\n    StrictHostKeyChecking no\n    UserKnownHostsFile=/dev/null\n    LogLevel ERROR" > /home/ansible/.ssh/config && \
    chmod 700 /home/ansible/.ssh && \
    chmod 600 /home/ansible/.ssh/config && \
    chown -R ansible:ansible /home/ansible

# Répertoire de travail
WORKDIR /home/ansible

# Utilisateur par défaut
USER ansible

# Point d'entrée
ENTRYPOINT []
CMD ["ansible", "--version"]
