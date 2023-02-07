# assignment1
CLO835 Assignment1

DEPLOY OF AMAZON ECR AND EC2 WITH TF

# Create ssh key pair and Amazon Linux EC2 in a public subnet in default VPC
$ ssh-keygen -t rsa -f assn1-dev
$ alias tf=terraform
$ tf init
$ tf apply --auto-approve

aws ecr create-repository --repository-name clo835-ass1-db --image-scanning-configuration scanOnPush=true

# update securitygroup :
Inbound edit update port range to cater 8080 to 8083

# Create and config a git hub repo
   update the ssh key
   update the secret
   cd terraform_code
   git init
   git config --global user.name "<your username>"
   git config --global user.email "<your email>"
   ssh-keygen -t rsa -f /home/ec2-user/.ssh/id_rsa  
   cat ~/.ssh/id_rsa.pub  <copy to the git repository>
   git clone https://github.com/igeiman13/clo835_fall2022_assignment1
   
BUILD AUTOMATION OF DOCKER IMAGES UPON MERGING TO THE MASTER BRANCH
log in to aws ec2: ssh -i assn1-dev <eip>
# Install Docker 
$ sudo yum update -y
$ sudo yum install docker -y
$ sudo systemctl start docker
$ sudo systemctl status docker

# Add ec2-user to the docker group to enable docker cli use w/o sudo in the next session
ls -ltra /var/run/docker.sock
$ sudo usermod -a -G docker ec2-user

# Log out of EC2  and log in again 

$ docker info
$ docker run -it -p 80:80 hello-world

# List all the running containers
$ docker ps

- sudo yum update -y
- sudo yum install mysql-client -y
- pip3 install -r requirements.txt
- pip install flask
 

Build application docker image

- docker build -t my_db -f Dockerfile_mysql .
- docker build -t my_app -f Dockerfile .
- docker images (db and app shld be existing)
- docker run -d -e MYSQL_ROOT_PASSWORD=pw my_db
- docker ps (mydb shld be running)
- docker inspect <myqsl container id>

PULL APPLICATION AND MY SQL IMAGES

logon : ssh -i assn1-dev ec2-user@3.221.186.88
chmod 400 assn1-dev

# Export environment variable
$ export ECR=943313814439.dkr.ecr.us-east-1.amazonaws.com/clo835-ass1-app

# Log into docker registry to get permissions to pull an image
$ aws ecr get-login-password --region us-east-1 | docker login -u AWS ${ECR} --password-stdin   

Pull application and MySQL images
docker pull <uri my_db> 
docker pull <uri my_app> 


Create new network and MySQL container. Do not map container port to host port. Test MySQL container. (sa MY SQLDB
docker network ls
docker network create  -d bridge --subnet 182.18.0.1/24 --gateway  182.18.0.1 assn1
docker network ls
docker run --network=assn1 -d -e MYSQL_ROOT_PASSWORD=pw 943313814439.dkr.ecr.us-east-1.amazonaws.com/clo835-ass1-db:v0.1
docker images 

* to demostrate the container testing, read me inside the clo835_assignment1 
