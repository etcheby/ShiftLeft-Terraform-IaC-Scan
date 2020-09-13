# Terraform Source Code Scan + IaC Assessment using Cloudguard Shiftleft Plugin in Jenkins Pipeline. 

Using Jenkins pipeline as a script (Jenkinsfile) where we're inserting Check Point ShiftLeft CLI plugin - 
ShiftLeft CLI plugin adds security to your CI/CD pipelines with the following blades:
  a- #iac-assessment to scan infrastructure as a code (IaC) templates;
  b- #image-scan to scan container images for vulnerabilities;
  c- #sourceguard to scan source code.

PS: To leverage the sourceguard blade you would need SourceGuard API token on top of the Cloudguard API token. 
    Shiftleft CLI is currently in EA (preview). To create SourceGuard tenant go to --> https://portal.checkpoint.com 


# Jenkins Pipeline Workflow 
This Jenkins pipeline is triggered by "Commit" to my GitHub repository hosting the Terraform template. In the build stages, we will respectively scan source code (look for harcoded credentials, malicious IPs, vulnerabilities) and assessment the templates against CloudGuard Compliance Ruleset " Terraform AWS CIS Foundations", initialize and plan and deploy the AWS infrasctructure from TF template using the Check Point DevSecOps container /dhouari/devsecops which itself already contains Terraform CLI.   
