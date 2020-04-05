Kubernetes on AWS using Kops
1. Launch Linux EC2 instance in AWS
2. Create and attach IAM role to EC2 Instance.

          Kops need permissions to access
              S3
              EC2
              VPC
              Route53
              Autoscaling
              etc..
  
3. Install Kops on EC2

        curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
        chmod +x kops-linux-amd64
        sudo mv kops-linux-amd64 /usr/local/bin/kops

4. Install kubectl

        curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
        chmod +x ./kubectl
        sudo mv ./kubectl /usr/local/bin/kubectl

5. Create S3 bucket in AWS

          S3 bucket is used by kubernetes to persist cluster state, lets create s3 bucket using aws cli Note: Make sure you choose bucket name that is uniqe accross all aws accounts

          aws s3 mb s3://kopshome.in.k8s --region ap-south-1
      
6. Create private hosted zone in AWS Route53

          1  Head over to aws Route53 and create hostedzone
          2  Choose name for example (kopshome.in)
          3  Choose type as privated hosted zone for VPC
          4 Select default vpc in the region you are setting up your cluster
          5  Hit create

7 Configure environment variables.
Open .bashrc file

          vi ~/.bashrc
          
Add following content into .bashrc, you can choose any arbitary name for cluster and make sure buck name matches the one you created in previous step.

        export KOPS_CLUSTER_NAME=kopshome.in
        export KOPS_STATE_STORE=s3://kopshome.in.k8s
        
Then running command to reflect variables added to .bashrc

        source ~/.bashrc
        
8. Create ssh key pair
This keypair is used for ssh into kubernetes cluster

        ssh-keygen
        
        kops create secret --name kopshome.in sshpublickey admin -i ~/.ssh/y.pub
        
9. Create a Kubernetes cluster definition.

        kops create cluster \
        --state=${KOPS_STATE_STORE} \
        --node-count=2 \
        --master-size=t2.micro \
        --node-size=t2.micro \
        --zones=ap-south-1a,ap-south-1b \
        --name=${KOPS_CLUSTER_NAME} \
        --dns private \
        --master-count 1

10. Create kubernetes cluster

          kops update cluster --yes
        
Above command may take some time to create the required infrastructure resources on AWS. Execute the validate command to check its status and wait until the cluster becomes ready

          kops validate cluster
          
For the above above command, you might see validation failed error initially when you create cluster and it is expected behaviour, you have to wait for some more time and check again.


11. To connect to the master

        ssh admin@api.javahome.in
        
# Destroy the kubernetes cluster

        kops delete cluster  --yes
        
# Optional (Create terraform scripts through kops)
  https://github.com/kubernetes/kops/blob/master/docs/terraform.md
  
  
