<h1 align="center">Grafana Operator</h1>

![Logo](_images/logo.PNG)


### Overview:

Istio is an open platform to connect, manage, and secure microservices and it is emerging as the standard for building service meshes on Kubernetes. It is built out on multiple components and a rather complex deployment scheme (around 14 Helm subcharts and 50+ CRDs). Installing, upgrading and operating these components requires deep understanding of Istio and Helm (the standard/supported way of deploying Istio).

The goal of the Istio-operator is to automate and simplify these and enable popular service mesh use cases (multi cluster federation, canary releases, resource reconciliation, etc) by introducing easy higher level abstractions.

### Operator's features are as follows:

The Operator can deploy and manage a Grafana instance on Kubernetes and OpenShift. The following features are supported:

- Install Grafana to a namespace.
- Configure Grafana through the custom resource.
- Import Grafana dashboards from the same or other namespaces.
- Import Grafana data sources from the same namespace.
- Install Plugins (panels).

### Grafana Operator Architecture

Grafana allows you to query, visualize and understand your metrics. 
The data was pulled from Prometheus which was plugged-in to the Grafana dashboard as a data source. Queries were fired from the dashboard with different expressions such as min, avg etc.Grafana has native Prometheus support.It comes with a large number of inbuilt reusable dashboards to bring your data together and share it.

A high level Prometheus & Grafana Architecture diagram is shown below :

![](_images/Grafana-Architecture.png)



### Objective of tutorial

In this tutorial,we are going to cover following topics:

1. Install Grafana Operator and verify its successful installation.
2. Create Grafana Instance and verify status of pods and services.
3. Monitoring a DB or any server using Prometheus and Grafana Operators.
4. How to access Grafana dashboard to visualize the different metrics.
5. Cleanup Operator.
