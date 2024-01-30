# NVIDIA MPS example on Kubernetes

Run the MPS server from `mps.yaml`, then the job from `nbody.yaml`.
Change the `CUDA_MPS_ACTIVE_THREAD_PERCENTAGE` in `nbody.yaml` and see the performance changing.

Tried this on a server with an NVIDIA RTX A4000 and:
* Kubernetes 1.29 (installed via [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/))
* [Docker](https://docs.docker.com/engine/install/#server) 25.0.1
* [cri-dockerd](https://github.com/Mirantis/cri-dockerd) 0.3.9
* [nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-container-toolkit) 1.14.4
* [k8s-device-plugin](https://github.com/NVIDIA/k8s-device-plugin) 0.14.4
* [flannel](https://github.com/flannel-io/flannel) 0.24.2

Contents of `/etc/docker/daemon.json`:
```json
{
  "default-runtime": "nvidia",
  "runtimes": {
      "nvidia": {
          "path": "/usr/bin/nvidia-container-runtime",
          "runtimeArgs": []
      }
  },
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  }
}
```

Need to set the [GPU compute mode](https://docs.nvidia.com/deploy/mps/index.html) to `EXCLUSIVE_PROCESS` with:
```bash
nvidia-smi -c EXCLUSIVE_PROCESS
```
