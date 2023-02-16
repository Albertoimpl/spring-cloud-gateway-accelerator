# Spring Cloud Gateway for Kubernetes

This is a sample of a Java Spring app that works with Spring Cloud Gateway and the Tanzu Application Platform.

## Dependencies

1. [kubectl CLI](https://kubernetes.io/docs/tasks/tools/)
2. Tanzu CLI and the apps plugin [Tanzu Application Platform](https://network.tanzu.vmware.com/products/tanzu-application-platform)
3. A cluster with Tanzu Application Platform, and the "Default Supply Chain", plus its dependencies. This supply chains
   is part of [Tanzu Application Platform](https://network.tanzu.vmware.com/products/tanzu-application-platform).

## Running the sample

Spring Cloud Gateway for Kubernetes is an optional component, so it has to be installed with:

```bash
tanzu package install spring-cloud-gateway --namespace tap-install --package-name spring-cloud-gateway.tanzu.vmware.com --version 1.2.6
```

We are going to deploy a simple service that returns hello:

```bash
tanzu app workload apply -f config/workload.yaml
```

And create our Spring Cloud Gateway instance with a route that points to the hello-service:

```bash
kubectl apply -f gateway/
```

On this example, we have chosen for the Gateway to create a Service with type LoadBalancer:
```bash
GATEWAY_LOADBALANCER_IP=$(kubectl get svc hello-gateway -o=jsonpath='{.status.loadBalancer.ingress[0].ip}')
curl "${GATEWAY_LOADBALANCER_IP}/gateway/hello"
```

Should output:
```text
Greetings from Spring Boot + Tanzu!%
```
