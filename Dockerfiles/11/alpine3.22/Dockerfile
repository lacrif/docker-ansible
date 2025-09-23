FROM alpine:3.22

# Metadata (inchangé)
LABEL org.opencontainers.image.vendor="Lacrif" \
    org.opencontainers.image.url="https://docs.ansible.com/" \
    org.opencontainers.image.source="https://github.com/lacrif/docker-ansible" \
    org.opencontainers.image.title="Ansible" \
    org.opencontainers.image.description="Ansible 11.10 sur Alpine Linux 3.22" \ 
    org.opencontainers.image.version="11" \
    org.opencontainers.image.documentation="https://docs.ansible.com/"

# Variables d'environnement
ENV PYTHONUNBUFFERED=1 \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    PATH="/home/ansible/.local/bin:$PATH" \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1

# Installation
RUN apk add --update --no-cache \
        ansible~=11 \
        bash \
        openssh \ 
        sshpass \
        rsync \
        py3-passlib

# # Création de l'utilisateur ansible
RUN addgroup -g 1000 ansible && \
    adduser -D -u 1000 -G ansible ansible && \
    # Configuration SSH
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