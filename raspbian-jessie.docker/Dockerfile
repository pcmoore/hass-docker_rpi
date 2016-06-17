FROM raspbian-jessie:latest
MAINTAINER Paul Moore <paul@paul-moore.com>

# NOTE: this is a container designed to update the base system image without
#       having to regenerate it using debootstrap

# update the base system
CMD apt-get update && apt-get upgrade -y && \
	echo ">>> CONTAINER ID: $HOSTNAME"; \
	echo ">>> commit: docker commit $HOSTNAME raspbian-jessie:latest"
