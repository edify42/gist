# no-docker

Hmmm another day, another monetary decision making process done at docker to
to push people into paying them. Fair play, and we should support them. But I'm
typing this on a work laptop and work should pay for this as it _should_ be an
enterprise plan we're on if I did run docker-for-desktop on my macbook.

Never-the-less, here we are and here's how to run docker with more flexibility:

## remove docker desktop

obvious. should remove most docker stuff except the cli.

## get minikube

I use k8s a lot so it's quite useful.

## manually install buildx

```bash
brew install buildkit # necessary?

...
BUILDX_BINARY_URL="https://github.com/docker/buildx/releases/download/v0.4.2/buildx-v0.4.2.linux-amd64"

curl --output docker-buildx \
  --silent --show-error --location --fail --retry 3 \
  "$BUILDX_BINARY_URL"

mkdir -p ~/.docker/cli-plugins

mv docker-buildx ~/.docker/cli-plugins/
chmod a+x ~/.docker/cli-plugins/docker-buildx
docker buildx install

export BUILDX_PLATFORMS=linux/amd64,linux/arm64,linux/ppc64le,linux/s390x,linux/386,linux/arm/v7,linux/arm/v6
docker run --rm --privileged tonistiigi/binfmt:latest --install "$BUILDX_PLATFORMS"

```

We should have:

```bash
docker buildx ls
NAME/NODE DRIVER/ENDPOINT  STATUS PLATFORMS
default * docker-container
  default default          Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

Oh right...

```bash
eval $(minikube docker-env)
docker buildx ls
NAME/NODE DRIVER/ENDPOINT STATUS  PLATFORMS
default * docker
  default default         running linux/amd64, linux/386, linux/arm64, linux/ppc64le, linux/s390x, linux/arm/v7, linux/arm/v6
```
