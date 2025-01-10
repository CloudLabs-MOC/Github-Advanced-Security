# Module 04: Code Scanning

## Lab Scenario

 In this lab, we'll cover a series of tasks designed to provide a comprehensive understanding of code scanning in GitHub. 

## Lab Objectives

### Implementing GitHub Advanced Security for Your Organization

In this lab, you will learn and perform:
- Introduction to Code Scanning
- What is CodeQL and how is it different from other static analysis tools? 
- Task 1: Turn on the default setup on a Python repository
- Task 2: Turn on advanced setup on a Java repository
- Task 3: Add some vulnerable code via a pull request and view the scan results in the PR 
- Task 4: Turn on GitHub Advanced Security for Organization
- Task 5: Verify Github Autofix is enabled
- Task 6: Create a code with potencial security vulnerabilities
- Task 7: Run a code scan
- Task 8: Apply autofixes to vulnerabilities

## Estimated timing: 90 minutes

## Architecture Diagram

   ![](images/seccamp18.png)
   
## Introduction to Code Scanning 

Code scanning in GitHub is a powerful feature designed to enhance the security of your software projects by automatically identifying and alerting you to potential security vulnerabilities in your codebase. Leveraging advanced static analysis techniques, and code scanning helps detect security flaws, bugs, and other issues early in the development process, enabling developers to address them proactively before they escalate into larger problems. 

### What is CodeQL and how is it different from other static analysis tools?

1. Code Scanning, powered by the CodeQL engine, performs thorough static analysis by accessing source code and integrating with the build process for compiled languages (or simulating compilation for interpreted languages). This approach ensures precise mapping of data flow and the ability to differentiate between remote and local sources. 

1. The fundamental difference is that all of the information about the application is aggregated in a relational database that allows for tracing complete data flows across the entire application.

1. For `compiled` languages, the CodeQL engine running under the hood of the Code Scanning process will hook into the compiler at build time. The CodeQL engine will then listen for the creation of data flows by the compiler, such as linkers and callbacks, and map those data flows as nodes in the database -- aptly called `DataFlow` nodes.
1.  This process allows CodeQL to avoid false positive vulnerability findings from dead code that has no existing dataflows. This is a common problem with other Static Analysis tools that do not have access to the compiler and instead rely on pattern matching and other techniques to identify vulnerabilities.
1. Once the data flow analysis is complete, an extraction of the code is then performed. Every variable, expression (combination or modification of variable(s)), method/function/class declaration, etc. is extracted as individual nodes in the database.

1. CodeQL then performs analysis by querying the database for _Remote_ flow sources that lead to sinks (where data is stored or executed) in ways that are exploitable and are otherwise not sanitized as part of that data flow.

1. For `interpreted` languages, like Javascript and Python, the CodeQL engine performs a depth-first, recursive extraction of the code where `DataFlow` nodes are created from things like `return` statements and passing variables from one function to another. We can gain a comprehensive view of the application and avoid flagging false positive vulnerabilities in code that is never called or executed.

## Task 1: Turn on the default setup on a Python repository

### Default Setup

In this task, you will learn how to activate the default CodeQL setup for a Python repository. This will include enabling CodeQL scanning with the default query suite, which focuses on providing highly accurate and actionable security findings.

1. In the **ghas-bootcamp-xxxx-xx-xx-cloudlabsxxxx** organization, click on repositories from the top navigation pane.

   ![github-advisory-database](images/getrepo.png)

1. From the list of repositories click on **ghas-bootcamp-python** to begin working through this module. This repository should have at least 2 code scanning findings with the **Default** and the **Extended** setup in this repository.

   ![github-advisory-database](images/i6.png)

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

   ![github-advisory-database](images/ge.png)

1. While that runs (It'll take approx. 5 minutes to get it enabled), let us see the difference between the two query suites.
	  - The Default query suite (also known as the `code-scanning` query suite in the _Advanced_ setup) has a less than 10% False Positive rate from findings within the Open Source ecosystem. We focus very heavily on providing true positive findings that are remotely exploitable, and this suite is the most "dialed in" in terms of findings.
	  - The Extended query suite (also known as the `security-extended` query suite in the _Advanced_ setup) has a less than 30% False Positive rate from findings within the Open Source ecosystem. You will find several interesting queries pulled into this suite, including _Memory Exploitation_ findings for C/C++ and other slightly more niche security vulnerabilities in other languages.

1. After you've clicked the **Enable CodeQL** button, go to the **_Actions_** tab to confirm that the initial scan has kicked off. The scan should take a couple of minutes. You can also check more configuration and details inside the the github action run.

   ![github-advisory-database](images/gf.png)

## Task 2: Turn on advanced setup on a Java repository

In this task, you will learn how to enable and configure the advanced CodeQL setup for a repository. You will gain skills in modifying the codeql.yml file to use the security-extended query suite and understand the benefits of advanced configurations for detecting a broader range of security vulnerabilities. You will also become familiar with the process of validating and reviewing CodeQL actions and results.

### Advanced Setup

 Now, let us enable  Advanced set-up in a different repository.

1. In the **ghas-bootcamp-xxxx-xx-xx-cloudlabsxxxx** organization, click on repositories from the top navigation pane.

   ![github-advisory-database](images/getrepo.png)

1. From the list of repositories click on **ghas-bootcamp-java** to begin working through this module. 

   ![github-advisory-database](images/i7.png)

1. Next, we're going to enable Advanced Setup for one of our compiled language repositories by going back to the `ghas-bootcamp-java` repository.

1. In the GitHub repository navigate to **Settings** from the top navigation pane and click on **Code Security** under Security.

   ![github-advisory-database](images/image1.png)

1. If code scanning is already enabled, select "Advanced Security" from the dropdown to switch to the advanced option.
   
   ![github-advisory-database](images/switch_advance.png)

1. A Pop-up Appears disbale Codeql

   ![github-advisory-database](images/disable_codeql.png)
   
1. Scroll to the right then click on the **Setup (1)** button and then click on **Advanced (2)**

   ![github-advisory-database](images/sec16.png)

   >**Note:** If it is already enabled, click on **Switch to Advanced**

1. Now you are redirect to the codeql.yml file in github/ workflows directory, and we need to copy the string **queries: security-extended (1)** from the **codeql.yml** file  and append this to a new line at number 66 under **Initialize CodeQL** section , which will read as **queries: security-extended (2)** as shown in the below screenshot. Commit these changes to your **main branch (3)**.

   ![github-advisory-database](images/java-codeql-ymlat.png)

   >**Note:** When updating the `codeql.yml` file, be aware that the line number for the **`Initialize CodeQL`** section may shift slightly, potentially changing from line 66 to one or two lines higher or lower. Ensure you append the string **`queries: security-extended`** to the correct location under this section based on the current line numbers in your file.
  
1. Now,  go to the **Actions** tab to confirm the CodeQL action is running.

   ![github-advisory-database](images/gj.png)
   
1. You can also review the code and results of that action.

   ![github-advisory-database](images/runactions.png)

   
   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
	
   - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
     >**Note:** Upon clicking the **Validate** button for this exercise, you'll receive a prompt to input your Organization name. Provide your **Organization name** which looks like **ghas-bootcamp-xxxx-xx-xx-cloudlabsxxxx**.
    
     >**Note:** Make sure to update the name of your organization, **ghas-bootcamp-xxxx-xx-xx-cloudlabsxxxx**.
    
     ![github-advanced-security](images/ghas-exercise1-8.png)
   
   - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="fdc513c6-e817-48a5-aec1-8875b2e7b0a4" />

## Task 3: Add some vulnerable code via a pull request and view the scan results in the PR  

### Task 3.1: Pull Request scans and Accurate Findings

In this task, you will learn how to enhance CodeQL's security analysis by enabling advanced query configurations and integrating extended security queries. You will gain experience in modifying CodeQL workflows, committing changes related to security vulnerabilities in code, and understanding how CodeQL will accurately identify specific issues, such as clear-text logging of sensitive information, while filtering out less relevant findings. This will deepen your understanding of leveraging CodeQL for more precise and effective security scanning in your codebase.

1. Next, we're going to enable _Advanced setup_ for one of our interpreted language repositories by going back to the `ghas-bootcamp-python` repository. Follow the instructions from the **Getting Started** section to go to the `ghas-bootcamp-python` repository.

1. Now, head over to **Settings** -> **Code security** -> scroll down to **Code scanning** and click the `...` and then click _Switch to advanced_. This will prompt us to turn off the existing CodeQL workflow to avoid duplicating Action runs.Click on **Disable CodeQL**

   ![github-advisory-database](images/i8.png)

   ![github-advisory-database](images/gl.png)

1. We are going to make similar updates to the `codeql.yml` file as we did in the `Advanced Setup` section, we need to copy the string **queries: security-extended (1)** from the **codeql.yml** file  and append this to a new line at number 66 under **Initialize CodeQL** section , which will read as **queries: security-extended (2)** as shown in the below screenshot. Commit these changes to your **main branch (3)** by clickin on **Commit changes.**

   ![github-advisory-database](images/python-codeql-ymlat.png)

   >**Note:** When updating the `codeql.yml` file, be aware that the line number for the `Initialize CodeQL` section may shift slightly, potentially changing from line 66 to one or two lines higher or lower. Ensure you append the string `queries: security-extended` to the correct location under this section based on the current line numbers in your file.

1. In the **Code** tab of the Python repository, navigate to the server folder to open the `routes.py` file and scroll down to **Line 40**.

   ![github-advisory-database](images/gm.png)
 
  
1. Notice that this part of the code is related to the vulnerabilities that have to do with SQL.

1. Add a **space** at the end of line 73:
`data = subprocess.check_output(command, shell=True)`.This is necessary to trigger the CodeQL alert for the **user-controlled-sources vulnerability**. Click on **Commit changes**.

   ![github-advisory-database](images/gnat01.png)

   ![github-advisory-database](images/gnat.png)

   >**Note:** Uncomment the lines of code.   

1. Click on commit to commit these changes to a **new branch** then click on **Propose changes.**  
 
   ![github-advisory-database](images/go.png)

1. Open a *Pull request* into the **main** branch, click on **Create pull request.**

   ![github-advisory-database](images/sec4.png)

1. Click on **Create pull request** again.   

   ![github-advisory-database](images/gp.png)
     
1. It may take a moment for the Action to trigger and the **Merge pull request** button will display green until the Action kicks off.
   
   - CodeQL flag this pull request with a _Query built from user-controlled sources_ finding.

     ![clear-text-logging-finding](images/prfailat01.png)

     ![clear-text-logging-finding](images/prfailat02.png)

## Task 4: Turn on GitHub Advanced Security for Organization.

### About the GitHub-recommended security configuration

The GitHub-recommended security configuration is a collection of enablement settings for GitHub's security features that is created and maintained by subject matter experts at GitHub. The GitHub-recommended security configuration is designed to successfully reduce the security risks for low- and high-impact repositories. We recommend you apply this configuration to all the repositories in your organization.

   Refer to the link for more information. [About the GitHub-recommended security configuration](https://docs.github.com/en/enterprise-cloud@latest/code-security/securing-your-organization/enabling-security-features-in-your-organization/applying-the-github-recommended-security-configuration-in-your-organization)

In this task, you will configure and apply security settings across all repositories within an organization. 
 
1. On the **Home** page, click on your **profile** icon in the top right corner.

   ![Profile Menu](./images/orgprofile.png)

2. Select **Your organizations** from the dropdown menu.

   ![Your Organizations](./images/org.png)

3. Choose **ghas-bootcamp-xxxx-xx-xx-cloudlabsxxx** from the list of organizations.

   ![Select Organization](./images/ghas-exercise1-4.png)

4. Navigate to the repository and click on the **Settings** tab.

   ![Repository Settings](./images/mod1org.png)

5. In the Settings menu, click-on **Configurations** under Code Security under Security section.

   ![Code Security Configurations](./images/mod1org1.png)

6. Click on **Apply to** and select **All repositories** to apply the configuration.

   ![Apply to All Repositories](./images/mod1org2.png)

7. Click on **Apply** to confirm the configuration changes, when the prompt appears.

   ![Apply Configuration](./images/advance-scanning7.png)

   > **Note**: If the configuration fails for any of the repositories, it could be due to the existing advanced setup.
   
   > **Note**: Follow the steps below for the repositories where the configuration is failed.

   ![](./images/i9.png)

8. Now navigate to that repository for which the configuration is getting failed.

   ![](./images/i10.png)

9. From left pane select **Code security**, and under Code scanning , click on **setup** and select **Default**.

   ![](./images/i11.png)

10. On the CodeQL default configuration pop up , click on **Enable CodeQL**

    ![](./images/i12.png)

11. Again perform step 1 to 7 to apply configurations for all the repositories.

# Fix code vulnerabilities using Github Copilot Autofix

## Task 5:  Verify Github Autofix is enabled

1. Navigate to your repository.
2. Click on the Settings icon
3. In the left sidebar, find and click on Code security.
4. Under this section, look for Code scanning.
5. Locate the Copilot Autofix option.
6. Ensure that the toggle is set to Enabled. If it’s not, click to enable it.

    ![](./images/autofix.png)

## Task 6: Create a code with potencial security vulnerabilities

1. Create a new file in the repository **ghas-bootcamp-python**
1. Click on the Add file button and select Create new file.
1. Name your file (e.g., app.py).
1. Copy and paste the below code:

In the new file, write the code that includes potential security vulnerabilities. For example, you can use the following code snippet:
Python

 ```
from flask import Flask, request
import sqlite3
import os

app = Flask(__name__)

def init_db():
    conn = sqlite3.connect(':memory:')
    cursor = conn.cursor()
    cursor.execute("CREATE TABLE user (id INTEGER PRIMARY KEY, name TEXT)")
    cursor.execute("INSERT INTO user (name) VALUES ('Alice')")
    cursor.execute("INSERT INTO user (name) VALUES ('Bob')")
    conn.commit()
    return conn

conn = init_db()

@app.route('/user')
def get_user():
    user_id = request.args.get('id')
    cursor = conn.cursor()
    # Introducing SQL Injection vulnerability
    cursor.execute(f"SELECT name FROM user WHERE id = {user_id}")
    user = cursor.fetchone()
    if user:
        return f"User: {user[0]}"
    else:
        return "User not found", 404

if __name__ == '__main__':
    debug_mode = os.getenv('FLASK_DEBUG', 'False').lower() in ['true', '1', 't']
    app.run(debug=debug_mode)

```

6. Commit the changes:
    - Scroll down to the Commit new file section. 
    - Add a commit message describing the changes (e.g., “Add app.py with potential SQL Injection vulnerability”).
    - Choose whether to commit directly to the main branch or create a new branch for this commit.
    - Click on Commit new file to save your changes.

7. Verify the file:
Ensure that the file is created and the code is correctly saved in your repository.

## Task 7: Run a code scan

1. In the repository page, navigate to **Actions** tab to view the workflow.

   ![](images/mod4.1.png) 

2. Check the CodeQL workflow:
   - Look for the CodeQL workflow in the list of workflows.
   - Ensure that the workflow has run automatically after committing the changes.

3. Review the scan results:
   - Click on the latest run of the CodeQL workflow to view the details.
   - Check the results to see if any vulnerabilities were identified.

    ![](images/result.png) 

    > Note: Ensure that the CodeQL scan completes successfully and identifies any vulnerabilities.

## Task 8: Apply autofixes to vulnerabilities

1. Navigate to the Security tab in your repository, and then click on Code scanning.

1. Review the list of vulnerabilities and click on an alert to view details.

   ![](images/scan.png) 

3. If an autofix is available, click on Apply fix to automatically apply the suggested fix.

   ![](images/fix.png) 

4. Commit the changes to your repository.

   ![](images/newchange.png) 

5. Make sure to merge and pull the request.

   ![](images/Merge.png) 

6. Autofix generates an updated text, just click confirm merge

   ![](images/confirmmerge.png) 

   > Note: Ensure that the autofixes are applied successfully and the vulnerabilities are resolved.

## Review

In this lab you have completed the following:

- Learned how CodeQL is different from other static analysis tools
- Turned on the default setup on a Python repository 
- Turned on the advanced setup on a Java repository
- Added some vulnerable code via a pull request and viewed the scan results in the PR.
- Turned on GitHub Advanced Security for Organization
- Create a code with potencial security vulnerabilities
- Apply autofixes to fix code vulnerabilities
