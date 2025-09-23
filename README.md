# Linux Image Ansible

[![CI](https://github.com/lacrif/docker-ansible/actions/workflows/ci.yml/badge.svg)](https://github.com/lacrif/docker-ansible/actions/workflows/ci.yml) 
[![Docker pulls](https://img.shields.io/docker/pulls/lacrif/ansible)](https://hub.docker.com/r/lacrif/ansible/)

## Description

Ce repository fournit des conteneurs Docker basés sur Rocky Linux et Ubuntu spécialement conçus pour tester des playbooks et rôles Ansible dans un environnement isolé et contrôlé. Ces images sont particulièrement utiles pour l'intégration continue (CI) et les tests automatisés avec des outils comme Jenkins et Travis CI.

## Caractéristiques

🐧 Rocky Linux 9 comme système d'exploitation de base
🔧 Ansible pré-installé et configuré
🔒 Environnement privilégié pour les tests complets
📦 Prêt à l'emploi pour l'intégration continue
🚀 Optimisé pour les tests de rôles et playbooks

## Prérequis

Docker installé sur votre système
Connaissances de base d'Ansible et Docker

## Installation

### Depuis Docker Hub

``` bash
docker pull lacrif/ansible:<version>-<os>
```

Remplacez <version> par la version souhaitée : 11 ou 12
Remplacez <os> par le système souhaité : rockylinux9, ubuntu24.04

## Utilisation

### Démarrage du conteneur

```bash
docker run --detach --privileged \
  --volume=/sys/fs/cgroup:/sys/fs/cgroup:rw \
  --cgroupns=host \
  lacrif/ansible:<version>-<os>
```

### Test de rôles Ansible

Pour tester vos rôles Ansible, montez votre répertoire de travail actuel :

```bash
docker run --detach --privileged \
  --volume=/sys/fs/cgroup:/sys/fs/cgroup:rw \
  --cgroupns=host \
  --volume="$(pwd)":/etc/ansible/roles/role_under_test:ro \
  lacrif/ansible:<version>-<os>
```

### Exécution de commandes Ansible

Une fois le conteneur démarré, récupérez l'ID du conteneur et exécutez vos commandes Ansible :

```bash
# Vérifier la version d'Ansible
docker exec --tty [container_id] env TERM=xterm ansible --version

# Vérifier la syntaxe d'un playbook
docker exec --tty [container_id] env TERM=xterm ansible-playbook /path/to/ansible/playbook.yml --syntax-check

# Exécuter un playbook
docker exec --tty [container_id] env TERM=xterm ansible-playbook /path/to/ansible/playbook.yml
```

### Mode interactif
Pour une session interactive :

```bash
docker exec -it [container_id] /bin/bash
```

## Exemples d'utilisation

Test d'un rôle simple

```bash
# 1. Démarrer le conteneur avec votre rôle monté
docker run -d --privileged \
  --volume=/sys/fs/cgroup:/sys/fs/cgroup:rw \
  --cgroupns=host \
  --volume="$(pwd)/my-role":/etc/ansible/roles/my-role:ro \
  --name ansible-test \
  lacrif/ansible:latest

# 2. Créer un playbook de test simple
docker exec ansible-test bash -c 'cat > /tmp/test-playbook.yml << EOF
---
- hosts: localhost
  connection: local
  roles:
    - my-role
EOF'

# 3. Exécuter le test
docker exec ansible-test ansible-playbook /tmp/test-playbook.yml

# 4. Nettoyer
docker stop ansible-test && docker rm ansible-test
```

### Intégration CI/CD

Exemple pour GitHub Actions :

```yaml
name: Ansible Role Test
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      
      - name: Test with Docker
        run: |
          docker run --detach --privileged \
            --volume=/sys/fs/cgroup:/sys/fs/cgroup:rw \
            --cgroupns=host \
            --volume="${PWD}":/etc/ansible/roles/role_under_test:ro \
            lacrif/docker-linux-ansible:latest
```

## Versions supportées

OS :
- Rocky Linux 9
- Ubuntu Noble
- Debian Bookworm
- Alpine 3.22

Ansible :
- 11  (dernière version stable)

## Notes

J'utilise Docker pour tester mes rôles et playbooks Ansible sur différents systèmes d'exploitation, grâce à des outils de CI comme Jenkins et Travis. Ce conteneur me permet de tester les rôles et les playbooks en exécutant Ansible localement à l'intérieur du conteneur.

> **Remarque importante :** Cette image est destinée uniquement aux tests en environnement isolé et non à la production. Les paramètres et la configuration utilisés peuvent ne pas être adaptés à un environnement de production sécurisé et performant. Son utilisation en production est donc à vos propres risques.

## Auteur

Lacrif
