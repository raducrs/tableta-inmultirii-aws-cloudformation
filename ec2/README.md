** Note this ```Cloudformation``` template is provided as reference. Was not tested or used**

Launch an instance in the AWS console using the template details.

## Post Launch steps
Instal using apt/yum:
- Docker

```
sudo yum update -y
sudo yum install docker -y
sudo service docker start
sudo usermod -a -G docker ec2-user
```

- AWS cli (optional instal if not already installed). Prepare the user that has permissions to pull images from AWS ECR (see policies for more)

```
aws configure
```

- Create the ```local.env``` file for enivroment variables
- Pull docker image (optionally using aws cli from AWS ECR)

```
aws ecr get-login-password --region eu-central-1 | sudo docker login --username AWS --password-stdin <user>.dkr.ecr.eu-central-1.amazonaws.com/<repository>
```

- Run docker image using command

```
sudo docker run -dt --env-file=/opt/ti/local.env -p=8080:8080 <user>.dkr.ecr.eu-central-1.amazonaws.com/<repository>:<tag>
```



