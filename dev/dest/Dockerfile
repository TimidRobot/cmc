# https://docs.docker.com/engine/reference/builder/

# https://registry.hub.docker.com/_/debian
FROM debian:bookworm-slim

# Configure apt not to prompt during docker build
ARG DEBIAN_FRONTEND=noninteractive

# Configure apt to avoid installing recommended and suggested packages
RUN apt-config dump \
    | grep -E '^APT::Install-(Recommends|Suggests)' \
    | sed -e's/1/0/' \
    | tee /etc/apt/apt.conf.d/99no-recommends-no-suggests

# Resynchronize the package index and install packages
# https://docs.docker.com/build/building/best-practices/#apt-get
RUN apt-get update && apt-get install -y \
        ssh \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir /run/sshd

# userone
RUN useradd --create-home userone \
    && mkdir -m 0700 /home/userone/.ssh \
    && chown userone:userone /home/userone/.ssh

# usertwo
RUN useradd --create-home usertwo \
    && mkdir -m 0700 /home/usertwo/.ssh \
    && chown usertwo:usertwo /home/usertwo/.ssh

# Run SSH server in the foreground
CMD /usr/sbin/sshd -4 -D -e -p ${SSH_PORT}
