# Docker VS Containerd
## Docker
* At the beginning Kubernetes was build only for docker
* Later Kubernetes introduced Container Runtime Interface (CRI)
	* Any container vendor can integrate with Kubernetes as long they adhere to OCI (Open Container Initiative) 
	* imagespec (define image specification)
	* runtimespec (define container runtime)
* Docker was not following CRI
* Kubernetes keep supporting docker through `dockershim`
* In v1.24 `dockershim` was removed
## Containerd
* Is part of Docker