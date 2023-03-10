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
 - 
 - 
#Note IP of the Instance from the Output:XX.XX.XX.XX 
 

#Copy the follwowing files into the tmp folder using SCP

 - init_kind.sh                                                                                                                                                                                                                                                               100%  558   203.7KB/s   00:00
 - kind.yaml  
 - mysql-deployments.yaml                                                                                                                                                                            100%  428   183.0KB/s   00:00
 - mysql-pods.yaml
 - mysql-service.yaml                                                                                                                                                                                     100%  270   140.3KB/s   00:00
 - mysql-replicaset.yaml  
 - web-application-deployments.yaml                                                                                                                                                                  100%  652   301.7KB/s   00:00
 - web-application-pods.yaml                                                                                                                                                                         100%  420   172.2KB/s   00:00
 - web-application-replicaset.yaml                                                                                                                                                                   100%  657   267.1KB/s   00:00
 - web-application-service.yaml    


#SSH into the EC2 instance and change directory (cd /tmp)

  - ssh -i week5 XX.XX.XX.XX


#Change Permission to run the script

  - chmod 777 init_kind.sh
 

#Create the Kind Cluster

- ./init_kind.sh


## Kubernets Pods

`cd k8s`

Create Namespace
$ kubectl create ns assignment2


# deploy mysql pod 
- kubectl apply -f mysql-pods.yaml -n assignment2
- 

# deploy the web application pod
- k apply -f web-application-pods.yaml -n assignment2
- 

# deploy mysql replicaset
- kubectl apply -f mysql-replicaset.yaml -n assignment2  
- 

# deploy web application replicaset
- kubectl apply -f web-application-replicaset.yaml -n assignment2  
- 

# deploy mysql deployments
- kubectl apply -f mysql-deployments.yaml -n assignment2 


# deploy web application deployments
- kubectl apply -f web-application-deployments.yaml -n assignment2


# deploy mysql Service
- kubectl apply -f mysql-service.yaml -n assignment2  


# deploy web application service 
- kubectl apply -f web-application-service.yaml -n assignment2 
- 

# access the web application
- kubectl port-forward web-application 8080:8080
- 

#Open another terminal
- curl localhost:8080
- 


# to check the logs of the application
- kubectl logs web-application
 

#Access the appliction through browser @MACHINE_IP:30000



