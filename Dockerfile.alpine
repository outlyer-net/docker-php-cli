# Tiny PHP CLI (based on Alpine, only PHP installed).
# Note php's builtin server can be used as a quick and dirty web server.
#
# This Dockerfile creates a multi-arch image leveraging the
# newer builtin support in Docker (see https://github.com/docker/buildx,
# https://docs.docker.com/docker-for-mac/multi-arch/)
#
# <https://github.com/outlyer-net/docker-php-cli>

FROM alpine:latest
LABEL maintainer="Toni Corvera <outlyer@gmail.com>"

#RUN apk update \
#	&& apk add php-cli \
#	&& rm -rf /var/cache/apk/*
RUN apk --no-cache add php-cli

VOLUME [ "/www" ]

EXPOSE 80/tcp

# run!
ENTRYPOINT ["php","-S","0.0.0.0:80"]

HEALTHCHECK --interval=30s \
	--timeout=30s \
	--start-period=10s \
	--retries=3 \
	CMD [ "pidof", "php" ]
