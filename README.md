# Ollama-on-Kubernetes-w-Nvidia-GPU

![Ollama](https://github.com/GoingOffRoading/Ollama-on-Kubernetes-w-Nvidia-GPU/blob/main/Ollama.png)

# Abstraction

I'm tinkering with GenAI in my homelab cluster and wasn't finding a lot of examples or documentation related to deploying this stuff to Kubernetes.  I've stood up this repo as a resource incase you find yourself in the same situation, and want to yank some of the learning curve out of GenAI Kubernetes deployments.

# Ollama Docker Resources

* Ollama/Ollama on [Docker Hub](https://hub.docker.com/r/ollama/ollama)
* Ollama/Ollama on [Github](https://github.com/ollama/ollama)

# Deployment Steps

1. Ensure that you have the [Nvidia drivers installed on the client](https://ubuntu.com/server/docs/nvidia-drivers-installation) and [Nivida Kuberentes plugin installed and enabled](https://github.com/NVIDIA/k8s-device-plugin).
2. Download [Ollama.yml](https://github.com/GoingOffRoading/Ollama-on-Kubernetes-w-Nvidia-GPU/blob/main/Ollama.yml) from this repo.
3. Make changes to the deployment... Most importantly the local directory that Ollama will persist it's volume in, and the nodeName.
4. Assuming you have kubectl installed and configured on this machine, run: `kubectl apply -f Ollama.yml`.
5. Get the Ollama pod name from `kubectl get pods`.
6. SSH into the pod via `kubectl exec -t (pod name) -- /bin/sh`.
7. Download and run a model.  As of October, the latest on Ollama is `ollama run llama3.2` but there are certainly [many more models to chose from](https://ollama.com/library).
8. Enjoy!

Also, take note that Ollama is being exposed for API calls at the API of the machine in the nodeName, and port `32125` (assuming you did not change the service to load ballancer, or change the NodePort).

# Common Questions

* Why not resolve any intermiten DNS issues in the cluster instead of calling for `dnsPolicy: "None"`?  Where's the PV/PVC?  Why use `nodeName' instead of setting up affinities?

    All great questions, of which I didn't have time to setup originally.  Please feel free to make your own tweaks, or open up a branch.

* What Nvidia drivers did you install?

    From the link in step 1, I used `sudo ubuntu-drivers install` and let Ubuntu sort it out.  I had a buggy time trying to get the `-server` and `-server -open` drivers to work, but the regular desktop driver worked great.

# Future 

- [ ] Setup a PV/PVC
- [ ] Setup node affinities
- [ ] Change the image back to `olamma/olamma` once image processing is in main
- [ ] Resouce limits?