FROM alpine:latest

LABEL \
    maintainer="Morgan Auchede <morgan.auchede@gmail.com>"

ENV \
    RESTIC_VERSION=latest

RUN set -eux \

    # Install packages

    && apk add --no-cache --no-progress --virtual BUILD_DEPS \
           build-base \
           bzip2 \
           curl \
           git \
           go \

    # Install dumb-entrypoint

    && curl --location "https://github.com/timonier/dumb-entrypoint/raw/master/bin/installer" | INSTALL_DIRECTORY="/usr/sbin" sh -s install \

    # Install hypnos

    && export $(curl "https://raw.githubusercontent.com/timonier/version-lister/release/generated/hypnos/latest" | xargs) \

    && curl --location "https://github.com/transitorykris/hypnos/releases/download/${HYPNOS_VERSION}/hypnos-linux-amd64.tar.gz" | tar --directory /usr/bin --extract --gzip \
    && chown root:root /usr/bin/hypnos \

    # Install rest-server

    && git clone "https://github.com/restic/rest-server" /tmp/rest-server \
    && sh -c "cd /tmp/rest-server && go run build.go" \
    && cp /tmp/rest-server/rest-server /usr/bin/rest-server \

    # Install restic

    && export $(curl "https://raw.githubusercontent.com/timonier/version-lister/release/generated/restic/${RESTIC_VERSION}" | xargs) \

    && curl --location --output /tmp/restic.bz2 "https://github.com/restic/restic/releases/download/v${RESTIC_VERSION}/restic_${RESTIC_VERSION}_linux_amd64.bz2" \
    && bunzip2 /tmp/restic.bz2 \
    && mv /tmp/restic /usr/bin/restic \
    && chmod +x /usr/bin/restic \

    # Clean

    && apk del --no-progress \
           BUILD_DEPS \

    && rm -f -r \
           /tmp/*

COPY ./rootfs /

ENTRYPOINT [ "dumb-init" , "--" , "dumb-entrypoint" ]
