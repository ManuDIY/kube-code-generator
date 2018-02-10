Kube code generator [![Docker Build Status](https://img.shields.io/docker/build/slok/kube-code-generator.svg)](https://hub.docker.com/r/slok/kube-code-generator) [![Docker Automated build](https://img.shields.io/docker/automated/slok/kube-code-generator.svg)](https://hub.docker.com/r/slok/kube-code-generator)
===================

A kubernetes code generator ready container to create your CRDs.

This uses the [official util](https://github.com/kubernetes/code-generator) created by Kubernetes to autogenerate the code required by Kubernetes resources.

## Env vars required

* `PROJECT_PACKAGE`: The project package path.
* `CLIENT_GENERATOR_OUT`: The client generated out path.
* `APIS_ROOT`: The path where are our API/resources.
* `GROUPS_VERSION`: The groups or the resources.
* `GENERATION_TARGETS`: The wanted generated code. [(deepcopy,defaulter,client,lister,informer) or "all".]

## Usage

Having a project that would be on `github.com/someone/myproject`, wants to generate the client on `github.com/someone/myproject/client`, has its resources on `github.com/someone/myproject/apis`, has the resources `github.com/someone/myproject/apis/test/v1alpha1` and `github.com/someone/myproject/apis/test2/v1` and wants only `deepcopy` and `client` autogenerated code this would be:


```bash
PROJECT_PACKAGE=/go/src/github.com/someone/myproject && \
docker run -it --rm \
	-v ${PWD}:/go/src/${PROJECT_PACKAGE}\
	-e PROJECT_PACKAGE=${PROJECT_PACKAGE} \
	-e CLIENT_GENERATOR_OUT=${PROJECT_PACKAGE}/client \
	-e APIS_ROOT=${PROJECT_PACKAGE}/apis \
	-e GROUPS_VERSION="test:v1alpha1 test2:v1" \
	-e GENERATION_TARGETS="deepcopy,client" \
    slok/kube-code-generator:latest
```

## Example

You have a project example in [example](example/) path.