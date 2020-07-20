# oddu-terraform-intorduction
This repository is meant for a general/basic introduction to Terraform

## Prerequisites

- Included is an mdp specific markdown file (oddu_terraform.md) for the presentation. In order to view this in "presentation" style, you will need the [mdp tool](https://github.com/visit1985/mdp).
  - Once installed, you can simply `mdp oddu_terraform.md` to see the file in presentation mode.
- In order to try out Terraform you will need to [download it](https://www.terraform.io/downloads.html). The demo uses v0.12.24 which can be found in the [GitHub releases](https://github.com/hashicorp/terraform/releases).
- You will need an actual "playground" to actually build any infrastructure. 
- There are some [HashiCorp provided examples](https://github.com/terraform-providers/terraform-provider-aws/tree/master/examples) that provide some good information / outlining of the usage of variables, userdata scripts, outputs, etc. I would strongly encourage glancing through a few of the provided examples (will still require an AWS "playground" with proper permissions in order to actually do any planning/applying). 

## Formatting

- There is a small example file `format_me.tf` included for practice with `terraform fmt`. It has a range of spacing for the configurations, but one can use `terraform fmt` within this directory to adjust the spacing automatically to a cleaner, better format.
