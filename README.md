# aws-securityhub-to-email
Demonstrates sending AWS Security Hub findings to your Email 

## Coming Soon!
This post will introduce you to the process of creating AWS Security Hub a custom action by sending findings to Email.  After reading this blog you will understand the process to create your own custom actions for utilization in your Security Operations play books.

## Send Security Hub Finding to Email

1.	Launch CloudFormation Stack 
⋅⋅*	Navigate to https://console.aws.amazon.com/cloudformation/
⋅⋅*	Select Create stack
c.	Select Upload a template file
d.	Select Choose file and locate “SecurityHubFindingsToEmail.json” on your local machin
e.	Select Next.

•	Stack Name:  EnableSecurityHubFindingsEmails
•	EmailAddress: “Your Email to send Security Hub Findings to”

f.	Complete Create Stack form
g.	Select Next, fill out any Tags and select Next again
h.	Accept IAM Resource creation
i.	Select Create Stack, CloudFormation will then begin creating the stack
j.	Wait for the CloudFormation console to report stack creation complete
2)	Check your email to confirm SNS Topic Subscription
3)	Enable Security Hub 
a.	As a  best business practice, Security Hub should be enabled in each AWS region by first navigating to the console (https://console.aws.amazon.com/securityhub/) in each region and clicking on Enable Security Hub. Note: If you are testing Security Hub you can just enable the service in the same region as you deployed the CloudFormation stack.
b.	Then, review Service Role Permissions and click on Enable Security Hub

c.	More information about the IAM Roles Security Hub utilizes can be found at (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html)
4)	Create Security Hub Custom Actions
a.	In the Security Hub Settings (https://console.aws.amazon.com/securityhub/home? - /settings) choose the Custom Actions tab. Select Create custom action. Then in the Create custom action pop up, specify the action name, description and ID then choose OK to create the action.
•	Name: Send to Email
•	Description: This custom action sends selected findings to email addresses defined in an SNS Topic Subscription.
•	Custom action ID: SendToEmail

b.	Your custom action will now show in the listing of Security Hub custom actions console.

5)	Optional step to customize the Email notification
a.	Navigate to the CloudWatch Events console (https://console.aws.amazon.com/cloudwatch/) and select the Events/Rules menu.
b.	Click on the CloudWatch Event Rule that was created by the CloudFormation Template. It will be named “CloudWatchEventRuleFindingsToEmail”
c.	Select Actions and click on Edit

d.	On the right-hand side of the “Create Rule” console, the SNS Topic “SendFindingsTopic” should be the selected target.
e.	Click “Configure Input”, Select the “Input Transformer” Radio button and fill in the following fields. Customize if you want.

Input Path: 

{"findingTime":"$.detail.findings[0].updatedAt","finding":"$.detail.findings[0].Types[0]","region":"$.detail.findings[0].Resources[0].Region","account":"$.detail.findings[0].AwsAccountId","findingDescription":"$.detail.findings[0].Description"}

Input Template: 

"AWS SecurityHub finding in <region> for Account: <account>. The finding is <finding> and the description of the finding is <findingDescription>.”

f.	Click Configure details, then Click Update rule to save

6)	To test out this custom action, navigate to “Testing Custom Actions in AWS Security Hub” at the end of this post.
