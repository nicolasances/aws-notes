# Using Boto3 to programmatically access AWS Services

**Boto3** is the SDK to interact with AWS APIs. 

## 1. Credentials
The first thing that needs to be done to enable a piece of code to use Boto3, is to configure credentials (Access Key) to interact with AWS APIs. 

### 1.1. Credentials for Local development
When developing locally, you will need to have credentials to use Boto3. 

These credentials can be created in **IAM**. <br>
To create credentials: 
* Create a **User** in IAM
* Assign the desired **roles** to the User
* Create an Access Key

Store the keys in a file located in ~/.aws/credentials, with the following format: 
```
[default]
aws_access_key_id = <your key id>
aws_secret_access_key = <your key value>
```

When creating a boto3 client, the sdk will look for the credentials in that file. 

To instantiate a boto3 client (code in python): 
```
session = boto3.session.Session()
client = session.client(
    service_name='the service you want to use',
    region_name=region_name, 
)
```

### 1.2. Credentials injection for ECS Services
To use boto3 within a container that runs in a ECS Service (Task), simply assign an IAM Role to the task.

Amazon ECS will then generate temporary credentials for that IAM Role. <br>
Any code that uses an AWS SDK (such as boto3 for Python) knows how to access those credentials via the metadata service.

The result is that your code using boto3 will automatically receive credentials that have the permissions associated with the IAM Role assigned to the task.

Sources: 
* [Stack Overflow question](https://stackoverflow.com/questions/72960237/aws-boto3-botocore-assume-iam-role-in-ecs-task)

## Resources
* [Boto3 Configuration](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html#configuration)
* [Managing credentials in Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html)