FROM ubuntu:noble

# Metadata
LABEL org.opencontainers.image.vendor="Lacrif" \
    org.opencontainers.image.url="https://docs.ansible.com/" \
    org.opencontainers.image.source="https://github.com/lacrif/docker-linux-ansible" \
    org.opencontainers.image.title="Ansible" \
    org.opencontainers.image.description="Ansible is a suite of software tools that enables infrastructure as code." \ 
    org.opencontainers.image.version="11" \
    org.opencontainers.image.documentation="https://docs.ansible.com/"

# Variables d'environnement
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONUNBUFFERED=1

# Installation et nettoyage en une seule couche pour minimiser la taille
RUN apt-get update \
    # Installation des dépendances minimales
    && apt-get install -y --no-install-recommends \
       sudo build-essential wget libffi-dev libssl-dev procps \
       python3-pip python3-dev python3-setuptools python3-wheel python3-apt \
       ansible \
    # Nettoyage complet des caches et fichiers temporaires
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/share/doc \
    && rm -rf /usr/share/man \
    && rm -rf /usr/share/info \
    && rm -rf /usr/share/lintian \
    && rm -rf /var/cache/debconf \
    && rm -rf /tmp/* \
    && rm -rf /var/tmp/*

# Création d'un utilisateur non-root pour Ansible
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
