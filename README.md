# AWS CloudGuard IaaS Transit Gateway Demonstration 

Terraform scripts for transit gateway demonstration of CloudGuard in AWS 
Builds the complete environment with web and application servers, northbound and southbound e-w hubs 

---------------------------------------------------------------
One time preparation of the AWS account 
1.	Create or choose a ssh key-pair in the account for the DC you are using
2.	Subscribe to the ELUAs for R80.20 BYOL gateway and management 
    R80.20 management 
    https://aws.amazon.com/marketplace/pp/B07KSBV1MM?qid=1558349960795&sr=0-4&ref_=srh_res_product_title

    R80.20 Gateway
    https://aws.amazon.com/marketplace/pp/B07LB3YN9P?qid=1558349960795&sr=0-5&ref_=srh_res_product_title

3.	Create security credentials for the API login (for terraform)
4.  Ensure you have enough resources in the account, this script creates 6 VPC, 1 transit gateway and 12 instances, the cost for this will be a few dollars per hour, so it is recommended to destroy the resources when not using them  

----------------------------------------------------------------

One time preparation of the Terraform scripts  
1. Modify the variables.tf to suite your needs   
2. Delete Route53.tf if not needed  
3. Run terraform init  

------------------------------------------------------------------

Solution Documentation   

The terraform script deploys these 3 CloudFormation templates with all the glue to configure them  
  template_url        = "https://s3.amazonaws.com/CloudFormationTemplate/management.json"  
  template_url        = "https://s3.amazonaws.com/CloudFormationTemplate/checkpoint-tgw-asg-master.yaml"  
  template_url        = "https://s3.amazonaws.com/CloudFormationTemplate/autoscale.json"  

TGW documentation (Outbound cluster)  
https://sc1.checkpoint.com/documents/IaaS/WebAdminGuides/EN/CP_CloudGuard_AWS_Transit_Gateway/html_frameset.htm

Autoscale Documentation (Inbound cluster)  
https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk112575   

Modules  
  checkpoint.tf   - Contains the CFT for the gateways and manager\
  tgw.tf\
  instances.tf\
  subnets.tf\
  vpc.tf\
  routes.tf\
  external_nlb.tf\
  external_alb.tf - Not used due to issues with ALB\
  internal_lb.tf        - app1\
  internal_lb_app2.tf   - app2\
  variables.tf\
  route53.tf        - Optional, delete if R53 is not used  

-------------------------------------------------------------------

To run the script  
    terraform init  
    terraform apply  

You can Logon after about 30 mins to the manager via the windows based Check Point SmartDashboard

To remove the environment  
1. set the autoscale group to 0 instances for the outbound autoscale group, wait a few minutes to allow the VPNs to be deleted then run;  
    terraform destroy 

Note: To use an existing manager then some modifications will be needed to terraform scripts and you will need to setup the autoprov-cfg 
