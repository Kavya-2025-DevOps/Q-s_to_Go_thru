
**Frequestly Asked**

1. Difference b/w Docker Image and Docker Container 
2. Difference between COPY and ADD
3. Difference between CMD and ENTRY POINT
4. Write a Docker File.
5. What is Multi Stage Build and write an Example.
6. Explain Docker Architecture.


##
Level 6: Scenario-Based Questions

**Scenario 1**: Your container works locally but fails in Kubernetes. Why?   
A container that works locally but fails in Kubernetes usually depends on assumptions that don't hold in the cluster environment—such as missing environment variables, networking differences, unavailable volumes, resource limits, security restrictions, or startup-order issues.  
I would start by checking pod status, events, logs, environment variables, and resource constraints using kubectl describe and kubectl logs to identify where the runtime environment differs from local Docker execution.  

Scenario 2: Docker image size is 1.5GB → reduce to <200MB.  
Scenario 3: Container crashes randomly in production. No logs.  
Scenario 4: High CPU usage in container but host is fine.  
Scenario 5: Deployment fails due to “port already in use”  
Scenario 6: You need zero-downtime deployment using Docker.  
Scenario 7: Docker build is very slow in Jenkins pipeline. 
