# Qualys Zero Touch API Based Assessment
Deploy pre-requsites for Qualys Zero Touch API Based Assessment

## License
_**THIS SCRIPT IS PROVIDED TO YOU "AS IS."  TO THE EXTENT PERMITTED BY LAW, QUALYS HEREBY DISCLAIMS ALL WARRANTIES AND LIABILITY FOR THE PROVISION OR USE OF THIS SCRIPT.  IN NO EVENT SHALL THESE SCRIPTS BE DEEMED TO BE CLOUD SERVICES AS PROVIDED BY QUALYS**

**Modifications made to the script for fucntionality by Derrek Arce are offered under the GNU General Public License v3.0 (GPLv3)**

## Usage:
These scripts deploy the necessary resources to onboard an AWS Organization to Qualys TotalCloud. The source scripts provided by Qualys have been modified due to failure in testing, but these have been verified to be functional and successfully deploy resources that are visible within Qualys. 

For more information, please refer: https://docs.qualys.com/en/tc/latest/#t=get_started%2Fgetting_started.htm
Follow the below mentioned steps to deploy below to deploy pre-requsites for Qualys Zero Touch API Based Assessment.

## Prerequisite Step
1. Generate Auth token.
    --> curl --location --request POST 'https://< API Gateway URL >/auth' --header 'Content-Type: application/x-www-form-urlencoded' --data-urlencode 'username=<QualysUsername> --data-urlencode 'password=<QualysPassword>'--data-urlencode 'token=true'
2. Generate Subscription Token.
    --> curl --location --request POST 'https://< API Gateway URL >/qas/subscription-token' --header 'Content-Type: application/json' --header 'Authorization: Bearer **_Auth Token_** --data-raw '{ "expiry": 500000}'
3. Store the secret within AWS Secrets Manager for proper security best practices and to avoid disclosure of the token within the logs, noting the ARN.

## EventBridge Deployment in Master/Management Account

1. Login to AWS Console and navigate to CloudFormation. 
2. Stack > Create Stack > With new resources (standard).
3. In 'Specify template', upload the template file. --> EventBridgeCF.yml
4. Click Next.
5. Under Specify stack details, provide Stack name.
6. In APIGatewayURL parameter, provide the Qualys API Gateway URL. Find the Gateway URL at https://www.qualys.com/platform-identification/ 
7. Input the Subscription Token Secret Manager ARN
8. Keep the default settings step 3 and step 4.
9. Click Next > Submit.


## Cross-Region Event Routing

1. Login to AWS Console > Navigate to CloudFormation
2. StackSets > Create StackSets
3. Permissions: Service-Managed
6. Prerequisite - Prepare template
7. Template is ready.
8. Specify template.
9. Upload a template file. (EventBridgeCrossRegion.yml) 
10. Click “Next”
11. StackSet Name:- Qualys-cross-region-event-bridge
12. Parameters
13. Enter Master Account ID (or whatever account in which you're deploying the EventBus)
14. StackRegion : Enter the EventBridge Deployment Region
15. Click “Next” --> “Next”
16. Set deployment options
17. Deploy to Organization
19. Specify regions:- Select the regions for Cross Region Event Routing 
20. Deployment options
21. Region Concurrency: Parallel
22. Click “Next” -> Submit

## Important links

**_Note: For Qualys Zero touch API Based Assessment, make sure that your EC2 instance has the SSM Agent installed and has SSM Inventory Configured. For more information, refer the below links:_**

* [Installing and Configuring SSM Agent](http://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)
* [Configuring Security Roles for System Manager](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-access.html)
