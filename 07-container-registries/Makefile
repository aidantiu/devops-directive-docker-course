.PHONY: build
build:
	echo "FROM scratch" > Dockerfile
	docker build --tag try-repo .
	-rm Dockerfile
	

# Have to authenticate to dockerhub and create repo first:
# https://docs.docker.com/engine/reference/commandline/login/
.PHONY: push-dockerhub
push-dockerhub:
	docker tag try-repo ymnwri/try-repo:1
	docker push ymnwri/try-repo:1

# Have to authenticate to ghcr.io first
# https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry
.PHONY: push-github-packages
push-github-packages:
	docker tag my-scratch-image ghcr.io/sidpalas/my-scratch-image # defaults to latest
	docker push ghcr.io/sidpalas/my-scratch-image

	docker tag my-scratch-image ghcr.io/sidpalas/my-scratch-image:abc-123 # defaults to latest
	docker push ghcr.io/sidpalas/my-scratch-image:abc-123
