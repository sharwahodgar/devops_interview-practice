## Interview Questions & Simple Answers

### [cite_start]1. What is CI/CD? [cite: 20]

**CI/CD** stands for **Continuous Integration** and **Continuous Delivery** (or **Continuous Deployment**). It's a set of practices that automates the entire software development lifecycle.

* [cite_start]**CI (Continuous Integration):** This is the practice of frequently merging code changes into a central repository, where automated tests and builds are run immediately[cite: 11]. *(In our task, this was running the `npm test` script.)*
* [cite_start]**CD (Continuous Delivery/Deployment):** This automates the delivery of validated code to a repository (like DockerHub) or directly to a live environment[cite: 4, 10]. *(In our task, this was building the Docker image and pushing it.)*

***

### [cite_start]2. How do GitHub Actions work? [cite: 21]

**GitHub Actions** is an automation tool built into GitHub. It works by using a YAML file (like our `main.yml`) to define **workflows**.

[cite_start]When an event occurs (like pushing code to `main` [cite: 13][cite_start]), the workflow triggers, and GitHub allocates a virtual machine (called a **runner**) to execute the defined sequence of **jobs** and **steps**[cite: 9, 23].

***

### [cite_start]3. What are runners? [cite: 22]

**Runners** are the virtual servers or machines that execute the steps in your GitHub Actions workflow.

* Think of a runner as the **robot's virtual body**.
* In our task, we used a GitHub-hosted runner (`runs-on: ubuntu-latest`), which is a fresh, temporary Ubuntu virtual machine provided by GitHub to run the build, test, and deployment commands.

***

### [cite_start]4. Difference between jobs and steps. [cite: 23]

* [cite_start]**Job:** A job is a set of **steps** that executes on the same **runner** (virtual machine)[cite: 9, 23]. Jobs run independently of each other by default. *(In our task, we had one job named `build-and-deploy`.)*
* **Step:** A step is a single task or command within a job. [cite_start]It can be a command line action (like `run: npm test`) or an action defined by the community (like `uses: actions/checkout@v4`)[cite: 23].

***

### [cite_start]5. How to secure secrets in GitHub Actions? [cite: 24]

You secure sensitive information (like our DockerHub token) by using **GitHub Secrets**.

* **Process:** Instead of putting the password directly into the `main.yml` file, you store it securely in the **Settings $\rightarrow$ Secrets and variables** section of the repository.
* **Access:** In the workflow file, you access the secret using the syntax `${{ secrets.DOCKER_PASSWORD }}`.
* **Security:** GitHub encrypts these values and ensures they are never printed in the logs, protecting them from exposure.

***

### [cite_start]6. How to handle deployment errors? [cite: 25]

You handle errors in a pipeline by making your steps resilient and adding proper error reporting:

1.  **Debugging:** When a job fails (shows a red X), you immediately check the **Action logs** to identify the specific step and error message (e.g., "Login Failed," "Dockerfile not found").
2.  **Conditional Steps:** Use conditions (like `if: always()`) to run cleanup or notification steps even after a preceding step has failed.
3.  **Rollback:** In a real deployment scenario, you would set up an additional job to automatically revert (rollback) the deployment to the last known working version if the new deployment fails health checks.

***

### [cite_start]7. Explain the Docker build-push workflow. [cite: 26]

[cite_start]This workflow is what we automated to get the image to DockerHub[cite: 10]:

1.  **Build:** The pipeline reads the **`Dockerfile`** to get instructions on how to package the app. It executes these steps (like installing Node.js and dependencies) to create a portable container image.
2.  **Tag:** The image is given a unique identifier (**tag**), like `latest` or a commit SHA.
3.  **Login:** The pipeline logs into the Docker registry (DockerHub) using stored credentials.
4.  [cite_start]**Push:** The final, tagged container image is uploaded (**pushed**) to the repository in DockerHub[cite: 11].

***

### [cite_start]8. How can you test a CI/CD pipeline locally? [cite: 27]

While the full deployment *push* only happens on the GitHub runner, you can test the individual components locally:

1.  **Local Application Test:** You can run `npm start` and `npm test` locally to ensure the application code is correct.
2.  **Local Docker Build:** You can run `docker build -t my-app .` in your terminal to confirm the `Dockerfile` is valid and the image builds successfully **before** pushing it to GitHub. This catches many common errors early.