# terraform-aws-cloudtrail-s3-bucket [![Latest Release](https://img.shields.io/github/release/cloudposse/terraform-aws-cloudtrail-s3-bucket.svg)](https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket/releases/latest) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)

[![README Header][readme_header_img]][readme_header_link]

[![Cloud Posse][logo]](https://cpco.io/homepage)

<!--




  ** DO NOT EDIT THIS FILE
  **
  ** This file was automatically generated by the `build-harness`.
  ** 1) Make all changes to `README.yaml`
  ** 2) Run `make init` (you only need to do this once)
  ** 3) Run`make readme` to rebuild this file.
  **
  ** (We maintain HUNDREDS of open source projects. This is how we maintain our sanity.)
  **





-->

Terraform module to provision an S3 bucket with built in policy to allow [CloudTrail](https://aws.amazon.com/cloudtrail/) [logs](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-management-and-data-events-with-cloudtrail.html).

This is useful if an organization uses a number of separate AWS accounts to isolate the Audit environment from other environments (production, staging, development).

In this case, you create CloudTrail in the production environment (Production AWS account),
while the S3 bucket to store the CloudTrail logs is created in the Audit AWS account, restricting access to the logs only to the users/groups from the Audit account.


The module supports the following:

1. Forced [server-side encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html) at rest for the S3 bucket
2. S3 bucket [versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html) to easily recover from both unintended user actions and application failures
3. S3 bucket is protected from deletion if it's not empty ([force_destroy](https://www.terraform.io/docs/providers/aws/r/s3_bucket.html#force_destroy) set to `false`)


---

This project is part of our comprehensive ["SweetOps"](https://cpco.io/sweetops) approach towards DevOps.
[<img align="right" title="Share via Email" src="https://docs.cloudposse.com/images/ionicons/ios-email-outline-2.0.1-16x16-999999.svg"/>][share_email]
[<img align="right" title="Share on Google+" src="https://docs.cloudposse.com/images/ionicons/social-googleplus-outline-2.0.1-16x16-999999.svg" />][share_googleplus]
[<img align="right" title="Share on Facebook" src="https://docs.cloudposse.com/images/ionicons/social-facebook-outline-2.0.1-16x16-999999.svg" />][share_facebook]
[<img align="right" title="Share on Reddit" src="https://docs.cloudposse.com/images/ionicons/social-reddit-outline-2.0.1-16x16-999999.svg" />][share_reddit]
[<img align="right" title="Share on LinkedIn" src="https://docs.cloudposse.com/images/ionicons/social-linkedin-outline-2.0.1-16x16-999999.svg" />][share_linkedin]
[<img align="right" title="Share on Twitter" src="https://docs.cloudposse.com/images/ionicons/social-twitter-outline-2.0.1-16x16-999999.svg" />][share_twitter]


[![Terraform Open Source Modules](https://docs.cloudposse.com/images/terraform-open-source-modules.svg)][terraform_modules]



It's 100% Open Source and licensed under the [APACHE2](LICENSE).







We literally have [*hundreds of terraform modules*][terraform_modules] that are Open Source and well-maintained. Check them out!







## Usage


**IMPORTANT:** The `master` branch is used in `source` just as an example. In your code, do not pin to `master` because there may be breaking changes between releases.
Instead pin to the release tag (e.g. `?ref=tags/x.y.z`) of one of our [latest releases](https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket/releases).


```hcl
module "s3_bucket" {
  source    = "git::https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket.git?ref=master"
  namespace = "cp"
  stage     = "prod"
  name      = "cluster"
  region    = "us-east-1"
}
```






<!-- markdownlint-disable -->
## Makefile Targets
```text
Available targets:

  help                                Help screen
  help/all                            Display help for all targets
  help/short                          This help short screen
  lint                                Lint terraform code

```
<!-- markdownlint-restore -->
## Requirements

| Name | Version |
|------|---------|
| terraform | >= 0.12.0 |
| aws | ~> 2.0 |
| local | ~> 1.2 |
| null | ~> 2.0 |

## Providers

| Name | Version |
|------|---------|
| aws | ~> 2.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| abort\_incomplete\_multipart\_upload\_days | Maximum time (in days) that you want to allow multipart uploads to remain in progress | `number` | `5` | no |
| access\_log\_bucket\_name | Name of the S3 bucket where s3 access log will be sent to | `string` | `""` | no |
| acl | The canned ACL to apply. We recommend log-delivery-write for compatibility with AWS services | `string` | `"log-delivery-write"` | no |
| arn\_format | ARN format to be used. May be changed to support deployment in GovCloud/China regions. | `string` | `"arn:aws"` | no |
| attributes | Additional attributes (e.g. `1`) | `list(string)` | `[]` | no |
| block\_public\_acls | Set to `false` to disable the blocking of new public access lists on the bucket | `bool` | `true` | no |
| block\_public\_policy | Set to `false` to disable the blocking of new public policies on the bucket | `bool` | `true` | no |
| delimiter | Delimiter to be used between `namespace`, `environment`, `stage`, `name` and `attributes` | `string` | `"-"` | no |
| enable\_glacier\_transition | Glacier transition might just increase your bill. Set to false to disable lifecycle transitions to AWS Glacier. | `bool` | `false` | no |
| enabled | Set to `false` to prevent the module from creating any resources | `bool` | `true` | no |
| environment | Environment, e.g. 'prod', 'staging', 'dev', 'pre-prod', 'UAT' | `string` | `""` | no |
| expiration\_days | Number of days after which to expunge the objects | `number` | `90` | no |
| force\_destroy | (Optional, Default:false ) A boolean that indicates all objects should be deleted from the bucket so that the bucket can be destroyed without error. These objects are not recoverable | `bool` | `false` | no |
| glacier\_transition\_days | Number of days after which to move the data to the glacier storage tier | `number` | `60` | no |
| ignore\_public\_acls | Set to `false` to disable the ignoring of public access lists on the bucket | `bool` | `true` | no |
| kms\_master\_key\_arn | The AWS KMS master key ARN used for the SSE-KMS encryption. This can only be used when you set the value of sse\_algorithm as aws:kms. The default aws/s3 AWS KMS master key is used if this element is absent while the sse\_algorithm is aws:kms | `string` | `""` | no |
| lifecycle\_prefix | Prefix filter. Used to manage object lifecycle events | `string` | `""` | no |
| lifecycle\_rule\_enabled | Enable lifecycle events on this bucket | `bool` | `true` | no |
| lifecycle\_tags | Tags filter. Used to manage object lifecycle events | `map(string)` | `{}` | no |
| name | Solution name, e.g. 'app' or 'jenkins' | `string` | `""` | no |
| namespace | Namespace, which could be your organization name or abbreviation, e.g. 'eg' or 'cp' | `string` | `""` | no |
| noncurrent\_version\_expiration\_days | Specifies when noncurrent object versions expire | `number` | `90` | no |
| noncurrent\_version\_transition\_days | Specifies when noncurrent object versions transitions | `number` | `30` | no |
| policy | A valid bucket policy JSON document. Note that if the policy document is not specific enough (but still valid), Terraform may view the policy as constantly changing in a terraform plan. In this case, please make sure you use the verbose/specific version of the policy | `string` | `""` | no |
| region | If specified, the AWS region this bucket should reside in. Otherwise, the region used by the callee | `string` | `""` | no |
| restrict\_public\_buckets | Set to `false` to disable the restricting of making the bucket public | `bool` | `true` | no |
| sse\_algorithm | The server-side encryption algorithm to use. Valid values are AES256 and aws:kms | `string` | `"AES256"` | no |
| stage | Stage, e.g. 'prod', 'staging', 'dev', OR 'source', 'build', 'test', 'deploy', 'release' | `string` | `""` | no |
| standard\_transition\_days | Number of days to persist in the standard storage tier before moving to the infrequent access tier | `number` | `30` | no |
| tags | Additional tags (e.g. `map('BusinessUnit','XYZ')` | `map(string)` | `{}` | no |
| versioning\_enabled | A state of versioning. Versioning is a means of keeping multiple variants of an object in the same bucket | `bool` | `false` | no |

## Outputs

| Name | Description |
|------|-------------|
| bucket\_arn | Bucket ARN |
| bucket\_domain\_name | FQDN of bucket |
| bucket\_id | Bucket ID |
| prefix | Prefix configured for lifecycle rules |




## Share the Love

Like this project? Please give it a ★ on [our GitHub](https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket)! (it helps us **a lot**)

Are you using this project or any of our other projects? Consider [leaving a testimonial][testimonial]. =)


## Related Projects

Check out these related projects.

- [terraform-aws-cloudtrail](https://github.com/cloudposse/terraform-aws-cloudtrail) - Terraform module to provision an AWS CloudTrail and an encrypted S3 bucket with versioning to store CloudTrail logs
- [terraform-aws-cloudtrail-cloudwatch-alarms](https://github.com/cloudposse/terraform-aws-cloudtrail-cloudwatch-alarms) - Terraform module for creating alarms for tracking important changes and occurances from cloudtrail.
- [terraform-aws-s3-log-storage](https://github.com/cloudposse/terraform-aws-s3-log-storage) - This module creates an S3 bucket suitable for receiving logs from other AWS services such as S3, CloudFront, and CloudTrail
- [terraform-aws-cloudtrail-s3-bucket](https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket) - S3 bucket with built in IAM policy to allow CloudTrail logs



## Help

**Got a question?** We got answers.

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket/issues), send us an [email][email] or join our [Slack Community][slack].

[![README Commercial Support][readme_commercial_support_img]][readme_commercial_support_link]

## DevOps Accelerator for Startups


We are a [**DevOps Accelerator**][commercial_support]. We'll help you build your cloud infrastructure from the ground up so you can own it. Then we'll show you how to operate it and stick around for as long as you need us.

[![Learn More](https://img.shields.io/badge/learn%20more-success.svg?style=for-the-badge)][commercial_support]

Work directly with our team of DevOps experts via email, slack, and video conferencing.

We deliver 10x the value for a fraction of the cost of a full-time engineer. Our track record is not even funny. If you want things done right and you need it done FAST, then we're your best bet.

- **Reference Architecture.** You'll get everything you need from the ground up built using 100% infrastructure as code.
- **Release Engineering.** You'll have end-to-end CI/CD with unlimited staging environments.
- **Site Reliability Engineering.** You'll have total visibility into your apps and microservices.
- **Security Baseline.** You'll have built-in governance with accountability and audit logs for all changes.
- **GitOps.** You'll be able to operate your infrastructure via Pull Requests.
- **Training.** You'll receive hands-on training so your team can operate what we build.
- **Questions.** You'll have a direct line of communication between our teams via a Shared Slack channel.
- **Troubleshooting.** You'll get help to triage when things aren't working.
- **Code Reviews.** You'll receive constructive feedback on Pull Requests.
- **Bug Fixes.** We'll rapidly work with you to fix any bugs in our projects.

## Slack Community

Join our [Open Source Community][slack] on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

## Discourse Forums

Participate in our [Discourse Forums][discourse]. Here you'll find answers to commonly asked questions. Most questions will be related to the enormous number of projects we support on our GitHub. Come here to collaborate on answers, find solutions, and get ideas about the products and services we value. It only takes a minute to get started! Just sign in with SSO using your GitHub account.

## Newsletter

Sign up for [our newsletter][newsletter] that covers everything on our technology radar.  Receive updates on what we're up to on GitHub as well as awesome new projects we discover.

## Office Hours

[Join us every Wednesday via Zoom][office_hours] for our weekly "Lunch & Learn" sessions. It's **FREE** for everyone!

[![zoom](https://img.cloudposse.com/fit-in/200x200/https://cloudposse.com/wp-content/uploads/2019/08/Powered-by-Zoom.png")][office_hours]

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://cpco.io/help-out) with our other projects, we would love to hear from you! Shoot us an [email][email].

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2020 [Cloud Posse, LLC](https://cpco.io/copyright)



## License

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

See [LICENSE](LICENSE) for full details.

```text
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
```









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know by [leaving a testimonial][testimonial]!

[![Cloud Posse][logo]][website]

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We ❤️  [Open Source Software][we_love_open_source].

We offer [paid support][commercial_support] on all of our projects.

Check out [our other projects][github], [follow us on twitter][twitter], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.



### Contributors

|  [![Andriy Knysh][aknysh_avatar]][aknysh_homepage]<br/>[Andriy Knysh][aknysh_homepage] |
|---|

  [aknysh_homepage]: https://github.com/aknysh
  [aknysh_avatar]: https://img.cloudposse.com/150x150/https://github.com/aknysh.png

[![README Footer][readme_footer_img]][readme_footer_link]
[![Beacon][beacon]][website]

  [logo]: https://cloudposse.com/logo-300x69.svg
  [docs]: https://cpco.io/docs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=docs
  [website]: https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=website
  [github]: https://cpco.io/github?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=github
  [jobs]: https://cpco.io/jobs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=jobs
  [hire]: https://cpco.io/hire?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=hire
  [slack]: https://cpco.io/slack?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=slack
  [linkedin]: https://cpco.io/linkedin?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=linkedin
  [twitter]: https://cpco.io/twitter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=twitter
  [testimonial]: https://cpco.io/leave-testimonial?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=testimonial
  [office_hours]: https://cloudposse.com/office-hours?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=office_hours
  [newsletter]: https://cpco.io/newsletter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=newsletter
  [discourse]: https://ask.sweetops.com/?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=discourse
  [email]: https://cpco.io/email?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=email
  [commercial_support]: https://cpco.io/commercial-support?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=commercial_support
  [we_love_open_source]: https://cpco.io/we-love-open-source?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=we_love_open_source
  [terraform_modules]: https://cpco.io/terraform-modules?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=terraform_modules
  [readme_header_img]: https://cloudposse.com/readme/header/img
  [readme_header_link]: https://cloudposse.com/readme/header/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=readme_header_link
  [readme_footer_img]: https://cloudposse.com/readme/footer/img
  [readme_footer_link]: https://cloudposse.com/readme/footer/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=readme_footer_link
  [readme_commercial_support_img]: https://cloudposse.com/readme/commercial-support/img
  [readme_commercial_support_link]: https://cloudposse.com/readme/commercial-support/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-cloudtrail-s3-bucket&utm_content=readme_commercial_support_link
  [share_twitter]: https://twitter.com/intent/tweet/?text=terraform-aws-cloudtrail-s3-bucket&url=https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket
  [share_linkedin]: https://www.linkedin.com/shareArticle?mini=true&title=terraform-aws-cloudtrail-s3-bucket&url=https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket
  [share_reddit]: https://reddit.com/submit/?url=https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket
  [share_facebook]: https://facebook.com/sharer/sharer.php?u=https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket
  [share_googleplus]: https://plus.google.com/share?url=https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket
  [share_email]: mailto:?subject=terraform-aws-cloudtrail-s3-bucket&body=https://github.com/cloudposse/terraform-aws-cloudtrail-s3-bucket
  [beacon]: https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/terraform-aws-cloudtrail-s3-bucket?pixel&cs=github&cm=readme&an=terraform-aws-cloudtrail-s3-bucket
