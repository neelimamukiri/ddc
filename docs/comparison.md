# Comparing the features for various platform stacks

<br>

 |  Stack/Feature  |  UCP  |  OpenShift   |   Kubernetes   |   Kismatic   |  
 |  -------------  |  -------------  | -------------  | -------------  | -------------  | 
 | Installation | Pre-config + ucp join | Ansible based | kubeadm or kargo or bootkube | kismatic install |
 | CLI | docker | oc/kubectl | kubectl |  kubectl | 
 | Registry | Built in - can be deployed as user wants | Bundled; or any registry implementing docker registry APIs including docker hub | --- | No | 
 | Multi-tenancy | Not really, but there are labels  | Projects similar to K8S namespaces | Namespaces |  Namespaces | 
 | Load Balancing | Routing mesh and service VIP to provide east-west and north-south load balancing | Service VIP for east-west |  Routers for north-south( uses Routes to resolve to pod IP) | No router |  | 
 | Source Code Management | --- | CI/CD pipeline | --- | --- | 
 | Scaling | Service Scaling | Pod scaling | Pod Scaling | Pod Scaling | 
 | Updates | Service update | Multiple modes of update - blue/green, A/B, rolling and recreate | Pod level | Pod level  | 
 | Storage | Volume plugins | Same as K8S (PVs and PVCs) | Supported volume plugins | TODO (add list) | 
 | Resource limits | Per Service | Quota/project and Limits/container | Per pod | Per pod | 
 | RBAC | Rudimentary, supported through the docker API proxy. Can be expanded based on labels. | Granular RBAC(regular, system users and service accounts); cluster level and project level authzs supported | Beta RBAC | ? | 
 | Identity Providers  | LDAP |  LDAP, Basic AuthN(server to server), Request Header,  OpenID Connect,  Github, Google | No? | ? | 
 | Content Trust | Built-in support | Currently not supported but its there in k8s roadmap | --- | ? | 
 | Build | docker build | S2I |  docker build |  custom build  | --- |  | 
 | Other features | Supports secrets, compose stacks | Supports other constructs(Template |  Secrets etc.) that are supported by K8S | Templates & Secrets |  | 
 | Networking Stack (default and plugin support) |  |  |  | Callico | 
 | Control Plane update |  |  |  | Yes | 
 | Control Plane Repair(scale) |  |  |  |  | 
 | Image Verification |  |  |  |  | 
 | Runtime Integrity |  |  |  |  | 
 | Scale (pods) |  |  |  |  | 
 | Federation |  |  | Yes |  | 
