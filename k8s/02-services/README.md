# Services: Connecting Pods over the network

A Pod, which is a virtual environment assigned an IP address by Kubernetes, is a disposable resource that is governed by another resource. When a Pod needs to communicate with another, it can utilize the IP address. Nonetheless, this approach presents two issues: firstly, the IP address changes when the Pod is replaced, and secondly, there is no straightforward method to obtain a Pod's IP address as it can only be accessed through the Kubernetes API.

```shell
$ docker build -t sleepy:1.0.0 -f docker-image/sleepy/Dockerfile .
$ kind load docker-image sleepy:1.0.0 --name local-dev
$ kubectl apply -f k8s-manifest/sleepy/sleepy-1-pod.yaml -f k8s-manifest/sleepy/sleepy-2-pod.yaml
$ kubectl wait --for=condition=Ready pod -l app=sleep-1
# check the IP address
$ kubectl get po -l app=sleep-1 --output jsonpath='{.items[0].status.podIP}'
# Ping the second Pod from the first one
$ kubectl exec po/sleep-2 -- ping -c 2 $(kubectl get po -l app=sleep-1 --output jsonpath='{.items[0].status.podIP}')

```

k8s virtual network extends across the entire cluster, allowing Pods to communicate using their IP addresses, regardless of the node they're operating on.

```shell
# it contains a static IP address for their whole lifetime
$ kubectl get po -l app=sleep-2 -o jsonpath='{.items[0].status.podIP}'
# deployment will create a replacement
$ kubectl delete po -l app=sleep-2
# it has new created IP address
$ kubectl get po -l app=sleep-2 -o jsonpath='{.items[0].status.podIP}'
```

k8s comes equipped with a DNS server that is capable of mapping Service names to their corresponding IP addresses and services allow Pods to communicate using a fixed domain name.

```shell
$ kubectl apply -f k8s-manifest/sleepy/sleepy-1-service.yaml -f k8s-manifest/sleepy/sleepy-2-service.yaml
$ kubectl get svc sleep-2-service
# it will fail.
$ kubectl exec deploy/sleep-1 -- ping -c 2 sleep-2
```
