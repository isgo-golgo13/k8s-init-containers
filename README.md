# k8s-init-containers
Kubernetes (K8s) Init Container Pattern in action (YAML) - Non-Helm version

![Init Containers Pattern Diagram](https://github.com/isgo-golgo13/k8s-init-containers/blob/main/pngs/init-containers.png)


### Init Container Pattern using K8s Pod w/out K8s Service

At the shell execute the following to create the K8s Pod that includes the dependent nginx container and 
the secondary init (busybox) container:

`kubectl create -f init-container-pod.yaml`

This will create the Pod in the `default` namespace in the K8s cluster. To validate the YAML in the init-container-pod.yaml file, 
run this file against the `kubeval` K8s YAML validator. To see how to install `kubeval` see this site: `kubeval.com/installation/`. 
As its docs reference, `kubeval` installs on Linux or runs runs less instrusively as a Docker container.

To run `kubeval` against the `init-container-pod.yaml` execute it as the following:

`kubeval init-container-pod.yaml` 

The result after executing this should show this (if the YAML is structured correctly):

`PASS - init-container-pod.yaml contains a valid Pod (init-container-pod)`

Then if not previously executed, execute the creation of the Pod as previously shown:

`kubectl create -f init-container-pod.yaml`

The get the status of the created Pod.

`kubectl get pods`

The previous execution should show this:

`init-container-pod   1/1     Running   0          4m30s`

Next execute into the Pod and execute the following `apt-get` instructions to install `curl`:

`kubectl exec -it init-container-pod -- /bin/sh`
`apt-get update && apt-get install -y curl`

Finally `curl` the localhost of the nginx container and the served html content will show what 
the init container generated into the index.html file.




### Init Container Pattern using K8s Deployment Controller w/ K8s Service

This version of the init containers patttern includes the nginx dependent container and two init containers 
(busybox-init-container1, busybox-init-container2). The first init container writes html to `/work-dir/index.html` and
the second init container writes additional content to `/work-dir/index.html`.

To execute this second version of the init containers pattern, create and deploy the `init-container-deployment.yaml` Deployment
into the default namespace K8s cluster. 

`kubectl create -f init-container-deployments.yaml` 

To qualify the Deployment is in the cluster, issue the following to see the five Pod replicas are running:

`kubectl get deployments -o wide` and then issue the following:
`kubectl get pods -o wide`

To create and deploy the Service for this Deployment into the K8s cluster issue the following:

`kubectl create -f init-container-service.yaml`

To qualify the Service is running correctly into the K8s cluster.

`kubectl get services -o wide`

To see the init containers in this version executing the content writing into the nginx served index.html page, get the cluster IP and 
append the Service NodePort port # (cluster-ip:node-port)

If running the K8s cluster on Minikube, then running `minikube ip` to get the cluster ip.
