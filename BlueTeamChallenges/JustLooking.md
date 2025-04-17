# MS Sentinel: Just Looking

I followed the instructions and launched Microsoft Azure to begin the task. I injested logs and I waited around 5 minutes until the deployments had succeeded.

The first question asks, "What's the row count for SigninLogs_CL table?" 

I navigated to the logs section in azure for eastus-sentinel-04163795 and clicked on tables. From there I ran the SigninLogs_CL table and saw in the bottom right that there 
were 930 rows.

I ran the same query for the AuditLogs_CL as well and found 58 rows.

The last question is shift + enter to use KQL queries.

## Task 3

The next task implores us to deploy the rules. After waiting another few minutes, the rules are deployed and we can now answer the questions.

Once in the incidents page in Microsoft Sentinal, the question asks what MITRE ATT&CK sub technique is used for Account Created and Deleted in Short Timeframe. 
To find this, I clicked on the rule and clicked where it says alerts.

![Screenshot 2025-04-16 140005](https://github.com/user-attachments/assets/b5b44fd8-3391-4a85-b605-e10b8dcad98d)

From there we can see that sub technique is T1078.004

The next question asks, Rule frequency (in hrs) for rule: Attempts to sign in to disabled accounts. We can also query here, and through a little digging we find that the answer
is 1 hour.

To find the answer to ResultType filter in rule: Explicit MFA Deny. We do another query and find that ResultType = 5000121.

This next question "AppDisplayName filter in rule: Brute force attack against Azure Portal". I realized I actaully had to go to configuration -> analytics and not Threat 
Management.

Looking at the query of the rule, I found that AppDisplayName is filtered to be Azure Portal.

I did the same for the last question as well, I looked at the rule and found that Category is filtered to find RoleManagement.

## Task 4 Incident 1

The first incident we deal with is marked as having high severity, this trickle down approach to remediating incidents ensures the potential higher level threats are dealt
with first to mitigate any further damage.

The first question asks how many accounts were created and deleted within a short timeframe. Querying the logs, I found that looking at the UserPrincipalName was
a good indicator on how many accounts were deleted. Although 8 logs were shown, it looks like there were some redudant logs and only 5 accounts got deleted.

To find the entity that deleted the accounts, I clicked on the drop down on one of the logs and found the user that deleted them was thmMultiTenantApp.

The technique for this can be found by going back to the rule itself. When I went back to the rule, under the tactics, I found InitialAccess as the tactic for the rule.

Finding the workflow ID can be found by going back to one of the logs that was alerted and going to Deletion_AdditionalDetails. From there, the workflow ID can be found.

The UPNSuffix is also in the logs as well and is tryhackmelabs.onmicrosoft.com.

## Task 5 Incident 2

Clicking on the second incident I clicked view full details and found the ip was 181.214.151.205. After that I used an IP geolocater and put in the IP to find that the IP traces back to Miami.

The disabled account in this situation is 
marcus@tryhackmelabs.onmicrosoft.com.

![Screenshot 2025-04-16 231525](https://github.com/user-attachments/assets/ccccbfb0-c1a2-4e37-8b17-3b07008ff028)

All of this could be found from microsofts sentinals amazing GUI.

The last question asks the ResultType filter in the rule, going to the rules details I found that its 50057.

## Task 6 Incident 3

Looking at the alert, I found the answer to the first few questiosn which was CredentialAccess and Brute Force. The error code when MFA is denied is 500121 by looking at the result type.

To find the name of the access policy that is triggered, I looked under AuthenticationRequirementPolicies_s in the logs and found Security Defaults.

I found the authentication method by looking under AuthenticationDetails_s and found mobile app authentication.

The browser version is also pretty easy to find as well as its also in the logs under DeviceDetail_s. The version is Firefox 125.0.

To find the amount of entities mapped to the incident, I viewed full details in Sentinal and found that 2 entities are mapped, 
185.243.57.70 and marcus@tryhackmelabs.com

![Screenshot 2025-04-16 233549](https://github.com/user-attachments/assets/27bc9674-e7b0-4994-aa18-28a1367f1255)

## Task 7 Incident 4

The last incident! 

The first thing I did to find the UPN that escalated Marcus was look at the 2 logs. At first, I only saw one log associated with Marcus email and saw the email that Initated it was breakglass@tryhackmelabs.onmicrosoft.com.

To find the role he got escalated to, I went under the same log I used to find the UPN and went under aaa and found the new value for his role was Privileged Role Administrator.

I found the source table to be AuditLogs_CL by looking at "Type" in the same log I've been looking at for this entire incident.

The other user to be a target was usr-24052103@tryhackmelabs.onmicrosoft.com. I knew this when I looked at the first log and saw the Target UPN.

The last question! What was the initiating IP address? This was also pretty easy, I just looked at the log and scrolled down until I found initiating IP address to be 2.59.157.197.

## Thoughts

This lab is a great introduction into how to use Microsoft Azure and Sentinel as a cybersecurity analyst. Before doing this challenge, I had no idea how to use Sentinel and properly analyze logs. After, I can now see how useful Azure can be to analyzing alerts with an amazing gui and very intuitive way of finding information from logs. 


