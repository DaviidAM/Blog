# Infrastructure

## Implicit dependencies

- Terraform can detect automatically dependencies between resources.
- In the example below, terraform detects that `webserver` resource must be created before `ip` resource.

```c
# resources definition
# instances
resource "aws_instance" "webserver" {
  ami           = "ami-1c1d217c"
  instance_type = "t2.micro"
  key_name      = "${var.key_name}"
}

# elastic IPs
resource "aws_eip" "ip" {
  instance = "${aws_instance.webserver.id}"
}
```

## Explicit dependencies

- Terraform cannot detect internal dependencies between resources. In these cases we can use `depends_on` argument, to define the dependencies between resources.
- In the example below, `mys3bucket` resource will be generated before `webserver` resource.

```c
# resources definition
# instances
resource "aws_instance" "webserver" {
  ami           = "ami-1c1d217c"
  instance_type = "t2.micro"
  key_name      = "${var.key_name}"
  # we can use the depends_on argument to inform Terraform that this 
  # EC2 instance must only be created after the S3 bucket has been created
  depends_on = ["aws_s3_bucket.mys3bucket"]
}

### elastic IPs
resource "aws_eip" "ip" {
  instance = "${aws_instance.webserver.id}"
}

# S3 storage
# this represent the S3 bucket our application expects to use
resource "aws_s3_bucket" "mys3bucket" {
  # notice the seemingly long name for the bucket. This is because S3 bucket names have to
  # be unique across ALL AWS accounts, so this name is used to hopefully avoid any conflicts
  # keep this in mind when you are writing code to follow along with this demo
  bucket = "sd-terr-terraform101-bucket"
  acl    = "private"
}

# output definitions
output "aws_instance_public_dns" {
  value = "${aws_instance.webserver.public_dns}"
}
```

## Nondependent resources

- If there's no dependencies between resources, terraform will create them in parallel.
