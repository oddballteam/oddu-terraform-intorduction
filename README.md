# oddu-terraform-introduction
This repository is meant for a general/basic introduction to Terraform

## Prerequisites

- Included is an mdp specific markdown file (oddu_terraform.md) for the presentation. In order to view this in "presentation" style, you will need the [mdp tool](https://github.com/visit1985/mdp).
  - Once installed, you can simply `mdp oddu_terraform.md` to see the file in presentation mode.
- In order to try out Terraform you will need to [download it](https://www.terraform.io/downloads.html). The demo uses v0.12.24 which can be found in the [GitHub releases](https://github.com/hashicorp/terraform/releases).
- You will need an actual "playground" to actually build any infrastructure. 
- There are some [HashiCorp provided examples](https://github.com/terraform-providers/terraform-provider-aws/tree/master/examples) that provide some good information / outlining of the usage of variables, userdata scripts, outputs, etc. I would strongly encourage glancing through a few of the provided examples (will still require an AWS "playground" with proper permissions in order to actually do any planning/applying). 

## Formatting

- There is a small example file `format_me.tf` included for practice with `terraform fmt`. It has a range of spacing for the configurations, but one can use `terraform fmt` within this directory to adjust the spacing automatically to a cleaner, better format.

## Example Workflow

(Example captured from devops github repository)
- User writes a new .tf file to add a Jenkins target group to existing application load balancer
```
resource "aws_lb_target_group" "jenkins-alb-tg" {
  name        = "dsva-vagov-utility-jenkins-lb-tg"
  port        = 80
  protocol    = "HTTP"
  target_type = "ip"
  vpc_id      = aws_vpc.vpc.id
}

resource "aws_alb_target_group_attachment" "jenkins-vetsgov" {
  target_group_arn  = aws_lb_target_group.jenkins-alb-tg.arn
  target_id         = "172.31.1.100"
  availability_zone = "all"
}

resource "aws_lb_listener_rule" "jenkins-http" {
  listener_arn = module.internal-tools-alb.listener-http-arn
  priority     = 1

  action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.jenkins-alb-tg.arn
  }

  condition {
    host_header {
      values = ["jenkins.vfs.va.gov"]
    }
  }
}

```
- User runs `terraform fmt` in order to catch any spacing errors / cleanup the code
- User runs `terraform init` to initialize the directory, install any specified provider plugins, etc.
- User runs `terraform plan` to see the proposed changes to the infrastructure
```
------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_alb_target_group_attachment.jenkins-vetsgov will be created
  + resource "aws_alb_target_group_attachment" "jenkins-vetsgov" {
      + id               = (known after apply)
      + target_group_arn = (known after apply)
      + target_id        = "172.31.1.100"
    }

  # aws_lb_listener_rule.jenkins-http will be created
  + resource "aws_lb_listener_rule" "jenkins-http" {
      + arn          = (known after apply)
      + id           = (known after apply)
      + listener_arn = "arn:aws-us-gov:elasticloadbalancing:us-gov-west-1:008577686731:listener/app/dsva-vagov-utility-tools/30a7782e04179703/d50903741669dca4"
      + priority     = 1

      + action {
          + order            = (known after apply)
          + target_group_arn = (known after apply)
          + type             = "forward"
        }

      + condition {
          + field  = (known after apply)
          + values = (known after apply)

          + host_header {
              + values = [
                  + "jenkins.vfs.va.gov",
                ]
            }

          + path_pattern {
              + values = (known after apply)
            }
        }
    }

  # aws_lb_target_group.jenkins-alb-tg will be created
  + resource "aws_lb_target_group" "jenkins-alb-tg" {
      + arn                                = (known after apply)
      + arn_suffix                         = (known after apply)
      + deregistration_delay               = 300
      + id                                 = (known after apply)
      + lambda_multi_value_headers_enabled = false
      + name                               = "dsva-vagov-utility-jenkins-lb-tg"
      + port                               = 80
      + protocol                           = "HTTP"
      + proxy_protocol_v2                  = false
      + slow_start                         = 0
      + target_type                        = "ip"
      + vpc_id                             = "vpc-84a444e0"

      + health_check {
          + enabled             = (known after apply)
          + healthy_threshold   = (known after apply)
          + interval            = (known after apply)
          + matcher             = (known after apply)
          + path                = (known after apply)
          + port                = (known after apply)
          + protocol            = (known after apply)
          + timeout             = (known after apply)
          + unhealthy_threshold = (known after apply)
        }

      + stickiness {
          + cookie_duration = (known after apply)
          + enabled         = (known after apply)
          + type            = (known after apply)
        }
    }

Plan: 3 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------
```
- User reviews intended changes and ensures that there are no surprises / discrepancies for the apply step
- If all looks good from the plan, user runs `terraform apply`
- This will again output the intended changes and prompt the user to type "yes" or "no" to accept/decline the changes
- If user enters "yes" at the prompt, Terraform will then apply all proposed changes
