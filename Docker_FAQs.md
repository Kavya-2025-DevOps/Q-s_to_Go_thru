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

**Scenario 5**: Deployment fails due to “port already in use”  
A "port already in use" deployment failure usually means another process, container, pod, or service is already bound to the requested port.  
In Docker, I'd check running containers and host processes using docker ps, lsof, or netstat.  
In Kubernetes, I'd inspect pod events with kubectl describe pod and verify there are no hostPort or NodePort conflicts. The resolution is typically to free the port, use a different port, or redesign the deployment to avoid direct host-port binding.  


**Scenario 6**: You need zero-downtime deployment using Docker.  
For zero-downtime Docker deployments, I would run the new version alongside the current version, verify it passes health checks, then shift traffic using a load balancer or reverse proxy before removing the old version.  
Blue-green deployment is the simplest approach, while rolling updates are preferable for larger systems.  
Health checks, graceful shutdown handling, and backward-compatible database migrations are essential to ensure users never experience failed requests during deployment.  

**Scenario 7**: Docker build is very slow in Jenkins pipeline.  
When Docker builds are slow in Jenkins, I first determine whether the bottleneck is dependency installation, Docker layer invalidation, image pushes, or Jenkins infrastructure.  
I optimize the Dockerfile to maximize cache hits, enable BuildKit, add a .dockerignore, use multi-stage builds, and cache dependencies such as Maven, Gradle, npm, or pip artifacts.  
For ephemeral agents, I use BuildKit registry-backed caching. I also profile build stages and ensure Jenkins agents have sufficient CPU, memory, and disk performance. This typically reduces build times dramatically.
