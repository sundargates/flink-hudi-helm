# Quick Start

## Prerequisites
Install the helm chart for Flink on Kubernetes. See [Flink on Kubernetes](https://nightlies.apache.org/flink/flink-kubernetes-operator-docs-main/docs/try-flink-kubernetes-operator/quick-start/) for more details.

## Setting up a docker registry
1. Point docker-cli to Docker running in minikube
    ```sh
    eval $(minikube -p minikube docker-env)
    ```
1. Start local Docker registry in minikube Docker (the `docker` command is talking to minikube Docker)
    ```sh
    docker run -d -p 5001:5000 --restart=always --name registry registry:2
    ```
   We tunnel port `5001` to `5000`, you can choose any available port.

## Building the Hudi Flink Docker Image
We need a custom docker image that extends the flink base image and adds the hudi flink example jar to it.
You can build this docker image by running the following command:
```shell
./gradlew :example:dockerPushImage
```

This should have pushed the docker image to the docker registry.
You can verify this by running
```shell
docker images
```

This should show you the docker image `hudi-flink:latest` in the list of images.
```angular2html
REPOSITORY                                           TAG                   IMAGE ID       CREATED          SIZE
localhost:5001/hudi/hudi-flink                       latest                87b936181d74   32 minutes ago   1.05GB
```

## Submitting the Flink Job
We can now submit the Flink job to the Flink cluster running in Kubernetes.
```shell
./gradlew example:createPod
```
This will create a pod in Kubernetes that runs the Flink job.
You can verify this by running
```shell
./gradlew example:openFlinkUI
```
and then opening the Flink UI in your browser by hitting on `http://localhost:8081`.

## Destroying the Flink job
You can destroy the Flink job by running
```shell
./gradlew example:deletePod
```
