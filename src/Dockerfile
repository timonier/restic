FROM alpine:latest

LABEL \
    maintainer="Morgan AUCHEDE <morgan.auchede@gmail.com>"

RUN set -e -u -x \
\
    # Install packages
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS bzip2 curl \
    && apk add --no-cache --no-progress apache2-utils bash ca-certificates tzdata \
\
    # Install rclone
\
    && curl --location --output /tmp/rclone.zip "https://github.com/ncw/rclone/releases/download/v1.53.1/rclone-v1.53.1-linux-amd64.zip" \
    && ( cd /tmp && unzip /tmp/rclone.zip ) \
    && mv /tmp/rclone-v1.53.1-linux-amd64/rclone /usr/local/bin/rclone \
    && chmod +x /usr/local/bin/rclone \
\
    # Install rest-server
\
    && curl --location --output /tmp/rest-server.gz "https://github.com/restic/rest-server/releases/download/v0.9.7/rest-server-0.9.7-linux-amd64.gz" \
    && gunzip /tmp/rest-server.gz \
    && mv /tmp/rest-server /usr/local/bin/rest-server \
    && chmod +x /usr/local/bin/rest-server \
\
    # Install restic
\
    && curl --location --output /tmp/restic.bz2 "https://github.com/restic/restic/releases/download/v0.10.0/restic_0.10.0_linux_amd64.bz2" \
    && bunzip2 /tmp/restic.bz2 \
    && mv /tmp/restic /usr/local/bin/restic \
    && chmod +x /usr/local/bin/restic \
\
    # Clean
\
    && apk del --no-progress BUILD_DEPS \
    && rm -f -r /tmp/*
