# Official and semi-official architectures: https://github.com/docker-library/official-images#architectures-other-than-amd64
ARCHITECTURES=linux/amd64,linux/arm64,linux/386,linux/arm/v7,linux/arm/v6
IMAGE_NAME=outlyernet/php-cli

DOCKER_BUILDX=env DOCKER_CLI_EXPERIMENTAL=enabled docker buildx
# This makefile uses the builder only temporarily and destroys it when done
MULTIARCH_BUILDER_NAME=$(subst /,_,$(IMAGE_NAME))-multiarch-builder

# Build locally
build:
	docker build \
		--tag $(IMAGE_NAME) \
		--tag $(IMAGE_NAME):$(shell date +%Y%m%d_%H%M%S) \
		.


# XXX: As of this writing (2020-03) running this with --load instead of --push (i.e. locally),
#      fails. Support for multi-arch in the daemon is pending, see https://github.com/docker/buildx/issues/59
push:
	-$(MAKE) multiarch-bootstrap
	$(DOCKER_BUILDX) build --platform $(ARCHITECTURES) \
		--tag $(IMAGE_NAME) \
		--push \
		.
	-$(MAKE) multiarch-unbootstrap

test: build
	docker run --rm -it -p 8000:80 $(IMAGE_NAME)

inspect:
	$(DOCKER_BUILDX) imagetools inspect $(IMAGE_NAME)


# Enable the new enhanced multi-arch support (requires Docker 19.03
# and experimental mode)
# https://docs.docker.com/docker-for-mac/multi-arch/
multiarch-bootstrap:
	$(DOCKER_BUILDX) create --use --name $(MULTIARCH_BUILDER_NAME)

multiarch-unbootstrap:
	$(DOCKER_BUILDX) rm $(MULTIARCH_BUILDER_NAME)
