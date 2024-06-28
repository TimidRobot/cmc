# https://docs.docker.com/engine/reference/builder/

# https://registry.hub.docker.com/_/bash
ARG BASH_VERSION
FROM bash:${BASH_VERSION}

RUN apk add --update-cache \
        coreutils \
        mawk \
        openssh-client-default \
        procps-ng \
        shellcheck \
    && rm -rf /var/cache/apk/*

# user
RUN adduser --disabled-password user \
    && mkdir -m 0700 /home/user/.ssh \
    && chown user:user /home/user/.ssh \
    && mkdir -m 0755 /home/user/bin \
    && chown user:user /home/user/bin

WORKDIR /home/user
USER user:user
ENV PATH=/home/user/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
CMD /bin/sh -c "trap 'exit' TERM; while true; do sleep 0.1; done"
