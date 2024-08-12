# Module 04: Code Scanning

## Lab Scenario

 In this lab, we'll cover a series of tasks designed to provide a comprehensive understanding of code scanning in GitHub. 

## Lab Objectives
In this lab, you will learn and perform:
- Introduction to Code Scanning
- What is CodeQL and how is it different from other static analysis tools? 
- Task 1: Turn on the default setup on a Python repository
- Task 2: Turn on advanced setup on a Java repository
- Task 3: Add some vulnerable code via a pull request and view the scan results in the PR
- Task 4: CodeQL Query Operations in Visual Studio  

## Estimated timing: 40 minutes

## Architecture Diagram

   ![Picture1](./images/ar04.png)
   
## Introduction to Code Scanning 

Code scanning in GitHub is a powerful feature designed to enhance the security of your software projects by automatically identifying and alerting you to potential security vulnerabilities in your codebase. Leveraging advanced static analysis techniques, and code scanning helps detect security flaws, bugs, and other issues early in the development process, enabling developers to address them proactively before they escalate into larger problems. 


## What is CodeQL and how is it different from other static analysis tools?

1. Code Scanning, powered by the CodeQL engine, performs thorough static analysis by accessing source code and integrating with the build process for compiled languages (or simulating compilation for interpreted languages). This approach ensures precise mapping of data flow and the ability to differentiate between remote and local sources. 

1. The fundamental difference is that all of the information about the application is aggregated in a relational database that allows for tracing complete data flows across the entire application.

1. For `compiled` languages, the CodeQL engine running under the hood of the Code Scanning process will hook into the compiler at build time. The CodeQL engine will then listen for the creation of data flows by the compiler, such as linkers and callbacks, and map those data flows as nodes in the database -- aptly called `DataFlow` nodes.
1.  This process allows CodeQL to avoid false positive vulnerability findings from dead code that has no existing dataflows. This is a common problem with other Static Analysis tools that do not have access to the compiler and instead rely on pattern matching and other techniques to identify vulnerabilities.
1. Once the data flow analysis is complete, an extraction of the code is then performed. Every variable, expression (combination or modification of variable(s)), method/function/class declaration, etc. is extracted as individual nodes in the database.

1. CodeQL then performs analysis by querying the database for _Remote_ flow sources that lead to sinks (where data is stored or executed) in ways that are exploitable and are otherwise not sanitized as part of that data flow.

1. For `interpreted` languages, like Javascript and Python, the CodeQL engine performs a depth-first, recursive extraction of the code where `DataFlow` nodes are created from things like `return` statements and passing variables from one function to another. We can gain a comprehensive view of the application and avoid flagging false positive vulnerabilities in code that is never called or executed.

## Task 1: Turn on the default setup on a Python repository

## Default Setup

1. In the **github-bootcamp-#####-cloudlabsaiuser-####** organization, click on repositories from the top navigation pane.

   ![github-advisory-database](images/getrepo.png)

1. From the list of repositories click on **ghas-bootcamp-python** to begin working through this module. This repository should have at least 2 code scanning findings with the **Default** and the **Extended** setup in this repository.

   ![github-advisory-database](images/gd.png)

1. In the GitHub repository navigate to **Settings** from the top navigation pane and click on **Code Security and Analysis** under Security.

   ![github-advisory-database](images/ga.png)

   >**Note**: Zoom out of the page if you are unable to see the **Setting**.

1. Now on the **GitHub Advanced Security**, click on **Enable**.
 
   ![Picture1](./images/ghadvanceenablae.png)

1. Scroll down and you will find the **Code Scanning** option. Click on the **Setup** button and we have two options to configure code scanning. That is **Default** and **Advanced** as depicted in the screenshot below. For now, click on **Default**

   ![github-advisory-database](images/gb.png)

1. Leave the query suite on Default and click the **Enable CodeQL** button.

   ![github-advisory-database](images/ge.png)

1. While that runs (It'll take approx. 5 minutes to get it enabled), let us see the difference between the two query suites.
	  - The Default query suite (also known as the `code-scanning` query suite in the _Advanced_ setup) has a less than 10% False Positive rate from findings within the Open Source ecosystem. We focus very heavily on providing true positive findings that are remotely exploitable, and this suite is the most "dialed in" in terms of findings.
	  - The Extended query suite (also known as the `security-extended` query suite in the _Advanced_ setup) has a less than 30% False Positive rate from findings within the Open Source ecosystem. You will find several interesting queries pulled into this suite, including _Memory Exploitation_ findings for C/C++ and other slightly more niche security vulnerabilities in other languages.

1. After you've clicked the **Enable CodeQL** button, go to the _Actions_ tab to confirm that the initial scan has kicked off. The scan should take a couple of minutes. You can also check more configuration and details inside the the github action run.

   ![github-advisory-database](images/gf.png)

## Task 2: Turn on advanced setup on a Java repository

## Advanced Setup

 Now, let us enable  Advanced set-up in a different repository.

1. In the **github-bootcamp-#####-cloudlabsaiuser-####** organization, click on repositories from the top navigation pane.

   ![github-advisory-database](images/getrepo.png)

1. From the list of repositories click on **ghas-bootcamp-java** to begin working through this module. 

   ![github-advisory-database](images/gg.png)

1. Next, we're going to enable Advanced Setup for one of our compiled language repositories by going back to the `ghas-bootcamp-java` repository.

1. In the GitHub repository navigate to **Settings** from the top navigation pane and click on **Code Security and Analysis** under Security.

   ![github-advisory-database](images/ga.png)

1. Now on the **GitHub Advanced Security**, click on **Enable**.
 
   ![Picture1](./images/ghadvanceenablae.png)

1. Scroll down and you will find the **Code Scanning** option. Click on the **Setup** button and click on **Advanced**

   ![github-advisory-database](images/gh.png)

   >**Note:** If it is already enabled, click on **Switch to Advanced**

1. Now you are redirect to the codeql.yml file in github/ workflows directory, and we need to copy the string **queries: security-extended (1)** from the **codeql.yml** file  and append this to a new line at number 67 under **Initialize CodeQL** section , which will read as **queries: security-extended (2)** as shown in the below screenshot. Commit these changes to your **main branch (3)**. Make sure you remove '#' to uncomment it (2).

   ![github-advisory-database](images/gi.png)
  
1. Now,  go to the **Actions** tab to confirm the CodeQL action is running.

   ![github-advisory-database](images/gj.png)
   
1. You can also review the code and results of that action.

   ![github-advisory-database](images/runactions.png)

   
  > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
	
  - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
    >**Note:** Upon clicking the **Validate** button for this exercise, you'll receive a prompt to input your Organization name. Provide your **Organization name** which looks like **github-bootcamp-cloudlabsuser-xxx**.
    
    >**Note:** Make sure to update the name of your organization, **github-bootcamp-cloudlabsuser-xxx**.
    
  	 ![github-advanced-security](images/ghas-exercise1-8.png)
   
  - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
  - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="fdc513c6-e817-48a5-aec1-8875b2e7b0a4" />

## Task 3: Add some vulnerable code via a pull request and view the scan results in the PR  

### Task 3.1: Pull Request scans and Accurate Findings

1. Next, we're going to enable _Advanced setup_ for one of our interpreted language repositories by going back to the `ghas-bootcamp-python` repository. Follow the instructions from the **Getting Started** section to go to the `ghas-bootcamp-python` repository.

1. Now, head over to **Settings** -> **Code security and analysis** -> scroll down to **Code scanning** and click the `...` and then click _Switched to advanced_. This will prompt us to turn off the existing CodeQL workflow to avoid duplicating Action runs.Click on **Disable CodeQL**

   ![github-advisory-database](images/gk.png)

   ![github-advisory-database](images/gl.png)

1. We are going to make similar updates to the `codeql.yml` file as we did in the `Advanced Setup` section, we need to copy the string **queries: security-extended (1)** from the **codeql.yml** file  and append this to a new line at number 67 under **Initialize CodeQL** section , which will read as **queries: security-extended (2)** as shown in the below screenshot. Commit these changes to your **main branch (3)**. Make sure you remove '#' to uncomment it (2).

   ![github-advisory-database](images/git1.png)


1. In the Code tab of the Python repository, navigate to the server folder to open the `routes.py` file and scroll down to **Line 40**.

   ![github-advisory-database](images/gm.png)
 
  
1. Notice that this part of the code is related to the vulnerabilities that have to do with SQL.

1. Uncomment the Python code starting from **line 38** until the end of the code. 

   ![github-advisory-database](images/gn.png)

1. Click on commit to commit these changes to a new branch,  click on **Create pull request**, open a **Pull request** into the **main** branch, and click on **Create pull request** again.
 
   ![github-advisory-database](images/go.png)

   ![github-advisory-database](images/gp.png)
     
1. Because our main branch isn't a `Protected Branch`, it may take a moment for the Action to trigger and the **Merge pull request** button will display green until the Action kicks off.
 - CodeQL does not flag this pull request with a _Query built from user-controlled sources_ finding. But why?

1. Go to the **Files changed** tab and review the code we added.
  - What we find on **Line 54** is a **`val`** assignment calling **`login.objects.raw`**, which does not exist as a function in this project.
  - While other static analysis tools would likely have marked **Line 50** as a SQL injection, the CodeQL analysis performed as part of code scanning correctly marks this as a **`Clear-text logging of sensitive information`**. This is the power of CodeQL in action - accurately tracing dataflows and identifying security vulnerabilities in your code without all of the noise.

    ![clear-text-logging-finding](images/prfail.png)

## Task 4: CodeQL Query Operations in Visual Studio

1. Start **Visual Studio Code** from the desktop.

    ![Picture1](./images/vscode1.jpg)

1. In the Visual Studio Code, Click on **Extensions**, search and select the **CodeQL**, click on **install**.

    ![Picture1](./images/vscodeql1.jpg)

1. In the Visual Studio Code terminal, click on **(...)** (1) and select the **Terminal** (2) menu, select **New Terminal** (3). The terminal window usually opens in the lower half of your screen.

    ![Picture1](./images/terminal.png)

1. Change the location to **Desktop** for easy navigation.

    ```
    cd Desktop
    ```
    
1. Clone the repository by running the following command:

      >**Note**: Make sure to update the name of your organization, **github-bootcamp-xxxx-cloudlabsuserxxx**.

      ```
      git clone --recursive https://github.com/github-bootcamp-xxxx-cloudlabsuserxxx/ghas-bootcamp-python.git
      ```

    ![Picture1](./images/clonerepo.png)
	>**Note**: If a pop up appears to login, click on Login from borwser and Sign in with GitHub Credentials and on the Authorize GitHub for VS Code page click on the Authorize Visual-Studio-Code.

    ![Picture1](./images/clonerepo4.png)
   
1. Go to the **Explorer** in the upper left corner, select **Open Folder**, select the folder you cloned from the **Desktop**, and then click **Select Folder**.

    ![Picture1](./images/clonerepo1.png)

1. Select **Yes, I trust the authors**.

    ![Picture1](./images/clonerepo2.png)

1. Go to **Account** icon from bottom and click on **Sign in with GitHub to use CodeQL**. Please note it might take some time to get the extension updated with code, If you get any pop-up select yes. 

    ![Picture1](./images/clonerepo3.png)

1. Sign in with GitHub Credentials and on the **Authorize GitHub for VS Code** page click on the **Authorize Visual-Studio-Code.**

    ![Picture1](./images/clonerepo4.png)

1. If you get the pop-up, The site is trying to open the Visual Studio Code then click on **Open**. It will navigate back to the Visual Studio.

    ![Picture1](./images/clonerepo5.png)

1. Go to the **CodeQL** extension and move the cursor over the **Database**, click on the **GitHub** icon.

    ![Picture1](./images/clonerepo6.png)  

1. Add the Database by the following command and press enter.

    >**Note**: Make sure to update the name of your organization, **github-bootcamp-xxxx-cloudlabsuserxxx**.

    ```
    https://github.com/github-bootcamp-xxxx-cloudlabsuserxxx/ghas-bootcamp-python
    ```

    ![Picture1](./images/clonerepo7.png)

1. Go to the **Queries** section, click on **Create query** option and select **Python** as your target query language.

    ![Picture1](./images/clonerepo8.png)

1. It will show up the location for query, press **enter**.

    ![Picture1](./images/clonerepo9.png)

1. It will generate the simple query, click on the **Run**  from right corner (1) and analyze the result.

    ![Picture1](./images/clonerepo10.png)

1. Now, replace the existing code with the provided code snippet. Then, click on the **Run** button to execute it.

  - This CodeQL query identifies named entities (special functions, built-in functions, modules, and files) in a Python codebase from a specified list of names ("foo", "baz", "main", "os", "sys", "re"). It checks if each name matches a specific type ("special", "builtin", "module", "file") and selects them, ordering the results by name and kind. 

	  ```
	  import python
	import semmle.python.types.Builtins
	
	predicate named_entity(string name, string kind) {
	  exists(Builtin::special(name)) and kind = "special"
	  or
	  exists(Builtin::builtin(name)) and kind = "builtin"
	  or
	  exists(Module m | m.getName() = name) and kind = "module"
	  or
	  exists(File f | f.getShortName() = name + ".py") and kind = "file"
	}
	
	from string name, string kind
	where
	  name in ["foo", "baz", "main", "os", "sys", "re"] and
	  named_entity(name, kind)
	select name, kind order by name, kind
	  ```

    ![Picture1](./images/module4task4code1.png)

17. This results helps to understand the classification and usage of these entities in the code.

    ![Picture1](./images/module4task4code2.png)

Please feel free to go through the document for further understanding:[Code Security](https://docs.github.com/en/code-security) and [CodeQL Documentation](https://codeql.github.com/docs/)

## Review

In this lab you have completed the following:

- Learnt how CodeQL is different from other static analysis tools
- Turned on the default setup on a Python repository 
- Turned on the advanced setup on a Java repository
- Added some vulnerable code via a pull request and viewed the scan results in the PR.  
