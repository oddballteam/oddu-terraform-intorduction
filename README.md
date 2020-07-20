# oddu-terraform-intorduction
This repository is meant for a general/basic introduction to Terraform

## Prerequisites

- Included is an mdp specific markdown file (oddu_terraform.md). In order to view this in "presentation" style, you will need the [mdp tool](https://github.com/visit1985/mdp).
  - Once installed, you can simply `mdp oddu_terraform.md` to see the file in presentation mode.
- In order to try out Terraform you will need to [download it](https://www.terraform.io/downloads.html). The demo uses v0.12.24 which can be found in the [GitHub releases](https://github.com/hashicorp/terraform/releases).
- You will need an actual "playground" to actually build any infrastructure, but the [HashiCorp provided examples](https://github.com/terraform-providers/terraform-provider-aws/tree/master/examples) will allow you to `terraform apply` and `terraform destroy` for some exposure to running the commands. These examples provide far better interaction than I could for the purpose of presentation and I strongly encourage going through a few to familiarize with Terraform commands in interested.

## Formatting

- There is a small example file `format_me.tf` included for practice with `terraform fmt`. It has a range of spacing for the configurations, but one can use `terraform fmt` within this directory to adjust the spacing automatically to a cleaner, better format.
