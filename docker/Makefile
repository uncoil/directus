SHELL=bash

version=$(shell git describe --tags --abbrev=0)
tag=latest
cmd=
user=directus
registry=gcr.io
repository=uncoil-io/directus
tmp_package_json=$(mktemp)

.PHONY: build-images

build-images:
	pnpm install
	pnpm -r build
	cd ../; node docker/pack;
	docker buildx build --platform linux/amd64 \
		--build-arg VERSION=$(version) \
		--build-arg REPOSITORY=$(repository) \
		--load \
		-t directus:temp \
		-f ./Dockerfile \
		..

	docker tag directus:temp $(registry)/$(repository):$(version)
	docker tag directus:temp $(registry)/$(repository):$(tag)
	docker image rm directus:temp

test-image:
	docker run --rm -it $(registry)/$(repository):$(tag) $(cmd)
