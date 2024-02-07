AWS

---
AMI
connect -> ssh client 
and follow instruction in aws

amazon-linux-extras install docker

ECS 

docker build test_app -t .
docker tag test_app zaenalarf/test_app
docker push zaenalarf/test_app

ssh -i "TEST_KEY.pem" ec2-user@ec2-52-64-32-106.ap-southeast-2.compute.amazonaws.com

docker run -d --rm -p 80:80 zaenalarf/test_app

security -> launch wizard -> security group -> edit inbound rules HTTP 

--
automation upoad docker using ECS 

https://www.youtube.com/watch?v=aLJHB2CuqBU&t=213s

https://stackoverflow.com/questions/65727113/aws-ecr-user-is-not-authorized-to-perform-ecr-publicgetauthorizationtoken-on-r

// for run cluster and instance || must with fargate and ec2
https://www.youtube.com/watch?v=PgyFrkJaNys&t=565s



=== kubernetes ====

https://github.com/kubernetes/dashboard

https://github.com/kubernetes/dashboard/releases/tag/v3.0.0-alpha0

-- RBAC
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

kubectl -n kubernetes-dashboard create token admin-user



kubectl expose deployment first-app --type=LoadBalancer --port=8080
kubectl get services

// restarting container
kubectl get pods

// scaling up pod
kubectl scale deployment/first-app --replicas=3
// scale down
kubectl scale deployment/first-app --replicas=1

// update pod
docker build -t zaenalarf/first_app:2 .
docker push zaenalarf/first_app:2
kubectl set image deployment/first-app first-app-kf6k9=zaenalarf/first_app:2 [container in pod/image repository]
kubectl rollout status deployment/first-app 

// undo deployment
kubectl set image deployment/first-app first-app-kf6k9=zaenalarf/first_app:3 [example: image not exist ]
kubectl rollout status deployment/first-app 
kubectl rollout undo deployment/first-app
kubectl get pods 

// undo older deployment
kubectl rollout history deployment/first-app
kubectl rollout undo deployment/first-app --revision
kubectl rollout undo deployment/first-app --to-revision=1

// deploy kube using files
kubectl apply -f=deployment.yaml
kubectl apply -f=services.yaml

// delete kube using filed
kubectl delete -f=deployment.yaml
kubectl delete -f=services.yaml
kubectl delete -f=deployment.yaml -f=service.yaml

// group
kubectl apply -f=deployment.yaml -f=service.yaml
kubectl delete deployments,services -l group=example

// persisten volume
kubectl apply -f=host-pv.yaml 
kubectl apply -f=host-pvc.yaml 
kubectl apply -f=deployment.yaml 
kubectl get pv


IAM AWS EKS
eks - cluster role

https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml

cloud formation
using stack custom

public and private cluster

menu -> security credential -> access key
aws configure
aws eks --region us-east-1 update-kubeconfig --name kub-dep-demo


click menu compute
add node group
choose ec2 service
add roles -> eks worker & cni & ec2 container registry read only
disallow remote nodes


=== MAKE VOLUME IN AWS ===
EC2
-> SECURITY GROUP -> CREATE -> CHOOSE EKSVPC -> inbound from eksvpc

EFS
customize -> next -> change security group eks-efs