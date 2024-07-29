# Configurations

## Load Order

- Terraform loads all configs (`.tf` or `.tf.json` files) in the directory, using alphabetical order.
- Override files are loaded after non-override files, using alpabetical order.

## Overrides

- Need to have the key work `override` in the name of the `.tf` file.
- Example below.

 Having a `config.tf` file like:

 ```c
 # ...
 resource "aws_instance" "web_server" {
   ami           = "ami-aaa"
   instance_type = "t2.medium"
 # ...
 ```

 And an `override.tf` file like:

 ```c
 resource "aws_instance" "web_server" {
   ami           = "ami-bbb"
   instance_type = "t2.micro"
 }
 ```

 The output will be:

 ```c
 ami           = "ami-bbb"
 instance_type = "t2.micro"
   ```

## Output

- Help us to extract info.

```c
# output definitions
/* syntax:
output NAME {
  value = VALUE
}
*/
output "aws_instance_public_dns" {
  value = "${aws_instance.webserver.public_dns}"
}

/*
parameters that can be used:
o   value (required), the value of the output, could be a string, list, or map. 
    Typically includes an interpolation (static outputs generally aren't that helpful).

o   description (optional), a human-friendly description for the output. This is
    primarily to pass on documentation for those using your Terraform configuration.

o   depends_on (list of strings), these are explicit dependencies that the output has, and these
    dependencies are created before the output value is processed. Dependencies are expresed with 
    the format of TYPE.NAME, for example aws_instance.webserver

o   sensitive (optional), a boolean value indicating sensitive information. When displaying outputs
    in terminal following a terraform apply or refresh operation, sensitive outputs are redacted,
    with <sensitive> displayed instead of the output actual values
*/

### setting the sensitive attribute = true:
output "sensitive_aws_instance_public_dns" {
  sensitive = true
  value = "${aws_instance.webserver.public_dns}"
}

/*
This is going to show in the "terraform apply" command:
sensitive_aws_instance_public_dns = <sensitive>

But if we execute "terraform output" command, we can see the sensitive value:
sensitive_aws_instance_public_dns = aaaaa
*/
```

## Providers

- Providers are executed in the init command, and it downloads the provider plugins.
- You ca have more than one provider from the same type using alias field.
- Plugins are stored in `terraform.d/plugins` , then inside each workspace the plugins are duplicated inside `.terraform/plugins`.

## Modules

- Folder with terraform files with reusable components.
 That folder should contain the following files:
 	- `main.tf` - Can be empty.
 	- `variables.tf` - Defining the params of the module.
 	- `outputs.tf` - Defining the output of the module.
 	- `Readme.mf` - Optional
 	- `LICENSE` - Optional
- Then you can use the module in your main terraform file.

```c
# Define a custom module
module <custom_module_name> {
 source = <folder_path_to_module>

 # Define module variables
 <module_variable_name> = value1
}

# You can see outputs from the module
output <custom_output> {
 value = "${module.<custom_module_name>.<module_output_name>}"
}
```

- Need to use `terraform get` in order to look for modules.
- If you modify any module after the first "get" command, you need to specify to terraform to update the module with `terraform get -update=true`.

## Local values

- Give a name to an expression. Only works inside a module.
Define a local:

```c
locals {
 default_prefix = ${<expression>}
}
```

Use local:

```c
output <what_ever> {
 value = ${"local.default_prefix"}
}
```

## Resource configuration

- Resources are part of the infrastructure.

```c
resource <resource_type> <custom_name> {

}
```

- Combination of `<resource_type>` and `<custom_name>` must be unique.

## Meta-parameters

- Meta-parameters available to all resources:
 	- **count (int)**
   The number of identical resources to create. This doesn't apply to all resources.
    - Note: modules does not currently support count meta-parameter.
    - Example:
 ```c
 variable "instance_ips" {
  default = {
   "0" = "10.1.1.10"
   "1" = "10.1.1.11"
   "2" = "10.1.1.12"
  }
 }
 
 # ...
 
 resource "aws_instance" "webserver" {
  # ...
  count         = "3"
  private_ip    = "${lookup(var.instance_ips, count.index)}"
  # ...
 }
 ```

	- **depends_on (list of strings)**
   Explicit dependencies that the current resource has. These dependencies will be created before
   this resource.
 	- **provider (string)**
   The name of a specific provider to use for the current resource.
 	- **lifecycle (configuration block)**
   Customizes the lifecycle behavior of the current resource.

## Lifecycle block

Using the lifecycle configuration block, you can customize the lifecycle behavior of a resource.
There are specific keys that the lifecycle block exposes for our use:

- **create_before_destroy (bool)**
  When this key is set, it ensures that the replacement of a resource is created before the original
  instance is destroyed.
   - Note: resources that use this create_before_destroy key can only depend on other resources
    that also use the create_before_destroy key.
- **prevent_destroy (bool)**
  Using this flag provides some additional protection against the accidental destruction of a resource.
  When this is set, any plan including a destroy of this resource will return an error.
- **ignore_changes (list of strings)**
  This key allows us to customize how 'diffs' are evaluated for resources. It allows individual
  attributes to be ignored through changes.
   - Note: ignored attribute names can be matched by their name but not by state ID.
    eg: if an aws_route_table has two routes defined and the ignore_changes list contains "route",
    both routes will be ignored. You can also use a single entry with a wildcard (eg: "\*") which
    matches all attribute names, however you cannot use a partial string together with a wildcard
    (eg: "rout\*").

## Timeouts

- By default, each resource has his own timeouts, but we can modify them.

```c
/*
The aws_instance provides these timeouts by default:
 - create (default 10m) for creating instances
 - update (default 10m) for updating instances
 - delete (default 10m) for destroying instances
*/
resource "aws_instance" "webserver" {
 # ...
 timeouts {
  create = "2s"
  update = "20m"
  delete = "1h"
 }
 # ...
}
```

## Data Sources

- Data that needs to be `Fetched` or `Computed` for use in terraform configuration.
- Example

```c
/*
we'll fetch the availability zones mapped to the aws provider.
  - this is a list of zones from aws which we'll use later
  - this data source is of the form: data TYPE NAME where:
    TYPE = "aws_availability_zones"
    NAME = "available"
  - TYPE + NAME combination must be unique
*/
data "aws_availability_zones" "available" {}
```

## Variables

- Few variables that can be defined after apply command.
- Example

```c
variable "aws_access_key" {}
```

After a `terraform apply` command we need to set those variables manually.

```

var.aws_access_key
 Enter a value:_
```

- We can offer a file with variables values:
 	- `terraform.tfvars`
 ```c
 aws_access_key = "aaa"
 ```

	- Execute apply command with var file:
 ```bash
 terraform apply -var-file='../terraform.tfvars'
 ```
