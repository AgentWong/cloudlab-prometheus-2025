Rocky(Default):
`DOCKER_IMAGE=geerlingguy/docker-rockylinux8-ansible DOCKER_TAG=latest molecule converge -s prometheus`

Debian:
`DOCKER_IMAGE=geerlingguy/docker-debian11-ansible DOCKER_TAG=latest molecule converge -s prometheus`

Ubuntu:
`DOCKER_IMAGE=geerlingguy/docker-ubuntu2204-ansible DOCKER_TAG=latest molecule converge -s prometheus`