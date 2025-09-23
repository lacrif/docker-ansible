FROM alpine:3.22

# Metadata
LABEL maintainer="Lacrif"
LABEL description="Ansible is a suite of software tools that enables infrastructure as code."
LABEL version="11"

# Installation
RUN apk add --update --no-cache \
        python3 \
        git

# Création d'un utilisateur ansible
RUN adduser -D -s /bin/bash ansible && \
    mkdir -p /home/ansible/.ssh && \
    echo -e "Host *\n    StrictHostKeyChecking no\n    UserKnownHostsFile=/dev/null\n    LogLevel ERROR" > /home/ansible/.ssh/config && \
    chmod 700 /home/ansible/.ssh && \
    chmod 600 /home/ansible/.ssh/config && \
    chown -R ansible:ansible /home/ansible

# Change vers l'utilisateur ansible
USER ansible
WORKDIR /home/ansible

# Crée l'environnement virtuel Python
RUN python3.12 -m venv /home/ansible/venv

# Active l'environnement virtuel et installe Ansible
RUN /home/ansible/venv/bin/pip install --upgrade pip --no-cache-dir && \
    /home/ansible/venv/bin/pip install ansible~=11.0 --no-cache-dir

# Ajoute le venv au PATH pour faciliter l'utilisation
ENV PATH="/home/ansible/venv/bin:$PATH"

# Point d'entrée
CMD ["ansible", "--version"]