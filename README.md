# CI/CD
## programs are developed by using docker.
* Database: Implemented from the database image (mysql, mongodb, postgress, etc.) in the hub.docker.com.
* Backend: Developed with nestjs, typeOrm (when developing RDBMS).
* Frontend: Development with Angular. 

-----
## CI
### Backend Development Method:
1. Only database is implemented with docker-compose.
2. When developing a backend program, it is developed in an environment that does not use docker.
3. During testing, data is transmitted and received from the database operating in the docker-compose method.

image 1.
![](images/ci-cd1.png)

-----

### Frontend  Development Method:
1. It is implemented with docker-compose by bundling the database and backend at once.
2. Frontend programs are developed in an environment that does not use docker.
3. During the test, the data is transmitted and received with the backend program operated by docker-compose.

image 2.
![](images/ci-cd2.png)

----
## CD

1. Commit and push the frontend and backend programs to github. 
2. By performing Github Acton, and database image, backend image, and frontend image are created by the docker and github action pipeline and pushed to ECR (AWS docker image hub). 
3. backend task (image 3, no. 3)
4. frontend task (image 3, no. 4)
5. When a new image is updated in ECR by detecting ECR, current running application task is not automatically updated actually (to control the update time and apply various variables). Two tasks run concurrently for a certain period of time.
6. If task is updated manually, the original task operates as it is for a certain period of time , and the new task waiting to be updated, and then old task is replaced with the new task smoothly, the current task that is operating does not stop until the new task is replaced completely, so the user can continue work without the task (web program) being stopped.

image 3.
![](images/ci-cd3.png)


-----
## AWS 
### Cluster configuration: 
1. ECS cluster consists of two tasks in one cluster, and each task operates as a web program.. 
   * task 1:  container 1: backend server
              container 2: database
   * task 2:  container 1: frontend 


2. How it works: 
   * Database and backend operate as one web application.
   * Sending and receiving data from frontend web to backend web.

-----
### Considerations when connecting Backend program and Database
1. When developing locally, if the database and backend are implemented with docker-compose, the container name of the database is used as the host name in order for the backend to interact with the database.
2. In AWS, because the containers included in one task are implemented in the same network, the address of the database is designated as localhost, so backend grogram can connect to the database withe it.


