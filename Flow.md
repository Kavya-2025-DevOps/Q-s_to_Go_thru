# What happens when a user hits the application URL?  

**Visual Diagram**  

User Browser
      │
      ▼
DNS Resolution
      │
      ▼
Load Balancer
      │
      ▼
Ingress Controller
      │
      ▼
Kubernetes Service
      │
      ▼
Pod (Spring Boot)
      │
      ▼
Database

**Explanation:**  

Step 1:  
User enters URL "(https://myapp.company.com)" => Browser sends an HTTP/HTTPS request.  

Step 2:  
DNS Resolution => DNS converts "(myapp.company.com)" to an IP address "(35.201.10.5)".
This IP usually belongs to: Load Balancer, Ingress Controller

Step 3:  
Request Reaches Load Balancer (eg:AWS ELB) -> Load balancer receives: "(GET /users)" -> Its job: Accept traffic, Distribute requests, Forward to Kubernetes cluster.   

Step 4:  
Ingress Controller Receives Request -> (eg:NGINX Ingress Controller)  
Ingress rules may look like: "(host: myapp.company.com)"  
Ingress decides: myapp.company.com -> springboot-service 'this is the service name'

Step 5:   
Service Receives Request  
Eg: kind: Service ; name: springboot-service -> Service acts as a stable endpoint -> It has POD1,2,3 -> through label selectors.  

Step 6:  
Service Chooses a Pod -> (POD1,2,3) are running -> Service performs load balancing -> Request may go to: (POD2)  

Step 7:  
kube-proxy Routes Traffic -> On the node, kube-proxy manages networking rules
It forwards traffic from: Service IP -> Pod IP ; 'This is transparent to the user'.

Step 8:  
Request Reaches Pod :  Container -> Spring Boot App (as defined by the yaml -> application listen on 8080

Step 9:   
Application may: Validate request, Call service layer, Query database, Build response

Step 10:  
Database Call (Optional) if requested in the call -> Application talks to: DB -> then Database returns data.

Step 11:   
Response Generated and Travels Back

