## kubernetes
kubernetes offer:
- high availability or no downtime
- scalability
- disaster recovery
## kubernetes components
- Pods:
    - smallest unit of k8s
    - abstraction over container
    - usually 1 application per pod
    - internal ip address that change when the pod stop
- Service:
    - permanent IP address linked to the pod
    - lifecycle of Pod and service are not connected so we always keep the exposed pemanent IP even when the pod restart
    - work as a load balancer 
- Ingress:
    - expose the services externally through an url
- ConfigMap:
    - external configration of your application
- Secret:
    - like a configmap but used to store secrets
    - base64 encoded
    - credentials, secrets, certificates
    - the built-in security mechanism is not enabled by default
    - connect it to the pod so it can read the secrets from it
- volumes:
    - k8s doesn't manage data persistance, that's why we need volumes to link a local or remote storage to a pod
    - storages are seperate from the cluster
- deployment:
    - an abstraction over pods
    - DB can't be replicated via deployment
- statefulset:
    - used for stateful apps: mongodb, mysql ...
    - use statefulset instead of deployment for statefull apps
    - not easy
    - DB are often hosted outside of k8s

## kubernetes architecture
- master node:
    - api server (the entrypoint of the cluster)
    - scheduler (lanch commande to start or stop a pod)
    - controller manager (make sure that the state of cluster match the demanded state, example number of running replicas)
    - etcd (key-value store that contains information abount the state of the cluster)
- worker node:
    - kubelet: start and stop pods
    - container runtime (example: docker)
    - kube proxy

## kubectl commands
```
kubectl create deployment [name]
example: kubectl create deployment nginx-depl --image=nginx
kubectl edit deployment [name]
kubectl delete deployment [name]
kubectl get nodes | pod | services | replicaset | deployment
kubectl describe pod [podname]
kubectl logs [podname]
kubectl exec -it [podname] -- bin/bash
```
```
kubectl get deployments -o wide
kubectl get deployment [deploymentname] -o yaml > deployment-result.yaml
kubectl delete -f nginx-deployment.yaml
kubectl delete -f nginx-service.yaml 
kubectl get all | grep mongo  # find all resource that have mongo in their name
```

## yaml config files
a config file is composed of 3 parts:
- metadata
- spec
- status, contains the desired state and the actual state (auto generated, we don't need to worry about)
kubernetes get the status informations from the etcd
- order of execution matter, secrets and configmaps should be applied first
```
#  when creating an external service inside minikube
# we need this command to assign an external ip to it
minikube service [service-name]
```