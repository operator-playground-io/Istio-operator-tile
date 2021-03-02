---
title: Istio Operator Installation Verification
description: This tutorial explains how to verify that the Istio Operator has been properly installed in the namespace.
---

### Istio Operator status verification 

**Step 1: Verify that the Istio Operator has been installed successfully by executing below command.**

```execute
kubectl get csv -n operators
```

You will see a similar output as below.

Output:

```
NAME                   DISPLAY   VERSION   REPLACES   PHASE
istio-operator.0.1.6   Istio     0.1.6                Succeeded
```

Note: Please wait for the PHASE status to be "Succeeded", then proceed.

**Step 2: Check the pod status.**

```execute
kubectl get pods -n operators
```

You will see a similar output as below.

Output:

```
NAME                              READY   STATUS    RESTARTS   AGE
istio-operator-67cdccc596-s7mf5   1/1     Running   0          60s
```

### Conclusion

This will verify that Istio Operator has been installed successfully.
