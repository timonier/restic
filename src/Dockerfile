FROM alpine:latest

LABEL \
    maintainer="Morgan Auchede <morgan.auchede@gmail.com>"

RUN set -e -u -x \
\
    # Install packages
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS bzip2 curl \
    && apk add --no-cache --no-progress apache2-utils ca-certificates \
\
    # Install dumb-entrypoint
\
    && curl --location "https://github.com/timonier/dumb-entrypoint/raw/master/src/dumb-entrypoint/installer" | sh -s -- install \
    && curl --location "https://github.com/timonier/dumb-entrypoint/raw/master/src/gosu/installer" | sh -s -- install \
\
    # Install rclone
\
    && export $(curl --location "https://github.com/timonier/version-lister/raw/generated/ncw/rclone/latest" | xargs) \
    && curl --location --output /tmp/rclone.zip "${RCLONE_RELEASE}" \
    && sh -c "cd /tmp && unzip /tmp/rclone.zip" \
    && mv /tmp/rclone-v"${RCLONE_VERSION}"-linux-amd64/rclone /usr/local/bin/rclone \
    && chmod +x /usr/local/bin/rclone \
\
    # Install rest-server
\
    && export $(curl --location "https://github.com/timonier/version-lister/raw/generated/restic/rest-server/latest" | xargs) \
    && curl --location --output /tmp/rest-server.gz "${REST_SERVER_RELEASE}" \
    && gunzip /tmp/rest-server.gz \
    && mv /tmp/rest-server /usr/local/bin/rest-server \
    && chmod +x /usr/local/bin/rest-server \
\
    # Install restic
\
    && export $(curl --location "https://github.com/timonier/version-lister/raw/generated/restic/restic/latest" | xargs) \
    && curl --location --output /tmp/restic.bz2 "${RESTIC_RELEASE}" \
    && bunzip2 /tmp/restic.bz2 \
    && mv /tmp/restic /usr/local/bin/restic \
    && chmod +x /usr/local/bin/restic \
\
    # Clean
\
    && apk del --no-progress BUILD_DEPS \
    && rm -f -r /tmp/*

ENTRYPOINT [ "dumb-entrypoint" ]
