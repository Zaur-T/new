### Start a Kubernetes Cluster
```bash
$ k3d cluster create gdp-test-assignment \
		--servers 3 --agents 3 \
		--k3s-node-label 'type=control@server:0,1,2' \
		--k3s-node-label 'type=worker@agent:0,1,2'

INFO[0000] Prep: Network                                
INFO[0000] Created network 'k3d-gdp-test-assignment'    
INFO[0000] Created image volume k3d-gdp-test-assignment-images 
INFO[0000] Starting new tools node...                   
INFO[0000] Creating initializing server node            
INFO[0000] Creating node 'k3d-gdp-test-assignment-server-0' 
INFO[0000] Starting Node 'k3d-gdp-test-assignment-tools' 
INFO[0001] Creating node 'k3d-gdp-test-assignment-server-1' 
INFO[0002] Creating node 'k3d-gdp-test-assignment-server-2' 
INFO[0002] Creating node 'k3d-gdp-test-assignment-agent-0' 
INFO[0002] Creating node 'k3d-gdp-test-assignment-agent-1' 
INFO[0003] Creating node 'k3d-gdp-test-assignment-agent-2' 
INFO[0003] Creating LoadBalancer 'k3d-gdp-test-assignment-serverlb' 
INFO[0003] Using the k3d-tools node to gather environment information 
INFO[0003] HostIP: using network gateway 172.20.0.1 address 
INFO[0003] Starting cluster 'gdp-test-assignment'       
INFO[0003] Starting the initializing server...          
INFO[0003] Starting Node 'k3d-gdp-test-assignment-server-0' 
INFO[0007] Starting servers...                          
INFO[0007] Starting Node 'k3d-gdp-test-assignment-server-1' 
INFO[0035] Starting Node 'k3d-gdp-test-assignment-server-2' 
INFO[0092] Starting agents...                           
INFO[0095] Starting Node 'k3d-gdp-test-assignment-agent-1' 
INFO[0095] Starting Node 'k3d-gdp-test-assignment-agent-2' 
INFO[0095] Starting Node 'k3d-gdp-test-assignment-agent-0' 
INFO[0162] Starting helpers...                          
INFO[0164] Starting Node 'k3d-gdp-test-assignment-serverlb' 
INFO[0177] Injecting records for hostAliases (incl. host.k3d.internal) and for 7 network members into CoreDNS configmap... 
INFO[0190] Cluster 'gdp-test-assignment' created successfully! 
INFO[0192] You can now use it like this:                
kubectl cluster-info
```

### Verify the Cluster
```bash
$ kubectl cluster-info
Kubernetes control plane is running at https://0.0.0.0:35835
CoreDNS is running at https://0.0.0.0:35835/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:35835/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

```

```bash
$ kubectl get all -A
NAMESPACE     NAME                                          READY   STATUS      RESTARTS   AGE
kube-system   pod/coredns-96cc4f57d-lfrbn                   1/1     Running     0          8m15s
kube-system   pod/helm-install-traefik--1-wtpjg             0/1     Completed   1          8m15s
kube-system   pod/helm-install-traefik-crd--1-rh849         0/1     Completed   0          8m15s
kube-system   pod/local-path-provisioner-84bb864455-8f4jv   1/1     Running     0          8m15s
kube-system   pod/metrics-server-ff9dbcb6c-t6cff            1/1     Running     0          8m15s
kube-system   pod/svclb-traefik-6nd7s                       2/2     Running     0          6m4s
kube-system   pod/svclb-traefik-8lwhm                       2/2     Running     0          6m4s
kube-system   pod/svclb-traefik-hwlz4                       2/2     Running     0          6m4s
kube-system   pod/svclb-traefik-jn94m                       2/2     Running     0          6m4s
kube-system   pod/svclb-traefik-mq55s                       2/2     Running     0          6m4s
kube-system   pod/svclb-traefik-rpp5f                       2/2     Running     0          6m4s
kube-system   pod/traefik-56c4b88c4b-lnsdq                  1/1     Running     0          6m5s

NAMESPACE     NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP                                                         PORT(S)                      AGE
default       service/kubernetes       ClusterIP      10.43.0.1      <none>                                                              443/TCP                      8m38s
kube-system   service/kube-dns         ClusterIP      10.43.0.10     <none>                                                              53/UDP,53/TCP,9153/TCP       8m32s
kube-system   service/metrics-server   ClusterIP      10.43.162.86   <none>                                                              443/TCP                      8m29s
kube-system   service/traefik          LoadBalancer   10.43.48.30    172.20.0.2,172.20.0.3,172.20.0.4,172.20.0.5,172.20.0.6,172.20.0.7   80:30715/TCP,443:30442/TCP   6m5s

NAMESPACE     NAME                           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
kube-system   daemonset.apps/svclb-traefik   6         6         6       6            6           <none>          6m4s

NAMESPACE     NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/coredns                  1/1     1            1           8m32s
kube-system   deployment.apps/local-path-provisioner   1/1     1            1           8m31s
kube-system   deployment.apps/metrics-server           1/1     1            1           8m29s
kube-system   deployment.apps/traefik                  1/1     1            1           6m5s

NAMESPACE     NAME                                                DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/coredns-96cc4f57d                   1         1         1       8m15s
kube-system   replicaset.apps/local-path-provisioner-84bb864455   1         1         1       8m15s
kube-system   replicaset.apps/metrics-server-ff9dbcb6c            1         1         1       8m15s
kube-system   replicaset.apps/traefik-56c4b88c4b                  1         1         1       6m5s

NAMESPACE     NAME                                 COMPLETIONS   DURATION   AGE
kube-system   job.batch/helm-install-traefik       1/1           2m21s      8m26s
kube-system   job.batch/helm-install-traefik-crd   1/1           109s       8m26s
```
