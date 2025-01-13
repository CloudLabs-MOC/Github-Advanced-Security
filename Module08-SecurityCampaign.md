# Module 05: Security Campaign

## Lab Scenario

 In this lab, we’ll cover a series of tasks designed to provide a comprehensive understanding of creating, launching, tracking, and managing security campaigns.

## Lab Objectives
In this lab, you will perform:

- Task 1: Turn on the default setup on a juice-shop repository
- Task 2: Creating security camapaign
- Task 3: Tracking Security Campaign
- Task 4: Editing and Managing Security Campigns

## Estimated Timing: 30 minutes

## Architecture Diagram

   ![](images/seccamp16.png)

### Task 1: Turn on the default setup on a juice-shop repository

1. Go to your profile on top of the right hand, and then select **Your organizations**.

   ![Picture1](./images/org.png)

1. Select **ghas-bootcamp-xxxx-xx-xx-cloudlabsxxx** from organizations.

   ![Picture1](./images/ghas-exercise1-4.png)

1. From the list of repositories click on **juice-shop** to begin working through this module.

   ![](images/seccamp13.png)

1. In the GitHub repository navigate to **Settings** from the top navigation pane and click on **Code Security** under Security.

   ![github-advisory-database](images/image1.png)

   >**Note**: Zoom out of the page if you are unable to see the **Settings** option from the top navigation pane.

1. Enable **GitHub Advanced Security** option.

   ![github-advanced-security](images/ghasopt.png)

1. Click on **Enable GitHub Advanced Security for this repository**.

   ![github-advanced-security](images/ghasopt1.png)

1. Scroll down and you will find the **Code Scanning** option. Scroll to the right then click on the **Setup (1)** button and we have two options to configure code scanning. That is **Default** and **Advanced** as depicted in the screenshot below. For now, click on **Default (2)**

   ![github-advisory-database](images/sec15.png)

1. Leave the query suite on Default and click the **Enable CodeQL** button.

   ![](images/seccamp14.png)

1. After you've clicked the **Enable CodeQL** button, go to the **_Actions_** tab to confirm that the initial scan has kicked off. The scan should take a couple of minutes. You can also check more configuration and details inside the the github action run.

   ![github-advisory-database](images/gf.png)

### Task 2: Creating Security Camapaign

**Security Campaigns:** [GitHub Security Campaigns](https://docs.github.com/ja/enterprise-cloud@latest/code-security/code-scanning/managing-code-scanning-alerts/fixing-alerts-in-security-campaign) are a feature within GitHub Advanced Security designed to help teams address security vulnerabilities at scale. These campaigns use Copilot Autofix to suggest fixes for up to 1,000 code scanning alerts at a time, allowing developers and security teams to collaborate efficiently. By prioritizing and fixing these alerts, teams can significantly reduce security debt and improve the overall security of their codebase

1. Select the **ghas-bootcamp-xxxx-xx-xx-cloudlabsxxx** organization from the top.

   ![](images/seccamp15.png)

1. Navigate to your repository’s **Security** tab.

   ![Picture1](./images/security-tabat.png)

1. In the left sidebar, click on the **Start a new security campaign** next to "Campaigns" to begin creating a new campaign.

    ![](./images/securitycampaign1.png)

1. Choose a **Critical CodeQL Alert** template to use for the campaign.

- **Critical CodeQL Alert:** It indicates a severe security vulnerability detected by GitHub's CodeQL analysis. CodeQL is a powerful semantic code analysis engine that identifies potential security issues in your codebase. Critical alerts often require immediate attention to prevent exploitation12.

- **MITRE Top 10 KEV:** The MITRE Top 10 Known Exploited Vulnerabilities (KEV) list highlights the most critical and frequently exploited software weaknesses. These weaknesses are prioritized based on their prevalence and potential impact34. This reduces risk, prevents breaches and can help protect sensitive data.

- **SQL Injection (CWE-89):** It occurs when an application improperly neutralizes special elements in SQL commands. This allows attackers to manipulate SQL queries, potentially leading to unauthorized data access or modification. Mitigations include using parameterized queries, prepared statements, and input validation

- **Cross-Site Scripting (CWE-79):** It involves injecting malicious scripts into web pages viewed by other users. This can lead to data theft, session hijacking, or defacement. Preventing XSS involves proper input validation, output encoding, and using security frameworks that automatically handle these issues78.

    ![](./images/template2.png)

7. Edit the **Campaign name** and **Short description**, define a **Campaign due date**, assign a **Campaign manager** as the primary contact, and finally, click **Create campaign** to initiate it.

    ![](./images/seccamp1.png)

8. It will display all the CodeQL Critical Alerts; next, open the dropdown for **juice-shop** to view its alerts.

    ![](./images/seccamp2a.png)

9. Open **juice-shop**, select all its alerts, and from the right-hand panel, click **Commit autofix** and choose **Commit to new branch**.

    ![](./images/seccamp3a.png)

10. Select the option to open a pull request, then click **Commit changes**.

    ![](./images/seccamp4a.png)

11. Click on **Ready for review**.

    ![](./images/seccamp5a.png)

12. Click on **Merge pull request** to finalize and integrate the changes into the main branch, then click on **Confirm merge**.

    ![](./images/seccamp6a.png)
    ![](./images/seccamp7a.png)

### Task 3: Tracking Security Campaign

When you create a campaign, the campaign tracking view is displayed and the campaign is listed in the sidebar of the **Security** tab for the organization. You can redisplay the campaign tracking view at any time by selecting it in the sidebar under "Campaigns".

1. Here, you can view the details of the **Campaign's progress** and **Status**, along with the alerts supported by **Copilot Autofix**.

- **In progress:** when at least one branch or pull request is created to fix the alert through the campaign view or the alert page.
- **Closed:** when the alert is fixed or dismissed, even if the development work was done outside the campaign framework.

  ![](./images/seccamp8.png)

2. Navigate to the **juice-shop** repository from the repository section. Select **Security** from the top menu, then click on **Critical CodeQL alert** under Campaigns. Here, you will see that **All alerts have been closed**.

   ![](./images/seccamp12.png)

   > **Note:** This status will also be reflected on the Security Campaign page, but it may take a few minutes to update.

### Task 4: Editing and Managing Security Campigns

There is a limit of 10 active campaigns. When a campaign is complete, or if you want to pause it, you should close it. When you close a campaign, it's no longer displayed for developers in the repository Security tab but you can still display the campaign tracking view to develop best practice. In addition, you can reopen a closed campaign from the "Closed campaigns" view, which is accessible from the sidebar in the Security tab of the organization.

1. On GitHub, navigate to the main page of the organization.

1. Under your organization name, click **Security**.

   ![Picture1](./images/security-tabat.png)

1. In the sidebar, under "Campaigns" click the name of the campaign to display the campaign tracking view.

1. In the campaign title row, click and select **Edit campaign**.

   ![](./images/seccamp9.png)

1. In the **Edit Campaign** section, you can update the **Campaign name**, **Short description**, **Campaign due date**, and **Campaign manager**. Once done, click on **Save changes** to apply the updates.

   ![](./images/seccamp10.png)

1. For deleting or closing the campaign select the required option **close campaign** or **delete campaign** by selecting the campaign.

- **Close campaign** to remove it from the active campaigns list and display it on the Closed campaigns view.
- **Delete campaign** to delete the campaign permanently. In the "Delete campaign" dialog, click Delete to confirm that you want to delete the campaign.

   ![](./images/seccamp11.png)

## Review

In this lab you have completed the following:

- Turned on the default setup on a juice-shop repository
- Created a campaign from a template
- Tracked Security Campaigns
- Edited and Managed Security Campigns
