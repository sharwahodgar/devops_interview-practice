## 1. What is Jenkins, and how is it used in CI/CD?

**Jenkins** is an open-source automation server that acts as the "brain" or "orchestrator" of a CI/CD pipeline. 

### Usage in CI/CD:

* **Continuous Integration (CI):** Jenkins monitors your code repository (like GitHub). When a developer commits code, Jenkins automatically triggers a build job to fetch the code, compile it, and run tests. This ensures new code changes integrate without breaking anything.
* **Continuous Delivery/Deployment (CD):** If the CI step passes, Jenkins moves on to packaging the application (like creating your **Docker Image**) and deploying it to a testing or production environment (like deploying your app to **`localhost:8081`**).

Jenkins' job is to **automate the entire software release lifecycle**, saving time and catching bugs early.

---

## 2. What is a Jenkinsfile?

A **Jenkinsfile** is a text file that contains the entire definition of a Jenkins Pipeline. It's written in a Groovy-based Domain Specific Language (DSL).

* **Pipeline as Code:** It allows you to define the entire workflow (the steps and stages) of your CI/CD process as code.
* **Version Control:** By committing the `Jenkinsfile` to your Git repository (like you did in `day2_devops`), you can track changes to your build process just like you track changes to your application code. This provides consistency and reproducibility.

---

## 3. How do you create and configure Jenkins pipelines?

There are two primary ways to create and configure a pipeline:

1.  **Pipeline Script from SCM (Source Code Management):** This is the **standard and recommended** method, and it's how you created your project.
    * You define the pipeline logic in a `Jenkinsfile` and commit it to your GitHub repository.
    * In the Jenkins web interface, you create a new **Pipeline** job and simply point it to the Git repository URL and specify the file path (`Jenkinsfile`). Jenkins pulls the script and executes it.
2.  **Pipeline Script directly in Jenkins:** The script is written and stored directly in the Jenkins web interface. This is discouraged for real projects because the history of the pipeline code is not tracked by Git.

---

## 4. What are some common stages in a Jenkins pipeline?

Pipeline stages represent major blocks of work in the delivery process. Common stages include:

1.  **Checkout/SCM:** Fetches the latest source code from the repository (e.g., pulling from GitHub's `main` branch).
2.  **Build:** Compiles the source code, generates artifacts, or, in the case of your project, uses **Docker** to create a deployable image.
3.  **Test:** Runs automated checks (unit tests, integration tests) to ensure code quality.
4.  **Security Scan:** Checks the code and dependencies for vulnerabilities.
5.  **Deploy:** Pushes the finalized artifact (like your Docker container) to the destination environment (e.g., local server, cloud).

---

## 5. What is the difference between a declarative and scripted Jenkins pipeline?

These are the two syntaxes available for writing a `Jenkinsfile`.

| Feature | Declarative Pipeline | Scripted Pipeline |
| :--- | :--- | :--- |
| **Structure** | **Structured and rigid.** Defined by required blocks like `pipeline { }`, `agent`, `stages`, and `steps`. | **Flexible, Groovy-based.** Code blocks are less restrictive and defined using Groovy control flow logic. |
| **Syntax** | Easier to read, learn, and enforce a standard structure. | More powerful, allowing complex logic (loops, functions, etc.). |
| **Use Case** | Ideal for **simple-to-moderate** CI/CD pipelines (like your project) where readability and standardization are key. | Best for **highly complex** workflows that require advanced programming features and conditional logic. |

Your `Jenkinsfile` used the **Declarative** syntax, which is the modern and preferred approach for most pipelines today because it's easier for teams to maintain.