#### Difference between daemonsets and deployments
Kubernetes ```deployments```manage stateless services running on your cluster (as opposed to for example StatefulSets which do manage stateful services). Their purpose is to keep a set of identical pods running and upgrade them in a controlled way. For example, you define how many replicas(```pods```) you want to run of your app in deployment definition and kubernetes will make that many replicas of your application spread over nodes. If you say 5 replica's over 3 node then some nodes have more than one replica of your app running.
DaemonSets manage groups of replicated Pods. However, DaemonSets attempt to adhere to a one-Pod-per-node model, either across the entire cluster or a subset of nodes. Daemonset will not run more than one replica per node. Another advantage of using Daemonset is, If you add a node to the cluster then Daemonset will automatically spawn pod on that node, which deployment will not do.

``` DaemonSets ``` are useful for deploying ongoing background tasks that you need to run on all or certain nodes, and which do not require user intervention. Examples of such tasks include storage daemons like ``` ceph ```, log collection daemons like ```fluentd ```, and node monitoring daemons like collectd

Lets take example you mentioned in question, why kube-dns is deployment and kube-proxy is daemonset?

The reason behind that is kube-proxy is needed on every node in cluster to run IP tables so that every node can access every pod no matter on which node it resides. Hence, when we make kube-proxy a daemonset and another node is added to cluster at later time ``` kube-proxy ``` is automatically spawned on that node.

Kube-dns responsibility is to discover the service IP using its name and even one replica of ``` kube-dns ``` is enough to resolve the service name to its IP and hence we make ``` kube-dns ``` a deployment because we don't need kube-dns on every node.
