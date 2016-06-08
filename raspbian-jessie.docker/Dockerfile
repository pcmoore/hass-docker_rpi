FROM raspbian-jessie:latest
MAINTAINER Paul Moore <paul@paul-moore.com>

# update the base system
CMD apt-get update && apt-get upgrade -y && \
	echo ">>> CONTAINER ID: $HOSTNAME"; \
	echo ">>> commit: docker commit $HOSTNAME raspbian-jessie:latest"
