# Linux Ansible Test Image

[![Build](https://github.com/lacrif/docker-linux-ansible/actions/workflows/build.yml/badge.svg)](https://github.com/lacrif/docker-linux-ansible/actions/workflows/build.yml) [![Docker pulls](https://img.shields.io/docker/pulls/lacrif/docker-linux-ansible)](https://hub.docker.com/r/lacrif/docker-linux-ansible/)

Linux Docker container for Ansible playbook and role testing.

## How to Use

  1. [Install Docker](https://docs.docker.com/engine/installation/).
  2. Pull this image from Docker Hub: `docker pull lacrif/docker-<os>-ansible:<version>`
  3. Run a container from the image: `docker run --detach --privileged --volume=/sys/fs/cgroup:/sys/fs/cgroup:rw --cgroupns=host lacrif/docker-<os>-ansible:<version>` (to test my Ansible roles, I add in a volume mounted from the current working directory with ``--volume=`pwd`:/etc/ansible/roles/role_under_test:ro``).
  4. Use Ansible inside the container:
    a. `docker exec --tty [container_id] env TERM=xterm ansible --version`
    b. `docker exec --tty [container_id] env TERM=xterm ansible-playbook /path/to/ansible/playbook.yml --syntax-check`

## Notes

I use Docker to test my Ansible roles and playbooks on multiple OSes using CI tools like Jenkins and Travis. This container allows me to test roles and playbooks using Ansible running locally inside the container.

> **Important Note**: I use this image for testing in an isolated environment—not for production—and the settings and configuration used may not be suitable for a secure and performant production environment. Use on production servers/in the wild at your own risk!

## Author Information

Lacrif
