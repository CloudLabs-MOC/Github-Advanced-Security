# Module 07: Security Campaign

## Lab Scenario

 In this lab, we’ll cover a series of tasks designed to provide a comprehensive understanding of creating, launching, tracking, and managing security campaigns.

## Lab Objectives
In this lab, you will perform:

- Task 1: Creating security camapaign
- Task 2: Tracking Security Campaign
- Task 3: Editing and Managing Security Campigns

## Estimated Timing: 30 minutes

## Architecture Diagram

   ![](../images/seccamp16.png)

### Task 1: Creating Security Camapaign

**Security Campaigns:** [GitHub Security Campaigns](https://docs.github.com/en/enterprise-cloud@latest/code-security/code-scanning/managing-code-scanning-alerts/fixing-alerts-in-security-campaign) are a feature within GitHub Advanced Security designed to help teams address security vulnerabilities at scale. These campaigns use Copilot Autofix to suggest fixes for up to 1,000 code scanning alerts at a time, allowing developers and security teams to collaborate efficiently. By prioritizing and fixing these alerts, teams can significantly reduce security debt and improve the overall security of their codebase

1. Select the **ghas-bootcamp-xxxx-xx-xx-<inject key="Deployment-id" enableCopy="false"/>** organization from the top.

   ![](../images/T1S1iii.png)

1. Navigate to your organization's **Security** tab.

   ![Picture1](../images/security-tabat.png)

1. In the left sidebar, click on the **Campaigns (1)**, click on **Create campaign (2)** and select **From template (3)**.

    ![](../images/T1S3.png)

1. Choose **Critical CodeQL Alert** template to use for the campaign.

   - **Critical CodeQL Alert:** It indicates a severe security vulnerability detected by GitHub's CodeQL analysis. CodeQL is a powerful semantic code analysis engine that identifies potential security issues in your codebase. Critical alerts often require immediate attention to prevent exploitation12.

   - **MITRE Top 10 KEV:** The MITRE Top 10 Known Exploited Vulnerabilities (KEV) list highlights the most critical and frequently exploited software weaknesses. These weaknesses are prioritized based on their prevalence and potential impact34. This reduces risk, prevents breaches and can help protect sensitive data.

   - **SQL Injection (CWE-89):** It occurs when an application improperly neutralizes special elements in SQL commands. This allows attackers to manipulate SQL queries, potentially leading to unauthorized data access or modification. Mitigations include using parameterized queries, prepared statements, and input validation

   - **Cross-Site Scripting (CWE-79):** It involves injecting malicious scripts into web pages viewed by other users. This can lead to data theft, session hijacking, or defacement. Preventing XSS involves proper input validation, output encoding, and using security frameworks that automatically handle these issues78.

   ![](../images/T1S4i.png)

1. Click on **Save as** and select **Published campaign**.

   ![](../images/T1S5iii.png)

1. Edit the **Campaign name** and **Short description**, define a **Campaign due date**, assign a **Campaign manager** as the primary contact, and finally, click **Create campaign** to initiate it.

    ![](../images/T1S6i.png)

8. It will display all the CodeQL Critical Alerts; next, open the dropdown for **ghas-bootcamp-WebGoat** to view its alerts.

    ![](../images/T1S7i.png)

1. Now, click on the first **Server-Side request forgery** issue.

    ![](../images/a42b-1.png)

1. Click on **Commit to new branch.**

    ![](../images/a42c.png)

   >**Note:** There is a chance that the option to **Commit** may not appear. In this case, look for the **Generate Fix** option, click on it, and once the fix is generated, you will be able to commit.

10. Select the option to open a pull request, then click **Commit changes**.

    ![](../images/seccamp4ab.png)

11. Click on **Ready for review**.

    ![](../images/seccamp5b.png)

12. Click on **Merge pull request** to finalize and integrate the changes into the main branch, then click on **Confirm merge**.

    ![](../images/T1S12iii.png)
    ![](../images/T1S12ii.png)

### Task 2: Tracking Security Campaign

When you create a campaign, the campaign tracking view is displayed and the campaign is listed in the sidebar of the **Security** tab for the organization. You can redisplay the campaign tracking view at any time by selecting it in the sidebar under "Campaigns".

1. Here, you can view the details of the **Campaign's progress** and **Status**, along with the alerts supported by **Copilot Autofix**.

- **In progress:** when at least one branch or pull request is created to fix the alert through the campaign view or the alert page.
- **Closed:** when the alert is fixed or dismissed, even if the development work was done outside the campaign framework.

   ![](../images/a54a.png)

   ![](../images/a54b.png)

   >**Note:** It will take upto 5-30 minutes to get it complete.
   
   >**Note:** If you move to the next alert before completing the previous fix suggestion, you may encounter a message stating that the fix suggestion was discarded due to new code being pushed. In this case, you’ll need to generate the fix again.

2. Navigate to the **ghas-bootcamp-WebGoat** repository from the repository section. Select **Security** from the top menu, then click on **Critical CodeQL alert** under Campaigns. Here, you will see that **One of the alerts havs been closed**.

   ![](../images/a47.png)

   > **Note:** This status will also be reflected on the Security Campaign page, but it may take a few minutes to update.

    >**Note:** Even after sometime if you are not able to see the progress you can feel free to move to the next task/exercise as the progress is dependent on Copilot results this may vary.


1. Click on the open alert as metioned below.

   ![](../images/a48.png)

1. On the selected alert page click on **Commit to new branch**.

   ![](../images/a49a.png)

   >**Note:** There is a chance that the option to **Commit to new branch** may not appear. In this case, look for the **Generate Fix** option, click on it, and once the fix is generated, you will be able to commit.
   

1. Select the option to open a pull request, then click **Commit changes**.

    ![](../images/seccamp4a.png)

1. Click on **Ready for review**.

    ![](../images/seccamp5b.png)

1. Click on **Merge pull request** to finalize and integrate the changes into the main branch, then click on **Confirm merge**.

    ![](../images/seccamp6b.png)
    ![](../images/T2S7.png)

1. Now try refreshing the page.

1. Now you can see that the alert has been closed.

   ![](../images/a51a.png)

   ![](../images/a51.png)

   > **Note:** This status will also be reflected on the Security Campaign page, but it may take a **5-30 minutes** to update. Refresh the page to get recent update.

1. Now, click on the remaining **alert** and follow the same steps as before to generate a fix.

   ![](../images/a52.png)

   > **Note:** There is a chance also you can see that you will not be able to close it as the **Copilot Autofix attempted to generate an autofix for this alert, but wasn't able to**. It entirely depends on the type of alert and whether GitHub Copilot is capable of generating a fix for it.

   >**Note:** Even after sometime if you are not able to see the progress you can feel free to move to the next task/exercise as the progress is dependent on Copilot results this may vary.

   ![](../images/a53a.png)

### Task 3: Editing and Managing Security Campigns

There is a limit of 10 active campaigns. When a campaign is complete, or if you want to pause it, you should close it. When you close a campaign, it's no longer displayed for developers in the repository Security tab but you can still display the campaign tracking view to develop best practice. In addition, you can reopen a closed campaign from the "Closed campaigns" view, which is accessible from the sidebar in the Security tab of the organization.

1. On GitHub, navigate to the main page of the organization.

1. Under your organization name, click **Security**.

   ![Picture1](../images/security-tabat.png)

1. In the sidebar, under "Campaigns" click the name of the campaign to display the campaign tracking view.

1. In the campaign title row, click and select **Edit campaign**.

   ![Picture1](../images/security-tabata.png)

1. In the **Edit Campaign** section, you can update the **Campaign name**, **Short description**, **Campaign due date**, and **Campaign manager**. Once done, click on **Save changes** to apply the updates.

   ![](../images/T3S5.png)

1. For deleting or closing the campaign select the required option **close campaign** or **delete campaign** by selecting the campaign.

- **Close campaign** to remove it from the active campaigns list and display it on the Closed campaigns view.

- **Delete campaign** to delete the campaign permanently. In the "Delete campaign" dialog, click Delete to confirm that you want to delete the campaign.

   ![](../images/T3S6.png)

## Review
In this lab you have completed the following:

- Created a campaign from a template
- Tracked Security Campaigns
- Edited and Managed Security Campigns

### You have successfully completed the lab, Click on **Next** to continue.
