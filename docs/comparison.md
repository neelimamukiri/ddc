# Comparing the features for various platform stacks

<br>

 |  Stack/Feature  |  <a href=https://docs.docker.com/datacenter/install/linux/>UCP</a>  |  <a href=https://github.com/openshift/openshift-ansible>OpenShift</a> <a href=https://install.openshift.com/></a>   |   <a href=https://github.com/kubernetes/kubernetes>Kubernetes</a>    |  <a href=https://github.com/apprenda/kismatic>Kismatic</a>   | <a href=https://www.ubuntu.com/containers/kubernetes>Canonical</a>  | <a href=https://coreos.com/tectonic/>Tectonic</a> | 
 |  -------------  |  -------------  | -------------  | -------------  | -------------  |  -------------  | ------------- |
 | Installation | Pre-config + ucp join | Ansible based | kubeadm or kargo or bootkube | kismatic install | conjure-up/juju charms | Graphical Installer, Terraform (alpha)
  | Provisioning | AWS, Azure | --- | --- | --- | AWS, Rackspace, Google, Joyent, MAAS | AWS, Bare metal, Azure (alpha), OpenStack(pre-alpha) |
 | CLI | docker | oc/kubectl | kubectl |  kubectl; kismatic(for cluster ops) | kubectl | kubectl |
 | Registry | Built in - can be deployed as user wants | Bundled | --- | Private docker registry | --- | Quay |
 | Multi-tenancy | Not really, but there are labels  | Projects similar to K8s namespaces | K8s Namespaces |  K8s Namespaces | K8s Namespaces | K8s Namespaces |
 | Load Balancing | Routing mesh and service VIP to provide east-west and north-south load balancing |  HA Proxy, F5 | ---  | nginx | Kube API LB(Experimental) | Tectonic Ingress(AWS ELB) |
 | Source Code Management | --- | CI/CD pipeline | --- | --- | --- | --- |
 | Scaling | Service Scaling | Pod scaling | Pod Scaling | Pod Scaling |  Pod Scaling | Pod Scaling | 
 | Updates | Service update | Multiple modes of update - blue/green, A/B, rolling and recreate | Pod level | Pod level  |  Pod level  | Pod level  | 
 | Storage | Volume plugins | Same as K8s | Supported volume plugins | Same as K8s, GlusterFS(default) | Same as K8s | Same as K8s |
 | Resource limits | Per Service | Same as K8s | Resource quotas and limits(namespace, pods) | Same as K8s | Same as K8s | Same as K8s | 
 | RBAC | Rudimentary, supported through the docker API proxy. Can be expanded based on labels. | Granular RBAC(regular, system users and service accounts); cluster level and project level authzs supported | Beta RBAC | Same as K8s | Same as K8s | Same as K8s|
 | Identity Providers  | LDAP |  LDAP, Basic AuthN(server to server), Request Header,  OpenID Connect,  Github, Google | Service Accounts & Users | Same as K8s |  Same as K8s | DEX (OIDC based, supports LDAP, SAML2.0, GitHub, Google)|
 | Content Trust | Built-in support | --- | --- | --- |  --- | --- | 
 | Build/Deploy | --- | S2I |  --- |  ---  | --- | --- |
 | Application Installation | compose stacks | Template | Helm Charts | Same as K8s | Same as K8s | Same as k8s |
 | Networking Stack (default and plugin support) | --- | OpenShift SDN | --- | Calico | Flannel |  Flannel |
 | Control Plane update | Yes | Yes |  No| Yes |  Yes | Yes |
 | Control Plane Repair(scale) | Yes | Yes | No | No | Yes|  Yes |
 | Image Verification | Yes | --- | --- | --- | --- | Clair |
 | Runtime Integrity | Yes | --- | --- | --- | --- | --- | 
 | Federation | Yes | Yes | Yes | Yes | Yes | Yes |
 | UI | UCP UI  | OpenShift UI | K8s dashboard | K8s dashboard | K8s dashboard | Tectonic Console | 
 | Windows | --- | Yes | Yes | Yes | --- | --- |
 | Logging | --- | --- | --- | --- | --- | --- |
 | Monitoring | --- | Same as K8s | cAdvisor, Heapster, Prometheus | Same as K8s | Same as K8s | Prometheus |
 | Supported OS | Linux 3.1+ | Windows (Origin), Linux, Mac (Origin) | Windows, Linux | RHEL 7, CentOS 7, Ubuntu 16.04  | | Container Linux |
