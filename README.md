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

### Start a Kubernetes Cluster

**clone cassandr helm chart from bitnami repo**
`git clone https://github.com/bitnami/charts.git`
**change version of helm chart to `8.0.2` in `Chart.yaml` file. Create override values file under `cassandra` chart and name it `gdp-values.yaml`. Content should be as below:**.
```
cluster:
  seedCount: 3
replicaCount: 3
pdb:
  create: true
  maxUnavailable: "2"
  minAvailable: ""
resources:
  requests:
    memory: "64Mi"
    cpu: "100m"
  limits:
    memory: "1000m"
    cpu: "2Gi" 
nodeAffinityPreset:
  values:
    - worker
```

**run `helm dep up` and then run `helm install cassandra cassandra -f cassandra/gdp-vaues.yaml -n cassandra --debug` to check the overrides (see below)`**

```
client.go:128: [debug] creating 6 resource(s)
NAME: cassandra
LAST DEPLOYED: Sun May  1 02:45:27 2022
NAMESPACE: cassandra
STATUS: deployed
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
cluster:
  seedCount: 3
nodeAffinityPreset:
  values:
  - worker
pdb:
  create: true
  maxUnavailable: "2"
replicaCount: 3
resources:
  limits:
    cpu: 2Gi
    memory: 1000m
  requests:
    cpu: 100m
    memory: 64Mi

COMPUTED VALUES:
affinity: {}
args: []
cluster:
  clientEncryption: false
  datacenter: dc1
  enableUDF: false
  endpointSnitch: SimpleSnitch
  extraSeeds: []
  internodeEncryption: none
  name: cassandra
  numTokens: 256
  rack: rack1
  seedCount: 3
clusterDomain: cluster.local
command: []
common:
  exampleValue: common-chart
  global:
    imagePullSecrets: []
    imageRegistry: ""
    storageClass: ""
commonAnnotations: {}
commonLabels: {}
containerPorts:
  cql: 9042
  intra: 7000
  jmx: 7199
  tls: 7001
containerSecurityContext:
  enabled: true
  runAsNonRoot: true
  runAsUser: 1001
customLivenessProbe: {}
customReadinessProbe: {}
customStartupProbe: {}
dbUser:
  existingSecret: ""
  forcePassword: false
  password: ""
  user: cassandra
diagnosticMode:
  args:
  - infinity
  command:
  - sleep
  enabled: false
existingConfiguration: ""
extraDeploy: []
extraEnvVars: []
extraEnvVarsCM: ""
extraEnvVarsSecret: ""
extraVolumeMounts: []
extraVolumes: []
fullnameOverride: ""
global:
  imagePullSecrets: []
  imageRegistry: ""
  storageClass: ""
hostAliases: []
hostNetwork: false
image:
  debug: false
  pullPolicy: IfNotPresent
  pullSecrets: []
  registry: docker.io
  repository: bitnami/cassandra
  tag: 4.0.3-debian-10-r59
initContainers: []
initDBConfigMap: ""
initDBSecret: ""
jvm:
  extraOpts: ""
  maxHeapSize: ""
  newHeapSize: ""
kubeVersion: ""
lifecycleHooks: {}
livenessProbe:
  enabled: true
  failureThreshold: 5
  initialDelaySeconds: 60
  periodSeconds: 30
  successThreshold: 1
  timeoutSeconds: 5
metrics:
  containerPorts:
    http: 8080
    jmx: 5555
  enabled: false
  image:
    pullPolicy: IfNotPresent
    pullSecrets: []
    registry: docker.io
    repository: bitnami/cassandra-exporter
    tag: 2.3.8-debian-10-r23
  podAnnotations:
    prometheus.io/port: "8080"
    prometheus.io/scrape: "true"
  resources:
    limits: {}
    requests: {}
  serviceMonitor:
    additionalLabels: {}
    enabled: false
    honorLabels: false
    interval: ""
    jobLabel: ""
    metricRelabelings: []
    namespace: monitoring
    scrapeTimeout: ""
    selector: {}
nameOverride: ""
networkPolicy:
  allowExternal: true
  enabled: false
nodeAffinityPreset:
  key: ""
  type: ""
  values:
  - worker
nodeSelector: {}
pdb:
  create: true
  maxUnavailable: "2"
  minAvailable: ""
persistence:
  accessModes:
  - ReadWriteOnce
  annotations: {}
  commitStorageClass: ""
  enabled: true
  mountPath: /bitnami/cassandra
  size: 8Gi
  storageClass: ""
podAffinityPreset: ""
podAnnotations: {}
podAntiAffinityPreset: soft
podLabels: {}
podManagementPolicy: OrderedReady
podSecurityContext:
  enabled: true
  fsGroup: 1001
priorityClassName: ""
readinessProbe:
  enabled: true
  failureThreshold: 5
  initialDelaySeconds: 60
  periodSeconds: 10
  successThreshold: 1
  timeoutSeconds: 5
replicaCount: 3
resources:
  limits:
    cpu: 2Gi
    memory: 1000m
  requests:
    cpu: 100m
    memory: 64Mi
schedulerName: ""
service:
  annotations: {}
  clusterIP: ""
  externalTrafficPolicy: Cluster
  extraPorts: []
  loadBalancerIP: ""
  loadBalancerSourceRanges: []
  nodePorts:
    cql: ""
    metrics: ""
  ports:
    cql: 9042
    metrics: 8080
  type: ClusterIP
serviceAccount:
  annotations: {}
  automountServiceAccountToken: true
  create: true
  name: ""
sidecars: []
startupProbe:
  enabled: false
  failureThreshold: 60
  initialDelaySeconds: 0
  periodSeconds: 10
  successThreshold: 1
  timeoutSeconds: 5
tls:
  autoGenerated: false
  certificatesSecret: ""
  clientEncryption: false
  existingSecret: ""
  internodeEncryption: none
  keystorePassword: ""
  passwordsSecret: ""
  resources:
    limits: {}
    requests: {}
  tlsEncryptionSecretName: ""
  truststorePassword: ""
tolerations: []
topologySpreadConstraints: []
updateStrategy:
  type: RollingUpdate
volumePermissions:
  enabled: false
  image:
    pullPolicy: IfNotPresent
    pullSecrets: []
    registry: docker.io
    repository: bitnami/bitnami-shell
    tag: 10-debian-10-r400
  resources:
    limits: {}
    requests: {}
  securityContext:
    runAsUser: 0

HOOKS:
MANIFEST:
---
# Source: cassandra/templates/pdb.yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: cassandra
  namespace: cassandra
  labels:
    app.kubernetes.io/name: cassandra
    helm.sh/chart: cassandra-8.0.2
    app.kubernetes.io/instance: cassandra
    app.kubernetes.io/managed-by: Helm
spec:
  maxUnavailable: 2
  selector:
    matchLabels: 
      app.kubernetes.io/name: cassandra
      app.kubernetes.io/instance: cassandra
---
# Source: cassandra/templates/serviceaccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: cassandra
  namespace: cassandra
  labels:
    app.kubernetes.io/name: cassandra
    helm.sh/chart: cassandra-8.0.2
    app.kubernetes.io/instance: cassandra
    app.kubernetes.io/managed-by: Helm
  annotations:
automountServiceAccountToken: true
---
# Source: cassandra/templates/cassandra-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: cassandra
  namespace: cassandra
  labels:
    app.kubernetes.io/name: cassandra
    helm.sh/chart: cassandra-8.0.2
    app.kubernetes.io/instance: cassandra
    app.kubernetes.io/managed-by: Helm
type: Opaque
data:
  cassandra-password: "a0pnbUJTN2V0Yg=="
---
# Source: cassandra/templates/headless-svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: cassandra-headless
  namespace: cassandra
  labels:
    app.kubernetes.io/name: cassandra
    helm.sh/chart: cassandra-8.0.2
    app.kubernetes.io/instance: cassandra
    app.kubernetes.io/managed-by: Helm
spec:
  clusterIP: None
  publishNotReadyAddresses: true
  ports:
    - name: intra
      port: 7000
      targetPort: intra
    - name: tls
      port: 7001
      targetPort: tls
    - name: jmx
      port: 7199
      targetPort: jmx
    - name: cql
      port: 9042
      targetPort: cql
  selector:
    app.kubernetes.io/name: cassandra
    app.kubernetes.io/instance: cassandra
---
# Source: cassandra/templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: cassandra
  namespace: cassandra
  labels:
    app.kubernetes.io/name: cassandra
    helm.sh/chart: cassandra-8.0.2
    app.kubernetes.io/instance: cassandra
    app.kubernetes.io/managed-by: Helm
spec:
  type: ClusterIP
  ports:
    - name: cql
      port: 9042
      targetPort: cql
      nodePort: null
    - name: metrics
      port: 8080
      nodePort: null
  selector:
    app.kubernetes.io/name: cassandra
    app.kubernetes.io/instance: cassandra
---
# Source: cassandra/templates/statefulset.yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: cassandra
  namespace: cassandra
  labels:
    app.kubernetes.io/name: cassandra
    helm.sh/chart: cassandra-8.0.2
    app.kubernetes.io/instance: cassandra
    app.kubernetes.io/managed-by: Helm
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: cassandra
      app.kubernetes.io/instance: cassandra
  serviceName: cassandra-headless
  podManagementPolicy: OrderedReady
  replicas: 3
  updateStrategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app.kubernetes.io/name: cassandra
        helm.sh/chart: cassandra-8.0.2
        app.kubernetes.io/instance: cassandra
        app.kubernetes.io/managed-by: Helm
    spec:
      
      serviceAccountName: cassandra
      affinity:
        podAffinity:
          
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - podAffinityTerm:
                labelSelector:
                  matchLabels:
                    app.kubernetes.io/name: cassandra
                    app.kubernetes.io/instance: cassandra
                namespaces:
                  - "cassandra"
                topologyKey: kubernetes.io/hostname
              weight: 1
        nodeAffinity:
          
      securityContext:
        fsGroup: 1001
      containers:
        - name: cassandra
          command:
            - bash
            - -ec
            - |
              # Node 0 is the password seeder
              if [[ $POD_NAME =~ (.*)-0$ ]]; then
                  echo "Setting node as password seeder"
                  export CASSANDRA_PASSWORD_SEEDER=yes
              else
                  # Only node 0 will execute the startup initdb scripts
                  export CASSANDRA_IGNORE_INITDB_SCRIPTS=1
              fi
              /opt/bitnami/scripts/cassandra/entrypoint.sh /opt/bitnami/scripts/cassandra/run.sh
          image: docker.io/bitnami/cassandra:4.0.3-debian-10-r59
          imagePullPolicy: "IfNotPresent"
          securityContext:
            runAsNonRoot: true
            runAsUser: 1001
          env:
            - name: BITNAMI_DEBUG
              value: "false"
            - name: CASSANDRA_CLUSTER_NAME
              value: cassandra
            - name: CASSANDRA_SEEDS
              value: "cassandra-0.cassandra-headless.cassandra.svc.cluster.local,cassandra-1.cassandra-headless.cassandra.svc.cluster.local,cassandra-2.cassandra-headless.cassandra.svc.cluster.local"
            - name: CASSANDRA_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: cassandra
                  key: cassandra-password
            - name: POD_IP
              valueFrom:
                fieldRef:
                  fieldPath: status.podIP
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: CASSANDRA_USER
              value: "cassandra"
            - name: CASSANDRA_NUM_TOKENS
              value: "256"
            - name: CASSANDRA_DATACENTER
              value: dc1
            - name: CASSANDRA_ENDPOINT_SNITCH
              value: SimpleSnitch
            - name: CASSANDRA_KEYSTORE_LOCATION
              value: "/opt/bitnami/cassandra/certs/keystore"
            - name: CASSANDRA_TRUSTSTORE_LOCATION
              value: "/opt/bitnami/cassandra/certs/truststore"
            - name: CASSANDRA_RACK
              value: rack1
            - name: CASSANDRA_TRANSPORT_PORT_NUMBER
              value: "7000"
            - name: CASSANDRA_JMX_PORT_NUMBER
              value: "7199"
            - name: CASSANDRA_CQL_PORT_NUMBER
              value: "9042"
          envFrom:
          livenessProbe:
            exec:
              command:
                - /bin/bash
                - -ec
                - |
                  nodetool status
            initialDelaySeconds: 60
            periodSeconds: 30
            timeoutSeconds: 5
            successThreshold: 1
            failureThreshold: 5
          readinessProbe:
            exec:
              command:
                - /bin/bash
                - -ec
                - |
                  nodetool status | grep -E "^UN\\s+${POD_IP}"
            initialDelaySeconds: 60
            periodSeconds: 10
            timeoutSeconds: 5
            successThreshold: 1
            failureThreshold: 5
          ports:
            - name: intra
              containerPort: 7000
            - name: tls
              containerPort: 7001
            - name: jmx
              containerPort: 7199
            - name: cql
              containerPort: 9042
          resources: 
            limits:
              cpu: 2Gi
              memory: 1000m
            requests:
              cpu: 100m
              memory: 64Mi
          volumeMounts:
            - name: data
              mountPath: /bitnami/cassandra
            
      volumes:
  volumeClaimTemplates:
    - metadata:
        name: data
        labels:
          app.kubernetes.io/name: cassandra
          app.kubernetes.io/instance: cassandra
      spec:
        accessModes:
          - "ReadWriteOnce"
        resources:
          requests:
            storage: "8Gi"

NOTES:
CHART NAME: cassandra
CHART VERSION: 8.0.2
APP VERSION: 4.0.3** Please be patient while the chart is being deployed **

Cassandra can be accessed through the following URLs from within the cluster:

  - CQL: cassandra.cassandra.svc.cluster.local:9042

To get your password run:

   export CASSANDRA_PASSWORD=$(kubectl get secret --namespace "cassandra" cassandra -o jsonpath="{.data.cassandra-password}" | base64 --decode)

Check the cluster status by running:

   kubectl exec -it --namespace cassandra $(kubectl get pods --namespace cassandra -l app=cassandra,release=cassandra -o jsonpath='{.items[0].metadata.name}') nodetool status

To connect to your Cassandra cluster using CQL:

1. Run a Cassandra pod that you can use as a client:

   kubectl run --namespace cassandra cassandra-client --rm --tty -i --restart='Never' \
   --env CASSANDRA_PASSWORD=$CASSANDRA_PASSWORD \
    \
   --image docker.io/bitnami/cassandra:4.0.3-debian-10-r59 -- bash

2. Connect using the cqlsh client:

   cqlsh -u cassandra -p $CASSANDRA_PASSWORD cassandra

To connect to your database from outside the cluster execute the following commands:

   kubectl port-forward --namespace cassandra svc/cassandra 9042:9042 &
   cqlsh -u cassandra -p $CASSANDRA_PASSWORD 127.0.0.1 9042
```
**export cassandra password with `export CASSANDRA_PASSWORD=$(kubectl get secret --namespace "cassandra" cassandra -o jsonpath="{.data.cassandra-password}" | base64 --decode)`**
**run cassandra client with:**
```
kubectl run --namespace cassandra cassandra-client --rm --tty -i --restart='Never' \
   --env CASSANDRA_PASSWORD=$CASSANDRA_PASSWORD \
    \
   --image docker.io/bitnami/cassandra:4.0.3-debian-10-r59 -- bash
```
**run inside the pod sqlsh client:** `cqlsh -u cassandra -p $CASSANDRA_PASSWORD cassandra`
### OR
**shell into cassandra pod via K9s nad run**  `cqlsh -u cassandra -p $CASSANDRA_PASSWORD cassandra`

**next run the following**:
```
CREATE KEYSPACE store
  WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
```

```
CREATE TABLE shopping_cart (
	... PRIMARY KEY(id),
	... item_count int,
	... last_update_timestamp timestamp)
```
```
INSERT INTO shopping_cart (item_count, last_update_timestamp) values('3', '2022-05-02');
```
**to show the data inseted**
```
select * from shopping_cart
```
