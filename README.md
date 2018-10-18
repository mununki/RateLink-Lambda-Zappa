# Severless GraphQL API with Django and Graphene

## Setup steps

1. Virtualenv with Python 3.6

   ```
   $ python3.6 -m venv env_my-django-lambda
   $ source env_my-django-lambda/bin/activate
   ```

2. Install requirements.txt

   ```
   $ pip3 install -r requirements.txt
   ```

3. Set up RDS in my_django_proejct/settings.py (using already existing RDS instance)

   > postgreSQL

   ```python
   DATABASES = {
     'default': {
     'ENGINE': 'django.db.backends.postgresql_psycopg2',
     'NAME': 'db_name',
     'USER': 'user_name',
     'PASSWORD': 'rds_master_password',
     'HOST': 'something.rds.amazonaws.com',
     'PORT': '5432',
     }
   }
   ```

4. Valid AWS account and Create IAM user with role for zappa

- Create IAM user for lambda
  - Permissions policies
    - AWSLambdaFullAccess
    - IAMFullAccess
    - AmazonAPIGatewayAdministrator
    - AWSCloudFormationReadOnlyAccess
    - Creating **custom policy** for only zappa
    ```
    {
      "Version": "2018-10-18",
      "Statement": [
        {
          "Sid": "my_project_zappa_1",
          "Effect": "Allow",
          "Action": [
            "cloudformation:CreateStack",
            "cloudformation:CreateChangeSet",
            "cloudformation:ListStacks",
            "cloudformation:UpdateStack",
            "cloudformation:DescribeStacks",
            "cloudformation:DescribeStackResource",
            "cloudformation:DescribeStackEvents",
            "cloudformation:ValidateTemplate",
            "cloudformation:DescribeChangeSet",
            "cloudformation:ExecuteChangeSet"
          ],
          "Resource": [
            "*"
          ]
        }
      ]
    }
    ```

5. Initializing Zappa

   ```
   $ zappa init
   ```

6. Edit zappa_settings.json

   - add VPC config with subnet1, subnet2, securityGroupId of AWS

7. Deployment

   ```
   $zappa deploy dev
   ```
