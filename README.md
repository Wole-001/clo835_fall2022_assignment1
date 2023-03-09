         ___        ______     ____ _                 _  ___  
        / \ \      / / ___|   / ___| | ___  _   _  __| |/ _ \ 
       / _ \ \ /\ / /\___ \  | |   | |/ _ \| | | |/ _` | (_) |
      / ___ \ V  V /  ___) | | |___| | (_) | |_| | (_| |\__, |
     /_/   \_\_/\_/  |____/   \____|_|\___/ \__,_|\__,_|  /_/ 
 ----------------------------------------------------------------- 

#Deploy the Terraform Code from Instance (:~/environment/clo835_fall2022_assignment1/terraform_code/dev/instances)
 - Run the following Commands
 - Terraform init
 - Terraform validate
 - Terraform Plan
 - Terraform apply --auto-approve

#SSH int the Machine (Pick the eip Output)
 - ssh -i Assignment-dev ec2-user@X.X.X.X.X 


#steps after ssh into the machine:

#install docker and add ec2user to docker group
- sudo yum install docker -y
- sudo systemctl start docker
- sudo usermod -aG docker $USER
- newgrp docker


#add aws credentials 
- mkdir .aws

#Add the credentials(Access key,Secret Acess Key & Session Token
- vi ~/.aws/credentials 

# To login-Go to ecr app on console, view push commands (Retrieve an authentication token and authenticate your Docker client to your registry)
- aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin xxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com

#docker login to ecr and pull the images (Remember to update Actions secrets and variables on git)

- docker pull 837853736685.dkr.ecr.us-east-1.amazonaws.com/mysql:1.1
- docker pull 837853736685.dkr.ecr.us-east-1.amazonaws.com/app:1.0


#create custom network Bridge
- docker network create  -d bridge --subnet 182.18.0.1/24 --gateway  182.18.0.1 cust-bridge

#run mysql container
- docker run --network cust-bridge -d --name mysql -e MYSQL_ROOT_PASSWORD=pw   837853736685.dkr.ecr.us-east-1.amazonaws.com/mysql:1.1

#export env that will be used from the application containers:
- export DBHOST=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mysql)
- export DBPORT=3306
- export DBUSER=root
- export DATABASE=employees
- export DBPWD=pw
- 

#deploy three containers with different colors
- docker run -d --network cust-bridge --name blue  -p 8081:8080  -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR=blue 837853736685.dkr.ecr.us-east-1.amazonaws.com/app:1.0
- docker run -d --network cust-bridge --name pink  -p 8082:8080  -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR=pink 837853736685.dkr.ecr.us-east-1.amazonaws.com/app:1.0
- docker run -d --network cust-bridge --name lime  -p 8083:8080  -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR=lime 837853736685.dkr.ecr.us-east-1.amazonaws.com/app:1.0

#Then access these applications using the machine ip and ports 8081, 8082, or 8083 
- X.X.X.X:8081 (For blue)
- X.X.X.X:8082 (for pink)
- X.X.X.X:8083 (for Lime)
- 


## Kubernets Pods

`cd k8s`

# deploy mysql pod 
- kubectl apply -f pods/mysql.yaml

# deploy mysql service
- kubectl apply -f mysql-service/mysql-service.yaml

# deploy the web application
- kubectl apply -f pods/web-application.yaml
 
# access the web application
- kubectl port-forward web-application 8080:8080
- curl localhost:8080

# to check the logs of the application
- kubectl logs web-application
 

## Kubernets Replicaset

# deploy mysql replicaset
- kubectl apply -f replicasets/mysql.yaml

# deploy web application replicaset
- kubectl apply -f replicasets/web-application.yaml

## kubernetes deployments

# deploy mysql deployments
- kubectl apply -f deployemnts/mysql.yaml

# deploy web application deployments
- kubectl apply -f deployemnts/web-application.yaml

# deploy web application service 
- kubectl apply -f deployemnts/application-service.yaml
 
and then access the appliction through MACHINE_IP:30000



