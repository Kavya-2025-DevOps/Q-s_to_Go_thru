**Frequestly Asked**

Level 1: Direct Questions  

1. Difference b/w Docker Image and Docker Container 
2. Difference between COPY and ADD
3. Difference between CMD and ENTRY POINT
4. Write a Docker File.
5. What is Multi Stage Build and write an Example.
6. Explain Docker Architecture.
7. Docker Best Practices


##
Level 2: Scenario-Based Questions

**Scenario 1**: Your container works locally but fails in Kubernetes. Why?   
A container that works locally but fails in Kubernetes usually depends on assumptions that don't hold in the cluster environment—such as missing environment variables, networking differences, unavailable volumes, resource limits, security restrictions, or startup-order issues.  
I would start by checking pod status, events, logs, environment variables, and resource constraints using kubectl describe and kubectl logs to identify where the runtime environment differs from local Docker execution.  

**Scenario 2**: Docker image size is 1.5GB → reduce to <200MB.  
To reduce a Docker image from 1.5 GB to under 200 MB,  
I would use a multi-stage build, switch to a minimal runtime image such as Alpine or Distroless, remove development dependencies, clean package-manager caches, add a proper .dockerignore, and copy only runtime artifacts into the final image.  
I'd then use docker history or Dive to identify large layers and verify the optimizations.  
These changes typically reduce image size by 80–90% while also improving security and deployment speed.  


**Scenario 3**: Container crashes randomly in production. No logs.  
If a container crashes randomly with no logs, I first check the container exit code and Kubernetes events.  
The most common causes are OOM kills, failed liveness probes, resource limits, node pressure, or segmentation faults.  
I would inspect "kubectl describe pod", review previous logs with "kubectl logs --previous", check resource consumption using "kubectl top", and examine cluster events.  
If the issue remains unclear, I'd add metrics, crash diagnostics, and memory profiling to capture the state immediately before the crash.  

**Scenario 4**: High CPU usage in container but host is fine.  
If a container has high CPU usage while the host is healthy, I would first determine whether the CPU is being used productively or wasted in a busy loop. I'd check container metrics, inspect processes and threads, review CPU limits and throttling, examine recent deployments, and profile the application. Common causes include infinite retry loops, inefficient code paths, excessive garbage collection, heavy logging, or CPU-intensive workloads. Profiling and runtime metrics are usually the fastest way to identify the root cause.  

Scenario 5: Deployment fails due to “port already in use”  
Scenario 6: You need zero-downtime deployment using Docker.  
Scenario 7: Docker build is very slow in Jenkins pipeline. 
