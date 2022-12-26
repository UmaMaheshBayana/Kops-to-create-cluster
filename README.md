# Kops-to-create-cluster

1) Consider one EC2 Linux micro machine to create and monitor clusters.
2) Install kubelet in the machine
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
````
3) Convert the file to executable formate and change the file destination
```
chmod +x ./kubectl
````
```
sudo mv kubectl /usr/local/bin/
````
4) Now install Kops in the machine
```
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
````
5) Convert the file to executable formate and change the file destination
```
sudo chmod +x kops-linux-amd64
````
```
sudo mv kops-linux-amd64 /usr/local/bin/kops
````
6) Craate S3 bucket to store the configurations, and this can be accessed from anywhere
7) Create IAM role with admin permissions
8) Install aws cli and Configure Access Key and Screat Key in the Kops machine
```
sudo apt-get install awscli
````
```
aws configure
````
9) Give the access key and Screat Key and store the credentials
10) Go to Instance settings and modify the IAM role to give access to S3 bucket.
11) Now go to AWS route 53 hosted zone and create hosted zone with name(kube.sanelahealth.com).
12) Configure ns address to godaddy ns name address.

***Create Kubernetes Cluster***
1) Create cluster
```
kops create cluster --name=kube.sanelahealth.com --state=s3://kopsbucket-testproject --zones=ap-south-1a --node-count=2 --node-size=t3.small --master-size=t3.medium --dns-zone=kube.sanelahealth.com --node-volume-size=8 --master-volume-size=8
````
2) Update cluster to update the configuratios
```
kops update cluster --name kube.sanelahealth.com --state=s3://kopsbucket-testproject --yes --admin     
````
3) Validate the cluster to know the cluster details
```
kops validate cluster --state=s3://kopsbucket-testproject
````
4) Delete the cluster
```
kops delete cluster --name kube.sanelahealth.com --state=s3://kopsbucket-testproject --yes
````
 
