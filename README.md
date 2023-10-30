# copatcher

[![Build Status](https://github.com/craftslab/copatcher/workflows/ci/badge.svg?branch=main&event=push)](https://github.com/craftslab/copatcher/actions?query=workflow%3Aci)
[![codecov](https://codecov.io/gh/craftslab/copatcher/branch/main/graph/badge.svg?token=7PMQALLZLY)](https://codecov.io/gh/craftslab/copatcher)
[![Go Report Card](https://goreportcard.com/badge/github.com/craftslab/copatcher)](https://goreportcard.com/report/github.com/craftslab/copatcher)
[![License](https://img.shields.io/github/license/craftslab/copatcher.svg)](https://github.com/craftslab/copatcher/blob/main/LICENSE)
[![Tag](https://img.shields.io/github/tag/craftslab/copatcher.svg)](https://github.com/craftslab/copatcher/tags)



## Introduction

*copatcher* is a container patcher written in Go.



## Prerequisites

- Go >= 1.18.0



## Install

```bash
# container-diff
curl -LO https://storage.googleapis.com/container-diff/latest/container-diff-linux-amd64
sudo install container-diff-linux-amd64 /usr/local/bin/container-diff

# buildkit
docker pull moby/buildkit:latest
```



## Run

```bash
# container-diff
container-diff diff --type=apt --type=node --type=pip \
  --json daemon://ubuntu:22.04 daemon://ubuntu:23.04 > diff.json

# buildkit
docker run --detach --rm --privileged --name buildkitd \
  --entrypoint buildkitd moby/buildkit:latest
# OR
docker run --detach --rm --privileged -p 127.0.0.1:8888:8888/tcp --name buildkitd \
  --entrypoint buildkitd moby/buildkit:latest --addr tcp://0.0.0.0:8888

# copatcher
version=latest make build
./bin/copatcher --image ubuntu:22.04 --report diff.json --tag 22.04-patched --timeout 5m \
  --addr docker-container://buildkitd --ignore-errors
# OR
./bin/copatcher --image ubuntu:22.04 --report diff.json --tag 22.04-patched --timeout 5m \
  --addr tcp://0.0.0.0:8888 --ignore-errors
```



## Docker

```bash
version=latest make docker
docker run ghcr.io/craftslab/copatcher:latest
```



## Usage

```
TBD
```



## Design

![design](design.png)



## License

Project License can be found [here](LICENSE).



## Reference

- [container-diff](https://github.com/GoogleContainerTools/container-diff)
- [copacetic](https://project-copacetic.github.io/copacetic/website/)
- [dockerfile-generator](https://www.startwithdocker.com/)
