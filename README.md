# k8s-init-containers
Kubernetes (K8s) Init Container Pattern in action (YAML) - Non-Helm version

#### To execute the init container in the 'init-container-pod.yaml' and verify init-container-pod works

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
