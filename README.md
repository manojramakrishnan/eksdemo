# eksdemo
This repository contains an example of microservices developed in springboot and deploy steps in EKS

Prerequisite softwares that need to be installed
Jdk 11
eksctl
kubectl
aws cli
maven
Docker for desktop

Step 1 packaging the services using maven and docker and push to dockerhub

1) Clone the repository https://github.com/manojramakrishnan/eksdemo
2) execute maven command " mvn clean install " by cd to all the directories - user-service, department-service, service-registry, cloud-gateway, cloud-config-server, hystrix-dashboard.
3) execute docker build -t <service-name>:0.0.1 .   by cd to all the directories - user-service, department-service, service-registry, cloud-gateway, cloud-config-server, hystrix-dashboard.
4) execute docker tag <service-name>:0.0.1 <yourdockerhub-username>/<service-name>:0.0.1     by cd to all the directories - user-service, department-service, service-registry, cloud-gateway, cloud-config-server, hystrix-dashboard.
5) push all the services to dockerhub using the command " docker push <youdockerhub-username>/<service-name>:0.0.1  "

Step 2 create the EKS Cluster by issuing the command " eksctl create cluster --name eks-cluster --version 1.30 --nodes=1 --node-type=t2.small --region us-east-1  "

Step 3 update the cluster config in local machine after the cluster is created. the command to update the config is " aws eks --region us-east-1 update-kubeconfig --name eks-cluster "

Step 4 go to the directory "eksdemo" and deploy all the service and deployment components to EKSCluster. The command is  " kubectl apply -f ./ "

Step 5 get the loadbalancer url which is cloud gateway svc. issue the following command " kubectl get svc " and copy the Loadbalancer url of cloud-gateway svc.

Step 6 access the user-service and department-service api methods in postman. the urls are mentioned below:-

http://<Cloud-gateway-loadbalancer-url>/departments/

in the body tab, please add the following json to create department. It will be a POST request

{
    "departmentName": "history",
    "departmentAddress": "trivandrum",
    "departmentCode": "ma-01"
}
it will create a department with department id 1

http://<Cloud-gateway-loadbalancer-url>/users/

in the body tab, please add the following json to create user. It will be a POST request

{
    "firstName": "manoj",
    "lastName": "kumar",
    "email": "manoj.rgv@gmail.com",
    "departmentId": 1

}

to fetch the department using department id, try this url http://<Cloud-gateway-loadbalancer-url>/departments/1
to fetch the user with department, then try this url http://<Cloud-gateway-loadbalancer-url>/users/1

Both the above api methods are GET request.












