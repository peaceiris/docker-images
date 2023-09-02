DOCKER_CLI_EXPERIMENTAL := enabled
DOCKER_BUILDKIT := 1

DOCKER_USERNAME ?= peaceiris
DOCKER_IMAGE_NAME := robotstxt
DOCKER_HUB_BASE_NAME := ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}
DOCKER_BASE_NAME := ghcr.io/${DOCKER_HUB_BASE_NAME}
DOCKER_VERSION := 0.1.0
DOCKER_TAG := v${DOCKER_VERSION}
GITHUB_REF_NAME ?= local
DOCKER_SCOPE := robotstxt-${GITHUB_REF_NAME}
DOCKER_OUTPUT_TYPE ?= docker
ifdef DOCKER_PLATFORM
	DOCKER_PLATFORM := --platform ${DOCKER_PLATFORM}
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
	docker version

.PHONY: build
build: setup-buildx
	docker buildx build . \
		--tag "${PKG_NAME}" \
		--tag "${HUB_NAME}" \
		${DOCKER_PLATFORM} \
		--label "org.opencontainers.image.version=${DOCKER_VERSION}" \
		--output "type=${DOCKER_OUTPUT_TYPE}" \
		--cache-from "type=gha,scope=${DOCKER_SCOPE}" \
		--cache-to "type=gha,mode=max,scope=${DOCKER_SCOPE}"
ifndef CI
	docker image inspect "${PKG_NAME}" | jq
	docker images
endif
