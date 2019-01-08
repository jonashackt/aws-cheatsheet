# aws-cheatsheet
If your brain starts to explode while thinking about the whole lot of AWS tools, here's some help

We'll try to shed some light into all the tools by classifying them into some categories (like IaaS, hybrid IaaS/PaaS, PaaS & Configuration Management) and also trying to separate them from each other.


## IaaS - Infrastructure-as-a-Service

#### EC2

Just "usual" VMs

#### Lightsail (EC2 light)

VPS (Virtual Private Server) like DigitalOcean / 1&1 (see https://www.heise.de/select/ix/2017/5/1492861894740647)

Subset of EC2, much simpler (512 MiB Lightsail Instance == t2.nano EC2, see https://stackoverflow.com/a/40932906/4964553)

#### S3 

Databases



## Hybrid IaaS/PaaS

#### Elastic Container Service (ECS)

Based on Docker-Containers - could be a good choice for mature projects & mid-term

Like Beanstalk, but with much more control for scaling, size/number of nodes (see https://stackoverflow.com/a/29586384/4964553), auto-scaling etc.

Could use __EC2 Container Registry (ECR)__ / AWS CLI

CLI example:
```bash
aws ecs describe-clusters
```

#### Elastic Container Service for Kubernetes (EKS)

Like ECS, but Kubernetes based (https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)

EKS manages the Kubernetes management infrastructure for you - distributed to different AWS availability zones (https://www.heise.de/developer/meldung/Amazon-EKS-Elastic-Container-Service-fuer-Kubernetes-jetzt-verfuegbar-4069657.html)

CloudWatch and CloudTrail for logging/monitoring AWS workloads

> Amazon EKS passed the Cloud Native Computing Foundation conformance test to become a certified hosted platform, which means that all the plugins and extensions that work with upstream Kubernetes will work as is in EKS (https://thenewstack.io/how-amazon-eks-brings-best-of-kubernetes-and-amazon-web-services/)



## PaaS - Platform-as-a-Service

#### Elastic Beanstalk

PaaS! More for project kickoffs

AWS Elastic Beanstalk == [Pivotal CloudFoundry](https://pivotal.io/de/platform) == [Red Hat OpenShift Container Platform](https://www.openshift.com/products/container-platform/)
(see https://www.dev-insider.de/grundlagen-und-zweck-von-aws-elastic-beanstalk-a-654399/)

Blue prints for usual apps

based on EC2, Route53 etc.
--> but compared to CloudFoundry et.al. you can access EC2 instances beneath via SSH

> manages all details of capacity provisioning, loadbalancing, autoscaling and monitoring

GUI, CLI, IDE plugins

Could use __EC2 Container Registry (ECR)__ / AWS CLI

Dockerrun.aws.json:
```json
{
  "AWSEBDockerrunVersion": "1",
  "Image": {
    "Name": "janedoe/image",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": "1234"
    }
  ],
  "Volumes": [
    {
      "HostDirectory": "/var/app/mydb",
      "ContainerDirectory": "/etc/mysql"
    }
  ],
  "Logging": "/var/log/nginx"
}
```

CLI example
```bash
eb run
```


### Fargate / ECS Fargate

PaaS for Containers

Managed Service, NO server access + NO responsibility for updating, patching etc., intended not for 24/7 running services 

removes any need of Docker host management (https://www.reddit.com/r/aws/comments/7mjs6x/elastic_beanstalk_vs_ecs_fargate/)

relatively expensive compared to ECS (with Fargate is .25 vCPU and 512 MB memory. The 30 day price for 1 container would be $13.68. A t2.micro offers 1 vCPU and 1GB of ram for $8.35.)



### Docker Datacenter / Docker Cloud

Part of Docker EE

onpremise download "Docker Universal Control Plane (UCP)"

In the Cloud use cloud.docker.com

Connect all Nodes running Docker, Cloud or on-Premise in one Browser-Dashboard (also AWS resources)



## Configuration Management Tools

#### Cloudformation


#### OpsWorks / Chef

AWS managed configuration management service, based on Chef (https://www.dev-insider.de/aws-opsworks-stacks-und-opsworks-for-chef-automate-a-663363/)

2 varieties (https://www.dev-insider.de/aws-opsworks-stacks-und-opsworks-for-chef-automate-a-663363/):

1. AWS OpsWorks for Chef Automate (costs (Enterprise Chef), as like AWS OpsWorks Puppet Enterprise (see https://www.dev-insider.de/chef-server-fuers-konfigurationsmanagement-aufsetzen-a-775500/))
2. AWS OpsWorks Stacks


#### Bootstrapping via CloudInit



#### Basis-KnowHow

###### Build-Time vs. Boot-Time

AMI baseconfig --> bootstrapping / boot up maybe needs a lot of time (Patches usw.) --> user defined AMIs could be a good way

VPCs, Security-Groups, Network-ACLs, Router, LBs ...



# Links

https://hackernoon.com/too-many-choices-how-to-pick-the-right-tool-to-manage-your-docker-clusters-b5b3061b84b7

