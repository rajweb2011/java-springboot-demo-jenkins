
#### Prerequisite 

Installed:   
Docker 
Jenkins 
maven 
java  


#### Test after jenkins build
http://localhost:8081


#####  Stop Docker Container:
```
docker stop `docker container ls | grep "hello-world-java:*" | awk '{ print $1 }'`
```

