---
title: Istio Operator cleanup. 
description: Learn how to cleanup the Istio Operator.
---


### Cleaning Up Operator


**Step 1: Delete the operator's Custom Resources by using `kubectl delete` commands as below.**

 
Example:
 
 ```execute
 kubectl delete -f Istioservicemesh.yaml -n operators
 kubectl delete -f RemoteClusterIstioservicemesh.yaml -n operators 
```

 

**Step 2: Delete the operator by using `kubectl delete` command as below.**
 
 
 Example:
 
 ```execute
 kubectl create -f https://operatorhub.io/install/istio.yaml
 ```
 

 
**Step 4: Delete all the yaml files.**
 
 
 ```execute
 rm -rf Istioservicemesh.yaml
 rm -rf RemoteClusterIstioservicemesh.yaml 
```
  
### Conclusion
You have successfully cleaned up the Istio Operator resources.
