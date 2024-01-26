---
tags:
  - Docker
  - DockerHub
  - SK8
  - Kubernetes
  - Minikube
  - Orquestration
aliases:
  - Comandos basicos de Docker y Kubernetes
---

## Kubctl

| Commands                                            | Description                                                        |
| --------------------------------------------------- | ------------------------------------------------------------------ |
| kubectl config current-context                      | get the current context                                            |
| kubectl config get-contexts                         | list all context                                                   |
| kubectl config use-context [contextName]            | set the current context                                            |
| kubectl config delete-context [contextName]         | delete a context from the config file                              |
| kubectx [contextName]                               | shorcut, is the same that kubectl config use-context [contextName] |
| kubectl config rename-context [old-name] [new-name] | Rename context                                                     |

![[Pasted image 20240108210629.png|800x500]]
>[!Info] Ejemplos
>forma imperativa -> kubectl create deployment mynginx1 --image=nginx
>forma declarativa con archivo yaml -> kubectl create -f deploy-example.yaml
>Para borrarlos usar 
	kubectl delete deployment mynginx1
    kubectl delete deploy mynginx2


## Namespace
![[Pasted image 20240109114210.png|800x500]]
![[Pasted image 20240109125410.png|800x500]]

| Commands                                                       | Description                                |
| -------------------------------------------------------------- | ------------------------------------------ |
| kubectl get namespace                                          | List all namespaces                        |
| kubectl get ns                                                 | Shortcut                                   |
| kubectl config set-context --current namespace=[namespaceName] | Set the current context to use a namespace |
| kubectl create ns [namespaceName]                              | Create a namespace                         |
| kubectl delete ns [namespaceName]                              | Delete a namespace                         |
| kubectl get pods --all-namespaces                              | List all pods in all namespaces            |

## Pod
| Commands                                                                            | Description                                              |
| ----------------------------------------------------------------------------------- | -------------------------------------------------------- |
| kubectl create -f [pod-definition.yml]                                              | Create a pod                                             |
| kubectl run [podname] --image=busybox -- /bin/sh -c "sleep 3600"                    | Run a pod                                                |
| kubectl get pods                                                                    | List the running pods                                    |
| kubectl get pods -o wide                                                            | Same but with more info                                  |
| kubectl describe pod [podname]                                                      | Show pod info                                            |
| kubectl get pod [podname] -o yaml > file.yaml                                       | Extract the pod definition in YAML and save it to a file |
| kubectl exec -it [podname] -- sh                                                    | Interactive mode                                         |
| kubectl delete -f [pod-definition.yml]                                              | Delete a pod                                             |
| kubectl delete pod [podname]                                                        | Same using the pod's name                                |
| kubectl get nodes                                                                   | Get a list of all the installed nodes                    |
| kubectl describe node                                                               | Get some info about the node                             |
| kubectl delete pod mybox --grace-period=0 --force                                   | cleanup force without grace period                       |
| kubectl run [name] --image=[image] --dry-run=client -o yaml > redis-definition.yaml | Create the manifest in YAML                              |


>[!Info] Periodo de gracia y edicion de un pods levantado
>cuando se elimina un pod se tiene 30 seg de periodo de gracia
>Para editar un pod usar lo siguiente
`kubectl edit pod redis`

![[Pasted image 20240118203437.png|800x500]]
## Init containers
![[Pasted image 20240116164412.png|800x500]]
![[Pasted image 20240116164445.png|800x500]]
![[Pasted image 20240116164503.png|800x500]]
![[Pasted image 20240116164515.png|800x500]]

## Multicontainers Pods
![[Pasted image 20240117142431.png|800x500]]

| Commands                                            | Description                  |
| --------------------------------------------------- | ---------------------------- |
| kubectl create -f [pod-definition.yml]              | Create a pod                 |
| kubectl exec -it [podname] -c [containername] -- sh | Exec into a pod              |
| kubectl logs [podname] -c [containername]           | Get the logs for a container |

## ReplicaSet
![[Pasted image 20240117152702.png|800x500]]
![[Pasted image 20240119200209.png|800x500]]
>[!Warning] match
>En la imagen anterior coinciden el matchlabels del selector y los metadatos/label del template, asi deben ser, si no coinciden se tendra el siguiente error
>![[Pasted image 20240119210319.png]]

![[Pasted image 20240119202403.png|800x500]]

| Commands                                     | Description                                 |
| -------------------------------------------- | ------------------------------------------- |
| kubectl apply -f [definition.yaml]           | Create a ReplicaSet                         |
| kubectl get rs                               | List ReplicaSets                            |
| kubectl describe rs [rsName]                 | Get info                                    |
| kubectl delete -f [definition.yaml]          | Delete a ReplicaSet                         |
| kubectl delete rs [rsName]                   | Same but using the ReplicaSet name          |
| kubectl replace -f [definition.yaml]         | Reemplazar o actualizar el conjunto RS      |
| kubectl scale rs -replace=6 -f [definition.yml] | update rs without change the definition yml |
| kubectl edit rs [nameofrs]                   | edit rs that its running without yaml file  |


## Deployment vs Pod
![[Pasted image 20240117154820.png|500x300]]
![[Pasted image 20240117154731.png|800x500]]
![[Pasted image 20240117154954.png|800x500]]
![[Pasted image 20240117155222.png|800x500]]
![[Pasted image 20240122180456.png|800x500]]
![[Pasted image 20240122180941.png|800x500]]
![[Pasted image 20240122181136.png|800x500]]

| Commands                                                                      | Description                        |
| ----------------------------------------------------------------------------- | ---------------------------------- |
| Kubectl create deploy [deploymentName] --image-busybox --replicas-3 --port=80 | The imperative way                 |
| kubectl apply -f [definition.yaml]                                            | Create a deployment                |
| kubectl get deploy                                                            | List deployments                   |
| kubectl describe deploy [deploymentName]                                      | Get info                           |
| kubectl get rs                                                                | List replicasets                   |
| kubectl delete -f [definition.yaml]                                           | Delete a deployment                |
| kubectl delete deploy [deploymentName]                                        | Same but using the deployment name |

## Services
![[Pasted image 20240122183836.png|800x500]]
![[Pasted image 20240122183942.png|800x500]]
![[Pasted image 20240122184236.png|800x500]]
![[Pasted image 20240122184330.png|800x500]]
![[Pasted image 20240122184628.png|800x500]]
>[!info] Puertos
>Unico parametro de puerto obligatorio es port que es el puerto de servicio, el targetPort que es el puerto del aplicativo pod y nodeport que es el puerto expuesto no son obligatorios y pueden ser aleatorios sino se detalla, con respecto a nodePort el rango valido es 30000 y 32767. Finalmente para aplicar estas definiciones necesitamos de labels y selectors
![[Pasted image 20240122185911.png|800x500]]
![[Pasted image 20240122190956.png|800x500]]
![[Pasted image 20240122191540.png|800x500]]


## DaemonSets
| Commands                            | Description                       |
| ----------------------------------- | --------------------------------- |
| kubectl apply -f [definition.yaml]  | Create a DaemonSet                |
| kubectl get ds                      | List DaemonSets                   |
| kubectl describe ds [rsName]        | Get info                          |
| kubectl delete -f [definition.yaml] | Delete a DaemonSet                |
| kubectl delete ds [rsName]          | Same but using the DaemonSet name |

## Statefulset
![[Pasted image 20240117173207.png|800x500]]
![[Pasted image 20240117173354.png|800x500]]

| Commands                            | Description                         |
| ----------------------------------- | ----------------------------------- |
| kubectl apply -f [definition.yaml]  | Create a StatefulSet                |
| kubectl get sts                     | List StatefulSets                   |
| kubectl describe sts [rsName]       | Get info                            |
| kubectl delete -f [definition.yaml] | Delete a StatefulSet                |
| kubectl delete sts [rsName]         | Same but using the StatefulSet name |

## Jobs
![[Pasted image 20240117174600.png|800x500]]

| Commands                                     | Description                 |
| -------------------------------------------- | --------------------------- |
| kubectl create job [jobName] --image=busybox | The imperative way          |
| kubectl apply -f [definition.yaml]           | Create a Job                |
| kubectl get job                              | List jobs                   |
| kubectl describe job [jobName]               | Get info                    |
| kubectl delete -f [definition.yaml]          | Delete a job                |
| kubectl delete job [jobName]                 | Same but using the Job name |

## Cronjob
| Commands                                                                                   | Description                     |
| ------------------------------------------------------------------------------------------ | ------------------------------- |
| kubectl create cronjob [jobName] --image-busybox --schedule="*/1****" -- bin/sh -c "date;" | The imperative way              |
| kubectl apply -f [definition.yaml]                                                         | Create a CronJob                |
| kubectl get cj                                                                             | List CronJobs                   |
| kubectl describe cj [jobName]                                                              | Get info                        |
| kubectl delete -f [definition.yaml]                                                        | Delete a CronJob                |
| kubectl delete cj [jobName]                                                                | Same but using the CronJob name |































