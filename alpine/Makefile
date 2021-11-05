SHELL=/bin/bash -o pipefail

REGISTRY   ?= appscode
BIN        := curl
IMAGE      := $(REGISTRY)/$(BIN)
VERSION    ?= alpine
SRC_REG    ?=

DOCKER_PLATFORMS := linux/amd64 linux/arm64 # linux/ppc64le linux/s390x
PLATFORM         ?= $(firstword $(DOCKER_PLATFORMS))
TAG              = $(VERSION)_$(subst /,_,$(PLATFORM))

container-%:
	@$(MAKE) container \
	    --no-print-directory \
	    PLATFORM=$(subst _,/,$*)

push-%:
	@$(MAKE) push \
	    --no-print-directory \
	    PLATFORM=$(subst _,/,$*)

all-container: $(addprefix container-, $(subst /,_,$(DOCKER_PLATFORMS)))

all-push: $(addprefix push-, $(subst /,_,$(DOCKER_PLATFORMS)))

ifeq (,$(SRC_REG))
container:
	@echo "container: $(IMAGE):$(TAG)"
	@docker buildx build --platform $(PLATFORM) --load --pull -t $(IMAGE):$(TAG) -f Dockerfile .
	@echo
else
container:
	@echo "container: $(IMAGE):$(TAG)"
	@docker tag $(SRC_REG)/$(BIN):$(TAG) $(IMAGE):$(TAG)
	@echo
endif

push: container
	@docker push $(IMAGE):$(TAG)
	@echo "pushed: $(IMAGE):$(TAG)"
	@echo

.PHONY: manifest-version
manifest-version:
	docker manifest create -a $(IMAGE):$(VERSION) $(foreach PLATFORM,$(DOCKER_PLATFORMS),$(IMAGE):$(VERSION)_$(subst /,_,$(PLATFORM)))
	docker manifest push $(IMAGE):$(VERSION)

.PHONY: docker-manifest
docker-manifest: manifest-version

.PHONY: release
release:
	@$(MAKE) all-push docker-manifest --no-print-directory
