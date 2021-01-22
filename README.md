## every time:

$ ./gradlew build

$ docker build --build-arg DEPENDENCY=build/dependency -t springboot-gradle-docker-aws .

[comment]: <> (Just to make sure it works locally)

$ docker run -p 8080:8080 springboot-gradle-docker-aws:latest

[comment]: <> (in new browser tab, go to localhost:8080/index. Curiously, localhost:8080 also works for me right now, but that was unexpected. Perhaps that's cuz the file name is index, and that's a default or something in some frameworks. Might look into that later.)

[comment]: <> (To stop your local version, once you've checked it works.)

$ Control + c 

$ aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/v9u5b3u6

$ docker tag springboot-gradle-docker-aws:latest public.ecr.aws/v9u5b3u6/springboot-gradle-docker-aws:latest

$ docker push public.ecr.aws/v9u5b3u6/springboot-gradle-docker-aws:latest

https://us-east-1.console.aws.amazon.com/ecr/repositories/public/463796021282/springboot-gradle-docker-aws?region=us-east-1



---

## first time:

you need somewhere to push a docker image to. this has some info on that:

https://medium.com/swlh/build-deploy-a-rest-api-from-scratch-using-spring-boot-and-aws-ecs-eb369137a020

---

$ ./gradlew build

$ docker build --build-arg DEPENDENCY=build/dependency -t springboot-gradle-docker-aws .

[comment]: <> (Just to make sure it works locally)

$ docker run -p 8080:8080 springboot-gradle-docker-aws:latest

[comment]: <> (in new browser tab, go to localhost:8080/index. Curiously, localhost:8080 also works for me right now, but that was unexpected. Perhaps that's cuz the file name is index, and that's a default or something in some frameworks. Might look into that later.)

[comment]: <> (To stop your local version, once you've checked it works.)

$ Control + c


---

[comment]: <> (Create a repository to push your local docker image to so aws knows where to find it when you want to deploy your app.)

https://us-east-1.console.aws.amazon.com/ecr/create-repository?region=us-east-1

[comment]: <> (I made a public one, since I don't feel like any of my info needed to be private.)

Click on your new repository. The returned page should show a button 'view push commands'. Click that.

Use commands 0, 2 and 3 (shown below, in order). I'm not sure command 1 works for us, that's why earlier we used `$ ./gradlew build` and `$ docker build --build-arg DEPENDENCY=build/dependency -t springboot-gradle-docker-aws .`

$ aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/v9u5b3u6

$ docker tag springboot-gradle-docker-aws:latest public.ecr.aws/v9u5b3u6/springboot-gradle-docker-aws:latest

$ docker push public.ecr.aws/v9u5b3u6/springboot-gradle-docker-aws:latest

---

Go to your new repository page. There should be a new (or newly updated) image (not an actual image lol. who decided to call it that? anyway, a new docker image, but (i'm guessing for copyright reasons) they're referring to them as repository images. Basically, a new row in that table called Images.). 

That's the image you just pushed.

Click the `copy URI` button on that row/image/however you want to identify it.

---

[comment]: <> (Get your web application/site to show up as an internet site by deploying the application in your docker image to an aws-determined domain by making a new elastic container service using fargate.)

https://console.aws.amazon.com/ecs/home?region=us-east-1#/firstRun

Find the one that says `custom`, click it and click `configure`.

paste docker image uri where appropriate

We're gonna use the memory and cpu values in aws's tomcat-webserver container definition, on the basis that our application seems more similar to that one than to the other sample container definitions.

Change hard size limit to 2048, 

port 8080

Open up advanced options, one of those sections mentions cpu, set appropriate looking one to 1024, open up advanced container configuration, change cpu units to 1024. (currently under environment, but that could change). Click `update` button.

Edit task definition so memory is set to 2gb and cpu is set to 1vCPU. Save. 

Next, next, next until you can click on the `create` button. do that.

View service.

Tasks.

Click task.

Wait till status is no longer pending (status indicator might be hard to find, but it's there. Also, you might need to refresh the page.).


Copy public IP. paste it in a browser window, + ':8080/index', or + ':8080/whateverendpointyouvedefined'. Or maybe just + ':8080', if you have an index.html and that's how that works. 


---



---

[comment]: <> (Get your web application/site to show up as an internet site by deploying the application in your docker image to an aws-determined domain by making a new elastic container service using fargate.)

https://console.aws.amazon.com/ecs/home?region=us-east-1#/firstRun

Find the one that says `custom`, click it and click `configure`.

paste docker image uri where appropriate

We're gonna use the memory and cpu values in aws's tomcat-webserver container definition, on the basis that our application seems more similar to that one than to the other sample container definitions.

Change hard size limit to 2048,

port 8080

advanced settings, cpu, 1024.

Update.

task memory to 2gb, cpu to 1vcpu.

Save. 

Next, next, next until you can click on the `create` button. do that.

View service.

Tasks.

Click task.

Wait till status is no longer pending (status indicator might be hard to find, but it's there. Also, you might need to refresh the page.).


Copy public IP. paste it in a browser window, + ':8080/index', or + ':8080/whateverendpointyouvedefined'. Or maybe just + ':8080', if you have an index.html and that's how that works.


---




alternate setting up ecs

ecs -> create cluster -> select tempalte containing word 'FARGATE'.

Check yes for VPC and cloudmetrics things

ECS -> Task Definitions -> Create new task definition -> Fargate

task role: none

task execution role: whatever it defaults to

task memory: 2gb

task cpu: 1vCPU

add container definition ->

paste docker image uri where appropriate

We're gonna use the memory and cpu values in aws's tomcat-webserver container definition, on the basis that our application seems more similar to that one than to the other sample container definitions.

Change hard size limit to 2048,

port 8080

Open up advanced options, one of those sections mentions cpu, set appropriate looking one to 1024, open up advanced container configuration, change cpu units to 1024. (currently under environment, but that could change). Click `update` button.

create task

go back to cluster, select yours, go to tasks, add task, create.


---


End Credits:

https://medium.com/swlh/build-deploy-a-rest-api-from-scratch-using-spring-boot-and-aws-ecs-eb369137a020



---
work in progress

https://docs.aws.amazon.com/codebuild/latest/userguide/sample-docker.html
assdf

changed IAM role that seemed related to codebuild so it has some words related to ECR