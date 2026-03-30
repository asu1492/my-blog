

Context: - Kubernetes Cluster - A single GPU node runs ~200 inference pods (can it?) - Everything healthy. High QPS. GPU fully utilized. 

Now: - containerd suddenly gets SIGKILL’ed. - Nothing crashes. - Traffic continues. - Pods remain Running. - metrics still scraped. - GPU utilization unchanged. - Inference latency looks normal. 

And then: You restart containerd. 

Things start getting weird: - k8s logs work - However, container not found - Deleting pods hangs in Terminating, never gets deleted. - New pods schedule successfully — but fail GPU allocation intermittently. - Old pods still serve traffic and still hold GPU memory. - Only a node reboot clears the state. 

Questions: 1. Why are workloads: - alive at the kernel level - visible to k8s cluster - still consuming GPU - and yet partially invisible to the runtime? 2. And why does GPU scheduling degrade only after containerd restarts?