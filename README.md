# CI/CD
## programs are developed by using docker.
* Database: Implemented by reading database image (mysql, mongodb, postgress, etc.) from hub.docker.com.
* Backend: Developed with nestjs, typeOrm (when developing RDBMS).
* Frontend: Development with Angular. 

-----
## CI
### Frontend  Development Method:
1. It is implemented with docker-compose by bundling the database and backend at once.
2. Frontend programs are developed in an environment that does not use docker.
3. During the test, the data is transmitted and received with the backend program operated by docker-compose.
![](images/ci-cd2.png)

----
### Backend Development Method:
1. Only database is implemented with docker-compose.
2. When developing a backend program, it is developed in an environment that does not use docker.
3. During testing, data is transmitted and received from the database operating in the docker-compose method.
![](images/ci-cd1.png)

-----
## CD

1. Commit and push frontend and backend programs to github. 
2. By performing Github Acton, each database, backend, and frontend program is created as a Docker image and pushed to ECR (AWS docker image hub). 
3. When a new image is updated in ECR, the actually running application is not automatically updated (to control the update time and apply various variables). And apply the update task in ECS only to the updated task.
4. If the manually modified task is updated, the program that was previously operated for a certain period of time operates as it is, and the new version waiting to be updated is operated at the same time, and then replaced with the new version. At this time, the situation in which the previously operated Web is not stopped does not occur, so the user can continue to work without the program being stopped.
![](images/ci-cd3.png)

-----
## AWS 
### Cluster configuration: 
1. ECS cluster consists of two tasks in one cluster, and each task operates as a web program.. 
   * task 1:  container 1: backened server
                     constainer 2: database
   * task 2:  cotainer 1: frontend 


2. How it works: 
   * Database and backend operate as one web application.
   * Sending and receiving data from frontend web to backend web.
-----
### Considerations when connecting Backend program and Database
1. When developing locally, if the database and backend are implemented with docker-compose, the container name of the database is used as the host name in order for the backend to interact with the database.
2. In AWS, because the contatiners included in one task are implemented in the same network, the address of the database is designated as localhost when connecting to the database in the backend program.


