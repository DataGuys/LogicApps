# Deployment Guide

This guide will help you deploy the XDR High Incident Logic App to your Azure environment.

## Prerequisites

1. **Azure Subscription** with permissions to create Logic Apps and API connections
2. **Microsoft Teams** workspace with admin permissions
3. **Microsoft Defender XDR** configured and generating incidents
4. **GitHub Account** to fork this repository

## Step 1: Fork and Prepare Repository

1. **Fork this repository** to your GitHub account
2. **Get your repository details**:
   - GitHub username: `YOUR-USERNAME`
   - Repository name: `YOUR-REPO-NAME`
3. **Update the Deploy button URL** in README.md:
   - Replace `YOUR-USERNAME` with your GitHub username
   - Replace `YOUR-REPO` with your repository name

## Step 2: Get Teams Channel Information

1. Open Microsoft Teams
2. Navigate to the channel where you want to receive incident notifications
3. Right-click on the channel name and select **"Get link to channel"**
4. Copy the URL - it will look like:

   ```url
   https://teams.microsoft.com/l/channel/19%3A{CHANNEL_ID}%40thread.tacv2/General?groupId={GROUP_ID}
   ```

5. Extract the values:
   - **Group ID**: The value after `groupId=`
   - **Channel ID**: The value between `19%3A` and `%40thread.tacv2`

## Step 3: Deploy to Azure

1. Click the **"Deploy to Azure"** button in your repository's README
2. Fill in the required parameters:
   - **Teams Group ID**: Enter the Group ID from Step 2
   - **Teams Channel ID**: Enter the Channel ID from Step 2
   - **Logic App Name**: Choose a unique name (default: `xdr-high-incident-workflow`)
   - **Resource Group**: Select existing or create new
   - **Location**: Choose your preferred Azure region
   - **Other parameters**: Customize as needed or keep defaults

## Step 4: Post-Deployment Configuration

### 4.1 Authorize Teams Connection

1. Go to the Azure portal
2. Navigate to your resource group
3. Find the API connection named `teams-connection`
4. Click on it and select **"Edit API connection"**
5. Click **"Authorize"** and sign in with your Teams account
6. Save the connection

### 4.2 Grant Graph API Permissions

1. In Azure portal, go to **Azure Active Directory** > **Enterprise applications**
2. Find your Logic App (search by the name you chose)
3. Go to **Permissions** and add the following Microsoft Graph permissions:
   - `SecurityIncident.Read.All`
   - `SecurityAlert.Read.All`
4. Grant admin consent for your organization

### 4.3 Test the Deployment

1. Go to your Logic App in the Azure portal
2. Click **"Run trigger"** > **"Recurrence"** to test manually
3. Check the run history to verify it completes successfully
4. Check your Teams channel for test notifications

## Step 5: Customize (Optional)

- **Recurrence Schedule**: Modify the trigger frequency in the Logic App designer
- **Incident Filters**: Adjust the Graph API query to filter different incident types
- **Teams Message**: Customize the HTML template in the "Post message" action
- **Action URLs**: Update the placeholder URLs with your actual automation endpoints

## Troubleshooting

### Common Issues

1. **Teams connection fails**: Ensure you have admin permissions in Teams
2. **No incidents appear**: Check if Defender XDR is generating high-severity incidents
3. **Permission errors**: Verify the managed identity has the required Graph API permissions
4. **Template deployment fails**: Check that all parameter values are valid

### Support

- Review the Logic App run history for detailed error information
- Check Azure Activity Log for deployment issues
- Ensure your Azure subscription has sufficient permissions

## Security Notes

- The Logic App uses managed identity for secure authentication
- No credentials are stored in the Logic App
- Review and audit the automation URLs before deploying to production
- Consider implementing approval workflows for destructive response actions
