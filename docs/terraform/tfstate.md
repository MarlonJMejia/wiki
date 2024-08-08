# Backends

## S3 Bucket and DynamoDB must be initialize first

* Create the bucket:

```h title="main.tf"
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
    }
  }
}

provider "aws" {
  region = "us-west-2"
}

resource "aws_s3_bucket" "terraform-state" {
  bucket = "terraform-state-dcube-10104" // Must be unique.
}

resource "aws_s3_bucket_versioning" "terraform-state" {
  bucket = aws_s3_bucket.terraform-state.id
  versioning_configuration {
    status = "Enabled"
  }
}

```

* Creating a DynamoDb table

```h title="main.tf"
resource "aws_dynamodb_table" "state_lock_table" {
  name         = "terraform_state_lock"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"
  attribute {
    name = "LockID"
    type = "S"
  }
}
```

* Adding the backend

??? warning "You must create and initialize the S3 and Dynamodb resources first."

```h title="providers.tf"
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
    }
  }
}

provider "aws" {
  region = "us-east-2"
}

terraform {
  backend "s3" {}
}
```

Create a backend file:

```h
bucket         = "terraform-state-dcube-10104"
region         = "us-east-2"
dynamodb_table = "terraform_state_lock"
encrypt        = true
````

```bash
terraform init -backend-config="key=dev/jenkins-agent.tfstate" \
-backend-config=backend.hcl
```