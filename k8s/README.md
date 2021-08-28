# k8s

## Core

- A container is a virtualized environment that typically runs a single application component.
- k8s is a platform for running containers (aka container orchestrator).
- The 2 core concepts in k8s are the API, which you use to define your applications, and the cluster, which runs your applications:
  - A cluster is a single logical unit composed of many server nodes. Some nodes run the k8s API, whereas others run application workloads, all in containers.
  - Each node has a container runtime installed i.e: Docker, containerd and rkt.
  - Applications are defined in YAML files and deployed by sending the YAML to the cluster. Users manage k8s applications remotely, using a command-line tool that talks to the k8s API.
    - Those YAML files are properly called _application manifests_, because they're a list of all the components shipped within the app. Those components are k8s resources:
      - _Services_ are k8s objects for managing network access. They may send traffic from outside world or between containers in the cluster.
      - _ConfigMaps_ and _secrets_ handle app configuration
      - _Volumes_ handle storage
      - _Pods_, _Replicasets_ and _deployments_ are k8s objects to support scale, rolling upgrades and complex deployment patterns.

## Prerequisites

Install all of the following list:

- [Docker](https://www.docker.com/products/docker-desktop)
- [k3s](https://k3s.io/) or [kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- [kubectl](https://kubernetes.io/docs/tasks/tools/). It connects to a k8s cluster. Both Docker or k3s install kubectl for you.

Verification

```
    :~$ kubectl get nodes

    NAME                          STATUS   ROLES                  AGE   VERSION
    local-cluster-control-plane   Ready    control-plane,master   56d   v1.20.2
```

## Contents

1. [Pods](01-pods/README.md)
2. [Services](02-services/README.md)
