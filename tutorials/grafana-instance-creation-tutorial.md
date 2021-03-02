---
title: Istio service mesh Instance Creation 
description: Learn how to create instances of your Istio service mesh
---

###  Create Istio service mesh

**Step 1: First, create the yaml definition of the Custom Resource for Istio service mesh as below.**

```execute
cat <<'EOF' > Istioservicemesh.yaml
apiVersion: istio.banzaicloud.io/v1beta1
kind: Istio
metadata:
  name: istio-sample
spec:
  autoInjectionNamespaces:
    - default
  citadel:
    image: 'docker.io/istio/citadel:1.1.2'
    replicaCount: 1
  defaultPodDisruptionBudget:
    enabled: true
  galley:
    image: 'docker.io/istio/galley:1.1.2'
    replicaCount: 1
  gateways:
    egress:
      maxReplicas: 5
      minReplicas: 1
      replicaCount: 1
      sds:
        image: node-agent-k8s
    ingress:
      maxReplicas: 5
      minReplicas: 1
      replicaCount: 1
      sds:
        image: node-agent-k8s
    k8singress: {}
  imageHub: docker.io/istio
  imageTag: 1.1.0
  includeIPRanges: '*'
  mixer:
    image: 'docker.io/istio/mixer:1.1.2'
    maxReplicas: 5
    minReplicas: 1
    replicaCount: 1
  mtls: false
  nodeAgent:
    image: 'docker.io/istio/node-agent-k8s:1.1.2'
  outboundTrafficPolicy:
    mode: ALLOW_ANY
  pilot:
    image: 'docker.io/istio/pilot:1.1.2'
    maxReplicas: 5
    minReplicas: 1
    replicaCount: 1
    traceSampling: 1
  proxy:
    image: 'docker.io/istio/proxyv2:1.1.2'
  proxyInit:
    image: 'docker.io/istio/proxy_init:1.1.2'
  sds: {}
  sidecarInjector:
    image: 'docker.io/istio/sidecar_injector:1.1.2'
    replicaCount: 1
    rewriteAppHTTPProbe: true
  tracing:
    zipkin:
      address: 'zipkin.istio-system:9411'
  version: 1.1.2
EOF
```

**Step 2: Execute the command below to create your Istio service mesh instance.**

```execute
kubectl create -f Istioservicemesh.yaml -n operators
```
You will see the following resources created:

```output
istio.istio.banzaicloud.io/istio-sample created
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



