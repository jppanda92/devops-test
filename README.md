# DevOps Engineer - Technical Test	
We think infrastructure is best represented as code, and provisioning of resources should be automated as much as possible.	

 Your task is to create a CI build pipeline that deploys this web application to a load-balanced	
environment. You are free to complete the test in a local environment (using tools like Vagrant and	
Docker) or use any CI service, provisioning tool and cloud environment you feel comfortable with (we	
recommend creating a free tier account so you don't incur any costs).	

 * Your CI job should:	
  * Run when a feature branch is pushed to Github (you should fork this repository to your Github account). If you are working locally feel free to use some other method for triggering your build.	
  * Deploy to a target environment when the job is successful.	
* The target environment should consist of:	
  * A load-balancer accessible via HTTP on port 80.	
  * Two application servers (this repository) accessible via HTTP on port 3000.	
* The load-balancer should use a round-robin strategy.	
* The application server should return the response "Hi there! I'm being served from {hostname}!".	

 ## Context	
We are testing your ability to implement modern automated infrastructure, as well as general knowledge of system administration. In your solution you should emphasize readability, maintainability and DevOps methodologies.	

 ## Submit your solution	
Create a public Github repository and push your solution in it. Commit often - we would rather see a history of trial and error than a single monolithic push. When you're finished, send us the URL to the repository.	

 ## Running this web application	
 This is a NodeJS application:	This is a NodeJS application:

- `npm test` runs the application tests	- `npm test` runs the application tests
- `npm start` starts the http server



# Solution

 ## Context
 
 * The solution is built using NodeJS, Docker Swarm and Travis CI.
 * The node app was built into a Dockerfile in the directory docker-node
 * The load balancer was built into a Dockerfile in the directory docker-lb
 * 'docker stack deploy' is used to deploy the solution in a Docker Swarm environment with 2 replicas of the app and 1 LB on top

 ## How to deploy? 
 
 * Prerequisites
   * Docker, docker-compose
 * Clone the repo to your local that has Docker CLI installed on. 
 * Connect to a Docker Swarm environment or if you are on your local/Linux, then run 'docker swarm init' to convert the node to a manager.
 * In the git clone directory run 'docker stack deploy --compose-file docker-compose.yml buildit-devops'.
 * Curl '127.0.0.1' to 'GET' the Node JS app. 
 * The container hostnames keep changing as per the round robin method. 

 ## How does it work? 

 * The solution is two layered. The first layer has an nginx server configured as a HTTP loadbalancer.
 * You can check the 'nginx.cong' file at 'docker-lb/nginx.conf'
 * The upstream traffic is routed to 'buildit-devops_node:3000', so the above used 'docker stack' command has to be accurate. 
 * The node app servers in the second layer are exposed at ports '3000', so that the traffic from nginx can be routed to them. 
 * Since in the 'nginx.conf' file there is no loadbalancing method mentioned, the default method is taken as Round Robin.  
