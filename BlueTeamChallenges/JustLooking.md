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
