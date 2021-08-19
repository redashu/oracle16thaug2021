# k8s Revision 

<img src="k8srev.png">

## k8s client story 

<img src="clientk8s.png">

## token config file in k8s cluster side 

```
root@masternode ~]# cd  /etc/kubernetes/
[root@masternode kubernetes]# ls
admin.conf  controller-manager.conf  kubelet.conf  manifests  pki  scheduler.conf
[root@masternode kubernetes]# 

```

### kubectl read  k8s master auth details  from  home directory or any user in any os under .kube/config file 

<img src="config.png">

## COnnecting and switching to multi k8s cluster using context concept 

```
❯ minikube update-context
🎉  "minikube" context has been updated to point to 127.0.0.1:51739
💗  Current context is "minikube"
❯ kubectl  get  nodes
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   7d18h   v1.21.2
❯ kubectl  config  get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
          kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   
*         minikube                      minikube     minikube           
❯ 
❯ kubectl  config  use-context  kubernetes-admin@kubernetes
Switched to context "kubernetes-admin@kubernetes".
❯ kubectl  get  nodes
NAME         STATUS   ROLES                  AGE   VERSION
masternode   Ready    control-plane,master   77m   v1.22.0
minion1      Ready    <none>                 75m   v1.22.0
minion2      Ready    <none>                 74m   v1.22.0
❯ 
❯ kubectl  config  get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   
          minikube                      minikube     minikube           
❯ kubectl  config  use-context  minikube
Switched to context "minikube".
❯ kubectl  get  nodes
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   7d18h   v1.21.2


```

## introduction to POD

<img src="pod.png">

### checking snyntax of YAML 

```
❯ kubectl  apply -f  ashupod1.yaml --dry-run=client
pod/ashupod-1 created (dry run)

```

### deploy pod 

```
❯ kubectl  apply -f  ashupod1.yaml
pod/ashupod-1 created
❯ kubectl  get  pods
NAME        READY   STATUS    RESTARTS   AGE
ashupod-1   1/1     Running   0          8s

```

### checking pods 

```
❯ kubectl  get pods
NAME           READY   STATUS    RESTARTS   AGE
ashupod-1      1/1     Running   0          9m18s
karthikpod-1   1/1     Running   0          5m4s
padma-1        1/1     Running   0          2m33s
poorvipod-1    1/1     Running   0          8m25s
sahilpod-1     1/1     Running   0          112s
skpod-1        1/1     Running   0          7m52s
srinipod-1     1/1     Running   0          8m53s
yashpod-1      1/1     Running   0          4m25s

```


##  deleting pods 

```
kubectl  delete  pod  sahilpod-1

```


### checking output of container inside pod 

```
kubectl  logs  -f  ashupod-1

```

### checking scheduling of pod 

```
❯ kubectl  get pod ashupod-1  -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP                NODE      NOMINATED NODE   READINESS GATES
ashupod-1   1/1     Running   0          12m   192.168.179.196   minion2   <none>           <none>

```

## accessing container inside pod 

```
❯ kubectl  exec -it  ashupod-1  -- sh
/ # 
/ # cat  /etc/os-release 
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.14.1
PRETTY_NAME="Alpine Linux v3.14"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://bugs.alpinelinux.org/"
/ # exit


```

