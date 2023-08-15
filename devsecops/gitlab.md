---
description: >-
  GitLab is a complete DevOps platform, delivered as a single application. This
  means you can manage your project from planning to deployment within one
  application.
---

# Gitlab

#### **1. Installing GitLab on Debian:**

**Preparation:**

*   Update and upgrade your system:

    ```bash
    bashCopy codesudo apt-get update && sudo apt-get upgrade
    ```
*   Install necessary dependencies:

    ```bash
    bashCopy codesudo apt-get install -y curl openssh-server ca-certificates
    ```

**GitLab Installation:**

*   Add the GitLab repository and install the package:

    ```bash
    bashCopy codecurl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
    sudo apt-get install gitlab-ce
    ```
*   Configure GitLab:

    ```bash
    bashCopy codesudo gitlab-ctl reconfigure
    ```
* Access GitLab: Navigate to your server's IP address or domain in your web browser. Set your root password when prompted.

#### **2. Your First Project on GitLab:**

* After logging in, click on the "New project" button.
* Provide a name and other details, then "Create Project".

#### **3. Interacting with Your Repository:**

Clone, modify, and push:

```bash
bashCopy codegit clone <repository_url>
cd <repository_name>
echo "# My GitLab Project" > README.md
git add README.md
git commit -m "Initial commit"
git push origin master
```

#### **4. CI/CD with GitLab:**

**Setting Up Basic CI/CD:**

* In your project, create a `.gitlab-ci.yml` file.
* For a simple Node.js application, the CI pipeline can be:

```yaml
yamlCopy codestages:
  - install
  - test

install_dependencies:
  stage: install
  script:
    - npm install

run_tests:
  stage: test
  script:
    - npm test
```

**Setting Up GitLab Runners:**

*   **Installation**:

    ```bash
    bashCopy codecurl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
    sudo apt-get install gitlab-runner
    ```
*   **Registration**:

    ```bash
    bashCopy codesudo gitlab-runner register
    ```

    Follow the prompts to register your runner with your GitLab instance.
*   **Starting the Runner**:

    ```bash
    bashCopy codesudo gitlab-runner start
    ```

#### **5. Deploying to Cloud Services like AWS & Azure:**

**AWS:**

* Add AWS credentials as GitLab environment variables.
* Add a deployment stage in `.gitlab-ci.yml`:

```yaml
yamlCopy codedeploy_aws:
  stage: deploy
  script:
    - aws s3 cp ./dist s3://your-bucket-name/ --recursive
```

**Azure:**

* Add Azure credentials as GitLab environment variables.
* Deploy using `.gitlab-ci.yml`:

```yaml
yamlCopy codedeploy_azure:
  stage: deploy
  script:
    - az storage blob upload-batch -d <destination_container_name> -s <source_directory>
```

#### **6. Collaborative Coding with GitLab:**

**Merge Requests:**

* Push changes to a new branch.
* On GitLab, select "Merge Requests" > "New Merge Request".
* Review, and when ready, merge.

**Issues & Milestones:**

* **Issues**: Used for feature requests, bugs, or tasks.
* **Milestones**: Group issues or merge requests for sprints or versions.

#### **7. GitLab Container Registry:**

Manage Docker images within GitLab:

1.  **Login**:

    ```bash
    bashCopy codedocker login registry.gitlab.com
    ```
2.  **Build & Push**:

    ```bash
    bashCopy codedocker build -t registry.gitlab.com/<username>/<project_name> .
    docker push registry.gitlab.com/<username>/<project_name>
    ```

#### **8. Monitoring with GitLab:**

GitLab provides Prometheus for monitoring. Navigate to the "Operations" > "Metrics" dashboard in your project to view metrics.
