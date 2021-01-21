you need somewhere to push a docker image to. this has some info on that:

https://medium.com/swlh/build-deploy-a-rest-api-from-scratch-using-spring-boot-and-aws-ecs-eb369137a020

---

$ ./gradlew build

$ docker build --build-arg DEPENDENCY=build/dependency -t demo .

$ docker run -p 8080:8080 demo:latest

---

https://us-east-1.console.aws.amazon.com/ecr/repositories/public/463796021282/demo?region=us-east-1

view push commands

aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/v9u5b3u6

docker tag demo:latest public.ecr.aws/v9u5b3u6/demo:latest

docker push public.ecr.aws/v9u5b3u6/demo:latest

---

https://us-east-1.console.aws.amazon.com/ecr/repositories/public/463796021282/demo?region=us-east-1

copy URI for docker image you just pushed

---

https://console.aws.amazon.com/ecs/home?region=us-east-1#/firstRun

custom

paste docker image uri where appropriate

hard size limit 2048

port 8080

open up advanced options, one of those sections mentions cpu, set appropriate looking one to 1024

next

update target to resolve error. for some reason 2gb and 1 gb come to mind. 

create, or save, or whatever

---
 
check out your new cluster (https://us-east-1.console.aws.amazon.com/ecs/home?region=us-east-1#/clusters, pick the cluster with the name you just used )

tasks

click the task

copy public IP

paste in new browser tab *** and append ':8080/index' DON'T FORGET THE 8080 it's super easy to miss!

(index cuz that's the string value we passed to @GetMapping in CommentController.java)















https://medium.com/swlh/build-deploy-a-rest-api-from-scratch-using-spring-boot-and-aws-ecs-eb369137a020



https://console.aws.amazon.com/ecs/home?region=us-east-1#/firstRun


https://console.aws.amazon.com/ecr/repositories/public/463796021282/demo?region=us-east-1