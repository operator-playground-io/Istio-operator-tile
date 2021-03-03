
### Deploy the sample application

Follow the steps below to deploy the  Bookinfo sample application.

**Step 1: Execute below command to deploy application.**

```execute
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```

Output:
```
service/details created
serviceaccount/bookinfo-details created
deployment.apps/details-v1 created
service/ratings created
serviceaccount/bookinfo-ratings created
deployment.apps/ratings-v1 created
service/reviews created
serviceaccount/bookinfo-reviews created
deployment.apps/reviews-v1 created
deployment.apps/reviews-v2 created
deployment.apps/reviews-v3 created
service/productpage created
serviceaccount/bookinfo-productpage created
deployment.apps/productpage-v1 created
```


**Step 2: The application will start. As each pod becomes ready, the Istio sidecar will be deployed along with it.**

```execute
kubectl get services
```

Output:

```
NAME                              READY   STATUS    RESTARTS   AGE
details-v1-558b8b4b76-2llld       2/2     Running   0          2m41s
productpage-v1-6987489c74-lpkgl   2/2     Running   0          2m40s
ratings-v1-7dc98c7588-vzftc       2/2     Running   0          2m41s
reviews-v1-7f99cc4496-gdxfn       2/2     Running   0          2m41s
reviews-v2-7d79d5bd5d-8zzqd       2/2     Running   0          2m41s
reviews-v3-7dbcdcbc56-m8dph       2/2     Running   0          2m41s

```

Note: Re-run the previous command and wait until all pods report READY 2/2 and STATUS Running before you go to the next step. This might take a few minutes depending on your platform.

**Step 3: Verify everything is working correctly up to this point. Run this command to see if the app is running inside the cluster and serving HTML pages by checking for the page title in the response:**

```execute
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
```

Output:

```
<title>Simple Bookstore App</title>
```
### Open the application to outside traffic

The Bookinfo application is deployed but not accessible from the outside. To make it accessible, you need to create an Istio Ingress Gateway, which maps a path to a route at the edge of your mesh.

Step 1: Associate this application with the Istio gateway:

```execute
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```

Output:
```
gateway.networking.istio.io/bookinfo-gateway created
virtualservice.networking.istio.io/bookinfo created
```

Step 3: Ensure that there are no issues with the configuration:

```execute
istioctl analyze
```

Output:

```
No validation issues found when analyzing namespace: default.
```

### Determining the ingress IP and ports

Follow these instructions to set the INGRESS_HOST and INGRESS_PORT variables for accessing the gateway. Use the tabs to choose the instructions for your chosen platform:

Step 1: Execute the following command to determine if your Kubernetes cluster is running in an environment that supports external load balancers:

```execute
kubectl get svc istio-ingressgateway -n istio-system
```

Output:

```
NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                      AGE
istio-ingressgateway   LoadBalancer   172.21.109.129   130.211.10.121  80:31380/TCP,443:31390/TCP,31400:31400/TCP   17h
```


If the EXTERNAL-IP value is set, your environment has an external load balancer that you can use for the ingress gateway. If the EXTERNAL-IP value is <none> (or perpetually <pending>), your environment does not provide an external load balancer for the ingress gateway. In this case, you can access the gateway using the service’s node port.

Choose the instructions corresponding to your environment:

Follow these instructions if you have determined that your environment has an external load balancer.

Step 2: Set the ingress IP and portsusing below command.

```
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')

```

In certain environments, the load balancer may be exposed using a host name, instead of an IP address. In this case, the ingress gateway’s EXTERNAL-IP value will not be an IP address, but rather a host name, and the above command will have failed to set the INGRESS_HOST environment variable. Use the following command to correct the INGRESS_HOST value:

```execute
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
```
 
Follow these instructions if your environment does not have an external load balancer and choose a node port instead.



 
Step 3: Set the ingress ports using below command.

```execute
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')

```


Step 4: Set GATEWAY_URL:

```execute
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```

Output:

```
192.168.4.102
```
Step 5: Ensure an IP address and port were successfully assigned to the environment variable:

```execute
echo "$GATEWAY_URL"
```

Output:

```
192.168.99.100:32194
```



### Verify external access

Confirm that the Bookinfo application is accessible from outside by viewing the Bookinfo product page using a browser.

Step 1: Run the following command to retrieve the external address of the Bookinfo application.

```execute
echo "http://$GATEWAY_URL/productpage"
```

Step 2: Paste the output from the previous command into your web browser and confirm that the Bookinfo product page is displayed.

### View the dashboard
Istio integrates with several different telemetry applications. These can help you gain an understanding of the structure of your service mesh, display the topology of the mesh, and analyze the health of your mesh.

Use the following instructions to deploy the Kiali dashboard, along with Prometheus, Grafana, and Jaeger.

Step 1. Install Kiali and the other addons and wait for them to be deployed.

```execute
kubectl apply -f samples/addons
kubectl rollout status deployment/kiali -n istio-system
```

Output:

```
Waiting for deployment "kiali" rollout to finish: 0 of 1 updated replicas are available...
deployment "kiali" successfully rolled out
```

Note:
If there are errors trying to install the addons, try running the command again. There may be some timing issues which will be resolved when the command is run again.


Step 2: Access the Kiali dashboard.

```
istioctl dashboard kiali
```

Step 3: In the left navigation menu, select Graph and in the Namespace drop down, select default.

The Kiali dashboard shows an overview of your mesh with the relationships between the services in the Bookinfo sample application. It also provides filters to visualize the traffic flow.


### Conclusion

Using above steps we are able to evaluate Istio’s features. 
