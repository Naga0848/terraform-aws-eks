## In this repo, we create the complete INFRA using the terraform and Jenkins files for varios components

    First Install the terraform on the Jenkins agent machine

    Install yum-config-manager to manage your repositories.

        $ sudo yum install -y yum-utils

    Use yum-config-manager to add the official HashiCorp RHEL repository.

        $ sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo

    Install Terraform from the new repository.

        $ sudo yum -y install terraform

## Understanding Upstream and Downstream

    From the below image, we can understand that Security Group is dependent on VPC and Bastion depends on Security Group. And the ALB is dependent on ACM

    ACM, EKS and Bastion are not dependent on each other, so these pipelines can be executed parallelly
![alt text](images/upstream&downstream.png)

![alt text](images/infra.png)