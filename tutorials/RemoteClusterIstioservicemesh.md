---
title: Remote Cluster of an Istio service mesh Instance Creation 
description: Learn how to create instances of Remote Cluster of an Istio service mesh
---

###  Create Remote Cluster of an Istio service mesh

**Step 1: First, create the yaml definition of the Custom Resource for Remote Cluster of an Istio service mesh as below.**

```execute
cat <<'EOF' > RemoteClusterIstioservicemesh.yaml
apiVersion: istio.banzaicloud.io/v1beta1
kind: RemoteIstio
metadata:
  name: remoteistio-sample
spec:
  autoInjectionNamespaces:
    - default
  citadel:
    enabled: true
    replicaCount: 1
  enabledServices:
    - labelSelector: istio=pilot
      name: istio-pilot
    - labelSelector: istio-mixer-type=policy
      name: istio-policy
    - labelSelector: statsd-prom-bridge
      name: istio-statsd
    - labelSelector: istio-mixer-type=telemetry
      name: istio-telemetry
    - labelSelector: app=jaeger
      name: zipkin
  includeIPRanges: '*'
  proxy: {}
  proxyInit: {}
  sidecarInjector:
    enabled: true
    initCNIConfiguration: {}
    replicaCount: 1
EOF
```

**Step 2: Execute the command below to create your Istio service mesh instance.**

```execute
kubectl create -f RemoteClusterIstioservicemesh.yaml -n operators
```
You will see the following resources created:

```output
istio.istio.banzaicloud.io/remoteistio-sample created
```

Wait till Pod STATUS is "Running", then proceed.


**Step 3: Check the pod status.**

```execute
kubectl get pods -n operators
```

You will see a similar output as below:

```
NAME                                      READY   STATUS    RESTARTS   AGE
istio-citadel-7b97584d7-bg9r8             1/1     Running   0          6m36s
istio-egressgateway-6f99fd887c-b8jzb      1/1     Running   0          6m36s
istio-galley-564cf94866-fmvc8             1/1     Running   0          6m36s
istio-ingressgateway-55c7745779-2wzh4     1/1     Running   0          6m36s
istio-operator-67cdccc596-s7mf5           1/1     Running   0          13m
istio-pilot-5bb5f47b7c-xtvlk              2/2     Running   0          6m36s
istio-policy-7f9c654dbf-tb77h             2/2     Running   0          6m36s
istio-sidecar-injector-68b459ccc8-rfln8   1/1     Running   0          6m27s
istio-telemetry-675fbccd4d-lltqd          2/2     Running   0          6m35s
```


### Conclusion

Now you are able to create Istio Remote Service Mesh and verified pods are running.
