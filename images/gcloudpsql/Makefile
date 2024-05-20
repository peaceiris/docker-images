DOCKER_CLI_EXPERIMENTAL := enabled
DOCKER_BUILDKIT := 1

GCLOUD_SDK_VERSION := 470.0.0
POSTGRESQL_VERSION := 16

DOCKER_USERNAME ?= peaceiris
DOCKER_IMAGE_NAME := gcloudpsql
DOCKER_HUB_BASE_NAME := ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}
DOCKER_BASE_NAME := ghcr.io/${DOCKER_HUB_BASE_NAME}
DOCKER_VERSION = 0.$(POSTGRESQL_VERSION).$(shell echo $(GCLOUD_SDK_VERSION) | cut -d. -f1)
DOCKER_TAG := v${DOCKER_VERSION}
GITHUB_REF_NAME ?= local
DOCKER_SCOPE := gcloudpsql-${GITHUB_REF_NAME}
DOCKER_OUTPUT_TYPE ?= docker
ifdef DOCKER_MULTI_PLATFORM
	DOCKER_PLATFORM := --platform linux/amd64
	DOCKER_OUTPUT_TYPE := registry
endif
PKG_NAME := ${DOCKER_BASE_NAME}:${DOCKER_TAG}
HUB_NAME := ${DOCKER_HUB_BASE_NAME}:${DOCKER_TAG}

.PHONY: login-dockerhub
login-dockerhub:
	echo "${DOCKER_HUB_TOKEN}" | docker login -u "${DOCKER_USERNAME}" --password-stdin

.PHONY: login-ghcr
login-ghcr:
	echo "${GITHUB_TOKEN}" | docker login ghcr.io -u "${DOCKER_USERNAME}" --password-stdin

.PHONY: login
login: login-dockerhub login-ghcr

.PHONY: setup-buildx
setup-buildx:
	docker buildx create --use --driver docker-container

.PHONY: build
build:
ifeq (${CI}, true)
	$(MAKE) setup-buildx
	docker version
	docker buildx inspect --bootstrap
endif
	docker buildx build . \
		--tag "${PKG_NAME}" \
		--tag "${HUB_NAME}" \
		${DOCKER_PLATFORM} \
		--label "org.opencontainers.image.version=${DOCKER_VERSION}" \
		--output "type=${DOCKER_OUTPUT_TYPE}" \
		--cache-from "type=gha,scope=${DOCKER_SCOPE}" \
		--cache-to "type=gha,mode=max,scope=${DOCKER_SCOPE}" \
		--build-arg POSTGRESQL_VERSION="${POSTGRESQL_VERSION}" \
		--build-arg GCLOUD_SDK_VERSION="${GCLOUD_SDK_VERSION}"
ifeq (CI, true)
	docker image inspect "${PKG_NAME}" | jq
	docker images
endif

.PHONY: test
test:
	docker run --rm --entrypoint bash "${HUB_NAME}" -c "gcloud --version && psql --version"
