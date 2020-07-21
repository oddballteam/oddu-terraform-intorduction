%title: OddU - Terraform
%author: Jeremy Britt
%date: 2020-07-21

-> Terraform - An Overview <-
==========

------------------------------------------------

-> # What is it? <-

Terraform is an Infrastructure as Code (IaC) tool for managing
componenets in a versioned fashion through the use of state
files and incremental execution plans for making changes to
infrastructure.

------------------------------------------------

-> # State / Backend <-

Terraform uses a state file to keep a sort of "manifest" of
the infrastructure components it is currently managing. State
files are stored in what is defined as the backend. State files
can optionally be saved locally, but is recommended to be added
to a _shared_ location such as S3. An example of this could
look like the following:

------------------------------------------------

terraform {
  backend "s3" {
    bucket         = "our-shared-terraform-remote-state"
    key            = "terraform.tfstate.my-unique-identifier"
    dynamodb_table = "our-shared-terraform-lock"
    region         = "us-west-1"
    encrypt        = true
  }
}

------------------------------------------------

-> Terraform Files (*.tf) <-

Terraform files are the configurations used to add and/or
remove infrastructure components. This can be done with one
large main.tf file or broken into component specific files
like ec2.tf, rds.tf, cloudwatch.tf, etc.

------------------------------------------------

-> # Terraform Init <-

"terraform init" is the command used to initialize the directory
containing Terraform files. It will use the configured backend
to locate the state file(s), install provider plugins, etc. If
no existing state file is located during initialization, terraform
init will create one at the configured backend as long as the
user can access it (properly configured IAM permissions when
using AWS).

------------------------------------------------

-> # Terraform Format <-

Terraform uses HashiCorp Configuration Language (HCL) for syntax.
HCL was designed to be machine-friendly, yet still human readable.
Users can typically focus on just quotation mark and curly brace
requirements though as spacing can be handled with a built-in
formatiing tool. Users may run "terraform fmt" within their
terraform directory to automatically adjust spacing (an example
is provided within the repository).

------------------------------------------------

-> # Terraform Plan <-

"terraform plan" is used to show the differences of current
state compared to new changes within the configuration files.
If a user adds a new RDS instance resource, a terraform plan
would show that terraform wants to add the new resource in the
given output. It is highly recommended to always run terraform
plan before attempting an apply in order to catch any discrepancies
that may exist. 

------------------------------------------------

-> # Terraform Apply <-

"terraform apply" is where the final steps take place to
manipulate / change infrastructure components. By default,
Terraform still prompts a yes or no confirmation to accept
the changes being made during the apply. It is important to
double check the changes being applied before entering yes to
finalize the apply.

------------------------------------------------

-> # Terraform Import, State Moves, Modules, etc. <-

Terraform has several additional functions not covered here
that factor heavily in making this tool extremely versatile
and powerful. Although they are not covered within this
presentation, they are worth some further research if you
want to learn more. 
