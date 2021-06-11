# Cyral Sidecar AWS module for Terraform

## Usage

```hcl
module "cyral_sidecar" {
    source  = "cyralinc/sidecar-aws/cyral"  
    version = "1.0.0" # terraform module version

    sidecar_version = ""
    sidecar_id      = ""

    name_prefix   = ""
    control_plane = ""

    vpc_id  = ""
    subnets = [""]

    ssh_inbound_cidr         = ["0.0.0.0/0"]
    db_inbound_cidr          = ["0.0.0.0/0"]
    healthcheck_inbound_cidr = ["0.0.0.0/0"]

    container_registry = ""
    client_id          = ""
    client_secret      = ""
}
```

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.12 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 3.22.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 3.22.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_autoscaling_group.cyral-sidecar-asg](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/autoscaling_group) | resource |
| [aws_cloudwatch_log_group.cyral-sidecar-lg](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_log_group) | resource |
| [aws_iam_instance_profile.sidecar_profile](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_instance_profile) | resource |
| [aws_iam_policy.init_script_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_role.sidecar_role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy_attachment.init_script_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.user_policies](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_launch_configuration.cyral-sidecar-lc](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/launch_configuration) | resource |
| [aws_lb.cyral-lb](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lb) | resource |
| [aws_lb_listener.cyral-sidecar-lb-ls](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lb_listener) | resource |
| [aws_lb_target_group.cyral-sidecar-tg](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lb_target_group) | resource |
| [aws_route53_record.cyral-sidecar-dns-record](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route53_record) | resource |
| [aws_secretsmanager_secret.cyral-sidecar-secret](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret) | resource |
| [aws_secretsmanager_secret_version.cyral-sidecar-secret-version](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret_version) | resource |
| [aws_security_group.instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group) | resource |
| [aws_ami.amazon_linux_2](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ami) | data source |
| [aws_availability_zones.all](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/availability_zones) | data source |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [aws_iam_policy_document.init_script_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.sidecar](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_region.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_additional_security_groups"></a> [additional\_security\_groups](#input\_additional\_security\_groups) | Additional security groups to attach to sidecar instances | `list(string)` | `[]` | no |
| <a name="input_ami_id"></a> [ami\_id](#input\_ami\_id) | Amazon Linux 2 AMI ID for sidecar EC2 instances. The default behavior is to use the latest version.<br>In order to define a new image, provide the desired image id. | `string` | `""` | no |
| <a name="input_asg_count"></a> [asg\_count](#input\_asg\_count) | Set to 1 to enable the ASG, 0 to disable. Only for debugging. | `number` | `1` | no |
| <a name="input_asg_desired"></a> [asg\_desired](#input\_asg\_desired) | The desired number of hosts to create in the autoscale group | `number` | `1` | no |
| <a name="input_asg_max"></a> [asg\_max](#input\_asg\_max) | The maximum number of hosts to create in the autoscale group. | `number` | `2` | no |
| <a name="input_asg_min"></a> [asg\_min](#input\_asg\_min) | The minimum number of hosts to create in the autoscale group | `number` | `1` | no |
| <a name="input_associate_public_ip_address"></a> [associate\_public\_ip\_address](#input\_associate\_public\_ip\_address) | Associates a public IP to sidecar EC2 instances | `bool` | `false` | no |
| <a name="input_client_id"></a> [client\_id](#input\_client\_id) | The client id assigned to the sidecar | `string` | n/a | yes |
| <a name="input_client_secret"></a> [client\_secret](#input\_client\_secret) | The client secret assigned to the sidecar | `string` | n/a | yes |
| <a name="input_cloudwatch_logs_retention"></a> [cloudwatch\_logs\_retention](#input\_cloudwatch\_logs\_retention) | Cloudwatch logs retention in days | `number` | `14` | no |
| <a name="input_container_registry"></a> [container\_registry](#input\_container\_registry) | Address of the container registry where Cyral images are stored | `string` | n/a | yes |
| <a name="input_container_registry_key"></a> [container\_registry\_key](#input\_container\_registry\_key) | Key provided by Cyral for authenticating on Cyral's container registry | `string` | `""` | no |
| <a name="input_container_registry_username"></a> [container\_registry\_username](#input\_container\_registry\_username) | Username provided by Cyral for authenticating on Cyral's container registry | `string` | `""` | no |
| <a name="input_control_plane"></a> [control\_plane](#input\_control\_plane) | Address of the control plane - <tenant>.cyral.com | `string` | n/a | yes |
| <a name="input_db_inbound_cidr"></a> [db\_inbound\_cidr](#input\_db\_inbound\_cidr) | Allowed CIDR block for database access to the sidecar. Can't be combined with 'db\_inbound\_security\_group'. | `list(string)` | n/a | yes |
| <a name="input_db_inbound_security_group"></a> [db\_inbound\_security\_group](#input\_db\_inbound\_security\_group) | Pre-existing security group IDs allowed to connect to db in the EC2 host. Can't be combined with 'db\_inbound\_cidr'. | `list(string)` | `[]` | no |
| <a name="input_dd_api_key"></a> [dd\_api\_key](#input\_dd\_api\_key) | API key to connect to DataDog | `string` | `""` | no |
| <a name="input_deploy_secrets"></a> [deploy\_secrets](#input\_deploy\_secrets) | Create the AWS Secrets Manager resource at secret\_location using client\_id, client\_secret and container\_registry\_key | `bool` | `true` | no |
| <a name="input_elk_address"></a> [elk\_address](#input\_elk\_address) | Address to ship logs to ELK | `string` | `""` | no |
| <a name="input_elk_password"></a> [elk\_password](#input\_elk\_password) | (Optional) Password to use to ship logs to ELK | `string` | `""` | no |
| <a name="input_elk_username"></a> [elk\_username](#input\_elk\_username) | (Optional) Username to use to ship logs to ELK | `string` | `""` | no |
| <a name="input_healthcheck_inbound_cidr"></a> [healthcheck\_inbound\_cidr](#input\_healthcheck\_inbound\_cidr) | Allowed CIDR block for health check requests to the sidecar | `list(string)` | n/a | yes |
| <a name="input_healthcheck_port"></a> [healthcheck\_port](#input\_healthcheck\_port) | Port used for the healthcheck | `number` | `8888` | no |
| <a name="input_iam_policies"></a> [iam\_policies](#input\_iam\_policies) | (Optional) List of IAM policies ARNs that will be attached to the sidecar IAM role | `list(string)` | `[]` | no |
| <a name="input_idp_certificate"></a> [idp\_certificate](#input\_idp\_certificate) | (Optional) The certificate used to verify SAML assertions from the IdP being used with Snowflake. Enter this value as a one-line string with literal <br> characters specifying the line breaks. | `string` | `""` | no |
| <a name="input_idp_sso_login_url"></a> [idp\_sso\_login\_url](#input\_idp\_sso\_login\_url) | (Optional) The IdP SSO URL for the IdP being used with Snowflake. | `string` | `""` | no |
| <a name="input_instance_type"></a> [instance\_type](#input\_instance\_type) | Amazon EC2 instance type for the sidecar instances | `string` | `"t3.medium"` | no |
| <a name="input_key_name"></a> [key\_name](#input\_key\_name) | AWS key name | `string` | `""` | no |
| <a name="input_load_balancer_certificate_arn"></a> [load\_balancer\_certificate\_arn](#input\_load\_balancer\_certificate\_arn) | (Optional) ARN of SSL certificate that will be used for client connections to Snowflake. | `string` | `""` | no |
| <a name="input_load_balancer_scheme"></a> [load\_balancer\_scheme](#input\_load\_balancer\_scheme) | EC2 network load balancer scheme ('internal' or 'internet-facing') | `string` | `"internal"` | no |
| <a name="input_load_balancer_subnets"></a> [load\_balancer\_subnets](#input\_load\_balancer\_subnets) | Subnets to add load balancer to. If not provided, the load balancer will assume the subnets specified in the `subnets` parameter. | `list(string)` | `[]` | no |
| <a name="input_log_integration"></a> [log\_integration](#input\_log\_integration) | Logs destination | `string` | `"cloudwatch"` | no |
| <a name="input_metrics_integration"></a> [metrics\_integration](#input\_metrics\_integration) | Metrics destination | `string` | `""` | no |
| <a name="input_name_prefix"></a> [name\_prefix](#input\_name\_prefix) | Prefix for names of created resources in AWS | `string` | n/a | yes |
| <a name="input_repositories_supported"></a> [repositories\_supported](#input\_repositories\_supported) | List of all repositories that will be supported by the sidecar (lower case only) | `list(string)` | <pre>[<br>  "dremio",<br>  "mongodb",<br>  "mysql",<br>  "oracle",<br>  "postgresql",<br>  "snowflake",<br>  "sqlserver",<br>  "s3"<br>]</pre> | no |
| <a name="input_secrets_location"></a> [secrets\_location](#input\_secrets\_location) | Location in AWS Secrets Manager to store client\_id, client\_secret and container\_registry\_key | `string` | n/a | yes |
| <a name="input_sidecar_dns_hosted_zone_id"></a> [sidecar\_dns\_hosted\_zone\_id](#input\_sidecar\_dns\_hosted\_zone\_id) | (Optional) Route53 hosted zone ID for the corresponding 'sidecar\_dns\_name' provided | `string` | `""` | no |
| <a name="input_sidecar_dns_name"></a> [sidecar\_dns\_name](#input\_sidecar\_dns\_name) | (Optional) Fully qualified domain name that will be automatically created/updated to reference the sidecar LB | `string` | `""` | no |
| <a name="input_sidecar_dns_overwrite"></a> [sidecar\_dns\_overwrite](#input\_sidecar\_dns\_overwrite) | (Optional) Update an existing DNS name informed in 'sidecar\_dns\_name' variable | `bool` | `false` | no |
| <a name="input_sidecar_dremio_ports"></a> [sidecar\_dremio\_ports](#input\_sidecar\_dremio\_ports) | List of ports allowed to connect to Dremio in sidecar | `list(number)` | <pre>[<br>  31010,<br>  31011,<br>  31012,<br>  31013,<br>  31014<br>]</pre> | no |
| <a name="input_sidecar_id"></a> [sidecar\_id](#input\_sidecar\_id) | Sidecar identifier | `string` | n/a | yes |
| <a name="input_sidecar_mongodb_ports"></a> [sidecar\_mongodb\_ports](#input\_sidecar\_mongodb\_ports) | List of ports allowed to connect to MongoDB in sidecar | `list(number)` | <pre>[<br>  27017,<br>  27018,<br>  27019,<br>  27020,<br>  27021,<br>  27022,<br>  27023,<br>  27024,<br>  27025,<br>  27026,<br>  27027,<br>  27028<br>]</pre> | no |
| <a name="input_sidecar_mysql_ports"></a> [sidecar\_mysql\_ports](#input\_sidecar\_mysql\_ports) | List of ports allowed to connect to MySQL in sidecar | `list(number)` | <pre>[<br>  3306,<br>  3307,<br>  3308,<br>  3309,<br>  3310<br>]</pre> | no |
| <a name="input_sidecar_oracle_ports"></a> [sidecar\_oracle\_ports](#input\_sidecar\_oracle\_ports) | List of ports allowed to connect to OracleDB in sidecar | `list(number)` | <pre>[<br>  1521,<br>  1522,<br>  1523,<br>  1524,<br>  1525<br>]</pre> | no |
| <a name="input_sidecar_postgresql_ports"></a> [sidecar\_postgresql\_ports](#input\_sidecar\_postgresql\_ports) | List of ports allowed to connect to PostgreSQL in sidecar | `list(number)` | <pre>[<br>  5432,<br>  5433,<br>  5434,<br>  5435,<br>  5436<br>]</pre> | no |
| <a name="input_sidecar_s3_ports"></a> [sidecar\_s3\_ports](#input\_sidecar\_s3\_ports) | List of ports allowed to connect to S3 in sidecar | `list(number)` | <pre>[<br>  453<br>]</pre> | no |
| <a name="input_sidecar_snowflake_ports"></a> [sidecar\_snowflake\_ports](#input\_sidecar\_snowflake\_ports) | List of ports allowed to connect to Snowflake in sidecar | `list(number)` | <pre>[<br>  443,<br>  444,<br>  445,<br>  446,<br>  447<br>]</pre> | no |
| <a name="input_sidecar_sqlserver_ports"></a> [sidecar\_sqlserver\_ports](#input\_sidecar\_sqlserver\_ports) | List of ports allowed to connect to SQLServer in sidecar | `list(number)` | <pre>[<br>  1433,<br>  1434,<br>  1435,<br>  1436,<br>  1437<br>]</pre> | no |
| <a name="input_sidecar_private_idp_key"></a> [sidecar\_private\_idp\_key](#input\sidecar\_private\_idp\_key) | (Optional) The private key used to sign SAML Assertions generated by the sidecar. Enter this value as a one-line string with literal <br> characters specifying the line breaks. | `string` | `""` | no |
| <a name="input_sidecar_public_idp_certificate"></a> [sidecar\_public\_idp\_certificate](#input\_sidecar\_public\_idp\_certificate) | (Optional) The public certificate used to verify signatures for SAML Assertions generated by the sidecar. Enter this value as a one-line string with literal <br> characters specifying the line breaks. | `string` | `""` | no |
| <a name="input_sidecar_version"></a> [sidecar\_version](#input\_sidecar\_version) | Version of the sidecar | `string` | n/a | yes |
| <a name="input_splunk_host"></a> [splunk\_host](#input\_splunk\_host) | Splunk host | `string` | `""` | no |
| <a name="input_splunk_index"></a> [splunk\_index](#input\_splunk\_index) | Splunk index | `string` | `""` | no |
| <a name="input_splunk_port"></a> [splunk\_port](#input\_splunk\_port) | Splunk port | `number` | `0` | no |
| <a name="input_splunk_tls"></a> [splunk\_tls](#input\_splunk\_tls) | Splunk TLS | `bool` | `false` | no |
| <a name="input_splunk_token"></a> [splunk\_token](#input\_splunk\_token) | Splunk token | `string` | `""` | no |
| <a name="input_ssh_inbound_cidr"></a> [ssh\_inbound\_cidr](#input\_ssh\_inbound\_cidr) | Allowed CIDR block for SSH access to the sidecar. Can't be combined with 'ssh\_inbound\_security\_group'. | `list(string)` | n/a | yes |
| <a name="input_ssh_inbound_security_group"></a> [ssh\_inbound\_security\_group](#input\_ssh\_inbound\_security\_group) | Pre-existing security group IDs allowed to ssh into the EC2 host. Can't be combined with 'ssh\_inbound\_cidr'. | `list(string)` | `[]` | no |
| <a name="input_subnets"></a> [subnets](#input\_subnets) | Subnets to add sidecar to (list of string) | `list(string)` | n/a | yes |
| <a name="input_sumologic_host"></a> [sumologic\_host](#input\_sumologic\_host) | Sumologic host | `string` | `""` | no |
| <a name="input_sumologic_uri"></a> [sumologic\_uri](#input\_sumologic\_uri) | Sumologic uri | `string` | `""` | no |
| <a name="input_volume_size"></a> [volume\_size](#input\_volume\_size) | Size of the sidecar disk | `number` | `30` | no |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | AWS VPC ID to deploy sidecar to | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_aws_iam_role_arn"></a> [aws\_iam\_role\_arn](#output\_aws\_iam\_role\_arn) | Sidecar IAM role ARN |
| <a name="output_aws_security_group_id"></a> [aws\_security\_group\_id](#output\_aws\_security\_group\_id) | Sidecar security group id |
| <a name="output_sidecar_dns"></a> [sidecar\_dns](#output\_sidecar\_dns) | Sidecar DNS endpoint |
| <a name="output_sidecar_load_balancer_dns"></a> [sidecar\_load\_balancer\_dns](#output\_sidecar\_load\_balancer\_dns) | Sidecar load balancer DNS endpoint |