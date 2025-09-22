# Utiliser l'image slim pour réduire la taille de base (~200MB -> ~80MB)
FROM ubuntu:24.04-slim

# Metadata (inchangé)
LABEL org.opencontainers.image.vendor="Lacrif" \
    org.opencontainers.image.url="https://docs.ansible.com/" \
    org.opencontainers.image.source="https://github.com/lacrif/docker-linux-ansible" \
    org.opencontainers.image.title="Ansible" \
    org.opencontainers.image.description="Ansible is a suite of software tools that enables infrastructure as code." \ 
    org.opencontainers.image.version="12" \
    org.opencontainers.image.documentation="https://docs.ansible.com/"

# Variables d'environnement
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONUNBUFFERED=1

# Installation et nettoyage en une seule couche pour minimiser la taille
RUN apt-get update \
    # Installation des dépendances minimales (suppression de build-essential après utilisation)
    && apt-get install -y --no-install-recommends \
       locales \
       python3 \
       python3-pip \
       python3-venv \
       python3-yaml \
       rsyslog \
       systemd \
       systemd-cron \
       sudo \
       iproute2 \
       ca-certificates \
       # Dépendances temporaires pour la compilation
       build-essential \
       libffi-dev \
       libssl-dev \
       libyaml-dev \
       python3-dev \
    # Configuration des locales
    && locale-gen en_US.UTF-8 \
    # Configuration rsyslog
    && sed -i 's/^\($ModLoad imklog\)/#\1/' /etc/rsyslog.conf \
    # Suppression du flag Python EXTERNALLY-MANAGED
    && rm -rf /usr/lib/python3.12/EXTERNALLY-MANAGED \
    # Création d'un environnement virtuel Python pour Ansible
    && python3 -m venv /opt/ansible \
    # Installation d'Ansible dans l'environnement virtuel
    && /opt/ansible/bin/pip install --no-cache-dir --upgrade pip \
    && /opt/ansible/bin/pip install --no-cache-dir "ansible~=12.0" \
    # Nettoyage des dépendances de build devenues inutiles
    && apt-get remove -y \
       build-essential \
       libffi-dev \
       libssl-dev \
       libyaml-dev \
       python3-dev \
    && apt-get autoremove -y \
    # Nettoyage complet des caches et fichiers temporaires
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/share/doc \
    && rm -rf /usr/share/man \
    && rm -rf /usr/share/info \
    && rm -rf /usr/share/lintian \
    && rm -rf /var/cache/debconf \
    && rm -rf /tmp/* \
    && rm -rf /var/tmp/* \
    # Nettoyage des caches Python
    && /opt/ansible/bin/pip cache purge \
    # Suppression des fichiers systemd inutiles
    && rm -f /lib/systemd/system/systemd*udev* \
    && rm -f /lib/systemd/system/getty.target

# Ajouter l'environnement virtuel Ansible au PATH
ENV PATH="/opt/ansible/bin:$PATH"

# Configuration d'Ansible (en une seule couche)
RUN mkdir -p /etc/ansible \
    && echo "[local]\nlocalhost ansible_connection=local" > /etc/ansible/hosts

# Copie et configuration d'initctl_faker
COPY initctl_faker /initctl_faker
RUN chmod +x /initctl_faker \
    && rm -f /sbin/initctl \
    && ln -s /initctl_faker /sbin/initctl

# Configuration des volumes et CMD (inchangé)
VOLUME ["/sys/fs/cgroup", "/tmp", "/run"]
CMD ["/lib/systemd/systemd"]
