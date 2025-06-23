# XDR High Incident Logic App

This Logic App monitors Microsoft Defender XDR for high severity incidents and sends detailed notifications to Microsoft Teams with an attack story timeline and response actions.

## Features

- Monitors for high severity incidents every 10 minutes
- Generates attack story timeline from alert evidence
- Sends rich HTML notifications to Teams
- Includes action buttons for incident response workflows
- Uses managed identity for Graph API authentication

## Prerequisites

- Azure subscription
- Microsoft Teams with appropriate permissions
- Microsoft Defender XDR configured
- Resource group for deployment

## Deploy to Azure

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FYOUR-USERNAME%2FYOUR-REPO%2Fmain%2Fazuredeploy.json)

## Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| logicAppName | Name of the Logic App | xdr-high-incident-workflow |
| teamsGroupId | Teams Group ID where notifications will be sent | Required |
| teamsChannelId | Teams Channel ID where notifications will be sent | Required |
| recurrenceInterval | Recurrence interval in minutes | 10 |
| timeZone | Time zone for the recurrence schedule | Pacific Standard Time |
| jiraBaseUrl | Base URL for Jira instance | https://your-company.atlassian.net |
| jiraProjectKey | Jira project key for security incidents | SEC |
| desktopResponseUrl | URL for desktop response automation workflow | https://your-logic-app-url.com/... |
| serverP2ResponseUrl | URL for server P2 response workflow | https://your-logic-app-url.com/... |
| autoInvestigationUrl | URL for auto investigation workflow | https://your-logic-app-url.com/... |

## Post-Deployment Configuration

1. **Configure Teams Connection**: After deployment, authorize the Teams API connection in the Azure portal
2. **Grant Graph API Permissions**: Assign the Logic App's managed identity the following permissions:
   - SecurityIncident.Read.All
   - SecurityAlert.Read.All
3. **Update URLs**: Replace placeholder URLs with your actual automation endpoints
4. **Test the Workflow**: Manually trigger the Logic App to verify functionality

## Teams Channel Setup

To get your Teams Group ID and Channel ID:

1. In Teams, right-click on the channel and select "Get link to channel"
2. The URL will contain the Group ID and Channel ID:
   ```
   https://teams.microsoft.com/l/channel/19%3A{CHANNEL_ID}%40thread.tacv2/General?groupId={GROUP_ID}
   ```

## Customization

- Modify the recurrence schedule in the trigger
- Adjust the incident filter criteria in the Graph API call
- Customize the Teams message template
- Add additional response actions or integrations

## Security Considerations

- The Logic App uses managed identity for authentication
- Ensure appropriate RBAC permissions are configured
- Review and audit the automation workflows regularly
- Consider implementing approval workflows for destructive actions
