# Module 06: Code Scanning

### Estimated Duration: 90 Minutes

## Lab Scenario

In this module, we'll cover a series of tasks designed to provide a comprehensive understanding of code scanning in GitHub. 

## Lab Objectives

### Implementing GitHub Advanced Security for Your Organization

In this module, you will learn and perform:

- Task 1: Add some vulnerable code via a pull request and view the scan results in the PR 
- Task 2: Create a code with potential security vulnerabilities
- Task 3: Run a code scan
- Task 4: Apply autofixes to vulnerabilities

## Architecture Diagram

![](../images/arch15a.png)
   
## Task 1: Add some vulnerable code via a pull request and view the scan results in the PR  

### Task 1.1: Pull Request scans and Accurate Findings

In this task, you will learn how to enhance CodeQL's security analysis by enabling advanced query configurations and integrating extended security queries. You will gain experience in modifying CodeQL workflows, committing changes related to security vulnerabilities in code, and understanding how CodeQL will accurately identify specific issues, such as clear-text logging of sensitive information, while filtering out less relevant findings. This will depend your understanding of leveraging CodeQL for more precise and effective security scanning in your codebase.

1. In the **ghas-bootcamp-xxxx-xx-xx-<inject key="Deployment-id" enableCopy="false"/>** organization, click on Repositories from the top navigation pane.

   ![github-advisory-database](../images/T1s1new.png)

1. From the list of repositories click on **ghas-bootcamp-python** to begin working through this module. This repository should have at least 2 code scanning findings with the **Default** and the **Extended** setup in this repository.

   ![github-advisory-database](../images/i6.png)

1. In the **Code (1)** tab of the Python repository, navigate to the **server (2)** folder to open the **routes.py (3)** file.

   ![github-advisory-database](../images/gmnew.png)
  
1. Notice that this part of the code is related to the vulnerabilities that have to do with SQL.

1. Uncomment the lines of code by removing **`#`** from **line 33 to the last line (1)**, and click on **Commit changes.. (2)**

   ![](../images/comment1new.png) 

1. Select **Create a new branch (1)** then click on **Propose changes (2)**.
 
   ![github-advisory-database](../images/T1.1S6new.png)

1. Open a *Pull request* into the **main** branch, click on **Create pull request.**

   ![github-advisory-database](../images/T1.1S7.png)

1. It may take a moment for the Action to trigger and the **Merge pull request** button will display green until the Action kicks off.
   
   - CodeQL flag this pull request with a _Query built from user-controlled sources_ finding.

     ![clear-text-logging-finding](../images/prfailat02a.png)

     ![clear-text-logging-finding](../images/prfailat02b.png) 

Refer to the link for more information: [Triaging code scanning alerts in pull requests](https://docs.github.com/en/code-security/code-scanning/managing-code-scanning-alerts/triaging-code-scanning-alerts-in-pull-requests)

## Task 2: Create a code with potential security vulnerabilities

In this task, you will create a new Python file in the repository containing code with intentional security flaws such as SQL injection.

1. Create a new file in the repository **ghas-bootcamp-python**

1. Click **Add file (1)** and choose **+ Create new file (2)** from the dropdown.

   ![](../images/file1.png)

1. Name your file (e.g., **app.py**).

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
    - Click on **Commit changes** on the top right corner. 
    - Enter **Commit Message** – Type **Create app.py (1)** in the commit message box.
    - Write **Extended Description** – Add a detailed note like **Add app.py with potential SQL Injection vulnerability (2)**.
    - Click the **Commit changes (3)** button to save.
   
      ![](../images/commit1.png)

7. Verify the file: Ensure that the file is created and the code is correctly saved in your repository.

## Task 3: Run a code scan

In this task, you will open the Actions tab, find the CodeQL workflow, and check that it ran after your commit. Then, review the scan results to see if any security issues were found

1. In the repository page, navigate to **Actions** tab to view the workflow.

   ![](../images/action1a.png) 

2. Check the CodeQL workflow:
   - Look for the CodeQL workflow in the list of workflows. 
   - Ensure that the workflow has run automatically after committing the changes.
   - Click on **Push on main**.

     ![](../images/mod6-task3-step2new.png)

3. Review the scan results:
   - Click on the latest run of the CodeQL workflow to view the details.
   - Check the results to see if any vulnerabilities were identified.

     ![](../images/resultasnew.png) 

     > Note: Ensure that the CodeQL scan completes successfully and identifies any vulnerabilities.

## Task 4: Apply autofixes to vulnerabilities

In this task, you will review detected vulnerabilities in your repository and apply autofixes to automatically resolve security issues, then commit and merge the changes to secure your code.

1. Navigate to the **Security and quality (1)** tab in your repository, and then click on **Code scanning (2)**.

   ![](../images/lab6-sq-tab.png)

1. Review the list of vulnerabilities and click on an alert for **app.py** file to view details.

   ![](../images/scana.png) 

3. If an autofix is available, click on **Generate fix** to automatically apply the suggested fix.

   ![](../images/fixa.png)

   > **Note:** It will take approximately a minute to generate the fix.

4. Click on **Commit to new branch** and click **Commit changes** to confirm and commit the changes to your repository.

   ![](../images/newchangea.png) 

   ![](../images/newchangeb.png)

1. Once All checks have been passed then click on **Ready for review**.

   ![](../images/newchangec.png)

5. Make sure to merge and pull the request. Click on **Merge pull request**.

   ![](../images/T4S6.png) 

6. Autofix generates an updated text, just click **Confirm merge**.

   ![](../images/T4S7.png) 

   > Note: Ensure that the autofixes are applied successfully and the vulnerabilities are resolved.

## Summary

In this module, you have completed the following:

- Learned how CodeQL is different from other static analysis tools
- Added some vulnerable code via a pull request and viewed the scan results in the PR.
- Create a code with potential security vulnerabilities
- Run a code scan
- Apply autofixes to fix code vulnerabilities

### You have successfully completed the module. Now, click on **Next >>** from the lower right corner to move on to the next page.
            
 ![Picture1](../images/np7.png)
