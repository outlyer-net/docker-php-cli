# Official and semi-official architectures: https://github.com/docker-library/official-images#architectures-other-than-amd64
ARCHITECTURES=linux/amd64,linux/arm64,linux/386,linux/arm/v7,linux/arm/v6
IMAGE_NAME=outlyernet/php-cli

DOCKER_BUILDX=env DOCKER_CLI_EXPERIMENTAL=enabled docker buildx
# This makefile uses the builder only temporarily and destroys it when done
MULTIARCH_BUILDER_NAME=$(subst /,_,$(IMAGE_NAME))-multiarch-builder

# Build locally
build: build-alpine build-debian build-ubuntu
build-%:
	docker build \
		--tag $(IMAGE_NAME) \
		--tag $(IMAGE_NAME):$(shell date +%Y%m%d_%H%M%S) \
		-f Dockerfile.$* \
		.

test: test-alpine
test-%: build-%
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

### Rules below this point are used for pushing to Docker Hub

push: push-alpine push-debian push-ubuntu push-latest

# XXX: As of this writing (2020-03) running this with --load instead of --push (i.e. locally),
#      fails. Support for multi-arch in the daemon is pending, see https://github.com/docker/buildx/issues/59
push-%:
	-$(MAKE) multiarch-bootstrap
	$(DOCKER_BUILDX) build --platform $(ARCHITECTURES) \
		--tag $(IMAGE_NAME):$* \
		--push \
		-f Dockerfile.$* \
		.
	-$(MAKE) multiarch-unbootstrap

push-latest: push-readme
	-$(MAKE) multiarch-bootstrap
	$(DOCKER_BUILDX) build --platform $(ARCHITECTURES) \
		--tag $(IMAGE_NAME):latest \
		--push \
		-f Dockerfile.alpine \
		.
	-$(MAKE) multiarch-unbootstrap

# Push updated readme to Docker Hub
push-readme:
	docker run --rm \
		-v $(PWD)/README.md:/data/README.md \
		-e DOCKERHUB_USERNAME=$(shell echo $(IMAGE_NAME) | cut -d/ -f1) \
		-e DOCKERHUB_REPO_PREFIX=$(shell echo $(IMAGE_NAME) | cut -d/ -f1) \
		-e DOCKERHUB_REPO_NAME=$(shell echo $(IMAGE_NAME) | cut -d/ -f2) \
		-e DOCKERHUB_PASSWORD=$(shell relevation hub.docker.com 2>/dev/null | sed -E -e '/^Password/!d' -e 's/^(\w|:)*\s//') \
		sheogorath/readme-to-dockerhub

