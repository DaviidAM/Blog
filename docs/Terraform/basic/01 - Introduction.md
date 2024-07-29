# Terraform

Terraform is an open-source infrastructure as code (IaC) tool developed by HashiCorp. It enables users to define and provision infrastructure resources in a declarative configuration language. With Terraform, you can describe the desired state of your infrastructure, and the tool will automatically create and manage the necessary resources to achieve that state, whether they are in the cloud, on-premises, or in a hybrid environment. This helps automate the process of infrastructure deployment and management, making it more scalable, repeatable, and efficient.

## Comments

```c
# this is a comment 
### and so is this

/* this is a multi-line comment just like
   in many common programming languages */
```

## Variables

```c
# variables definition
variable "aws_access_key" {}
variable "aws_secret_key" {}
variable "private_key_path" {}
variable "key_name" {default = "ubuntu1"}

# values are assigned using key = value
# strings are in double quotes " "
# strings can interpolate using syntax wrapped in ${ }

# variable types
variable "<var_name>" {
 type = "<var_type>" //it can be: string, list, map
 default = "..." 
 /*
 -> example list
 default = ["val1", "val2"]
 
 -> example map
 default = {
  key1 = "val1"
  key2 = "val2"
 }
 /*
}
```

## Providers

```c
# providers definition
provider "aws" {
  access_key = "${var.aws_access_key}"
  secret_key = "${var.aws_secret_key}"
  region     = "us-west-1"
}
```

## Resource

```c
# resources definition
resource "aws_instance" "ubuntu" {
  ami           = "ami-b2527ad2"
  instance_type = "t2.micro"
  key_name        = "${var.key_name}"
  
  # shell-stype 'here doc' syntax for multiline strings
  user_data = <<-EOF
              #!/bin/bash
              echo "Hello, World!" > index.html
              nohup busybox httpd -f -p 8800 &
              EOF
}
```

## Values types

```c
/* numbers are assumed to be base 10 but if you prefix a number
   with 0x that number is then treated as a hexadecimal value */

# boolean values are either true or false

/* lists of primitive types are possible with square brackets ([])
   for example [false, true, true] */

/* maps can be made with curly braces { } and colons :
   for example {"label1":true, "label2":false} */

```

## Conditionals

```c
# you can also use conditionals to determine a value based on logic
# the syntax uses the ternary operator '?'
# (some condition) ? (true value) : (false value)
resource "aws_instance" "webserver" {
  subnet = "${var.env == "production" ? var.prod_subnet : var.dev_subnet}"
}

# support operators
# for equality you can use == and !=
# numerical comparators include >,  <,  >=,  <=
# booleans &&, ||, unary !

# typical use case
resource "aws_instance" "webserver" {
  count = "${var.evaluation ? 1 : 0}"
}
```
