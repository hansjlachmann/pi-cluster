# pi-cluster
The RaspberryPi cluster for my Kubernetes Homelab running with k3s

The Kubernetes cluster is always manintained in the .yaml files in this repository, Flux service running on the RaspberryPi is automatically pulling changes from this repository deploying it to the Kubernetes cluster. 

Applications hosted is: 
- Linkding (bookmarks manager)


- Using the Repository structure in the Flux guides:
https://fluxcd.io/flux/guides/repository-structure/
```
├── apps
│   ├── base
│   ├── production 
│   └── staging
├── infrastructure
│   ├── base
│   ├── production 
│   └── staging
└── clusters
    ├── production
    └── staging
```
All changes (deployments) to the kubernetes cluster will be manintained in the yaml files in the /apps directory
