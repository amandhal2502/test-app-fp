### Create an SSH key pair
```ssh-keygen -t rsa```

Press enter thrice after running this command


### Clone this Repo to deploy the required infrastructure using Terraform
```git clone https://github.com/amandhal/ec2_ecr.git```


### Change your current directory to ec2_ecr
```cd ec2_ecr```


### Deploy the required Infrastructure
```terraform init```

```terraform apply --auto-approve```


### Login to EC2 instance
```ssh -i ~/.ssh/id_rsa ec2-user@<public_ip>```

Replace public_ip with the actual public_ip of deployed EC2 instance


### Logout and log back in to make the docker permissions effective
Press ```Ctrl+D``` to logout and then run ```ssh -i ~/.ssh/id_rsa ec2-user@<public_ip>```


### Create a custom docker network of type bridge
```docker network create assignment1```


### Login to Amazon ECR
```aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.us-east-1.amazonaws.com```


### Trigger workflow/github actions by pushing any change to the main branch


### Create database container
```docker run -d --name dbcontainer --net assignment1 -e MYSQL_ROOT_PASSWORD=pw aws_account_id.dkr.ecr.us-east-1.amazonaws.com/my_db:image_version```


### Get the IP of the database container to use in the next step
```docker inspect <database_container_id>```


### Export required env variables
```
export DBHOST=IP of the database
export DBPORT=3306
export DBUSER=root
export DATABASE=employees
export DBPWD=pw
```


### Create application containers
```docker run -d -p 8081:8080  -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR="blue"  --name blue --net assignment1 aws_account_id.dkr.ecr.us-east-1.amazonaws.com/my_app:<image_version>```


```docker run -d -p 8082:8080  -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR="pink"  --name pink --net assignment1 aws_account_id.dkr.ecr.us-east-1.amazonaws.com/my_app:<image_version>```


```docker run -d -p 8083:8080  -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR="lime"  --name lime --net assignment1 aws_account_id.dkr.ecr.us-east-1.amazonaws.com/my_app:<image_version>```


### Login to a container
```docker exec -it <container_name/id> bash```


### Ping other containers on the same custom network by their names
```ping <container_name>```
