---
description: >-
  GitLab is a complete DevOps platform, delivered as a single application. This
  means you can manage your project from planning to deployment within one
  application.
cover: https://images3.alphacoders.com/109/1091500.jpg
coverY: 0
---

# Gitlab

## <mark style="color:red;">I</mark><mark style="color:red;">**nstalling GitLab on Debian**</mark>

### <mark style="color:yellow;">**Preparation**</mark>

#### <mark style="color:purple;">Update and upgrade your system:</mark>

```bash
sudo apt-get update && sudo apt-get upgrade
```

#### <mark style="color:purple;">Install necessary dependencies:</mark>

```bash
sudo apt-get install -y curl openssh-server ca-certificates
```

### <mark style="color:yellow;">**GitLab Installation**</mark>

#### <mark style="color:purple;">Add the GitLab repository and install the package:</mark>

```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo apt-get install gitlab-ce
```

#### <mark style="color:purple;">Configure GitLab:</mark>

```bash
sudo gitlab-ctl reconfigure
```

#### <mark style="color:purple;">Access GitLab:</mark>&#x20;

Navigate to your server's IP address or domain in your web browser. Set your root password when prompted.

## <mark style="color:red;">**Your First Project on GitLab**</mark>

* After logging in, click on the "New project" button.
* Provide a name and other details, then "Create Project".

## <mark style="color:red;">**Interacting with Your Repository**</mark>

#### <mark style="color:purple;">Clone, modify, and push:</mark>

```bash
git clone <repository_url>
cd <repository_name>
echo "# My GitLab Project" > README.md
git add README.md
git commit -m "Initial commit"
git push origin master
```

## <mark style="color:red;">**CI/CD with GitLab**</mark>

#### <mark style="color:purple;">**Setting Up Basic CI/CD:**</mark>

* In your project, create a `.gitlab-ci.yml` file.

#### <mark style="color:purple;">For a simple Node.js application, the CI pipeline can be:</mark>

```yaml
codestages:
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

## <mark style="color:red;">**Setting Up GitLab Runners**</mark>

#### <mark style="color:purple;">**Installation**</mark><mark style="color:purple;">:</mark>

```bash
curl -LJO "https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_${arch}.deb"
dpkg -i gitlab-runner_<arch>.deb
```

{% embed url="https://docs.gitlab.com/runner/install/linux-manually.html" %}

#### <mark style="color:purple;">**Registration**</mark><mark style="color:purple;">:</mark>

```bash
sudo gitlab-runner register
```

Follow the prompts to register your runner with your GitLab instance.

#### <mark style="color:purple;">**Starting the Runner**</mark><mark style="color:purple;">:</mark>

```bash
sudo gitlab-runner start
```

#### <mark style="color:purple;">**Viewing Live Logs:**</mark>

```bash
sudo gitlab-runner --debug run
```

This starts the runner in the foreground with live debugging information.

#### <mark style="color:purple;">**Checking Runner Status:**</mark>

```bash
sudo gitlab-runner status
```

#### <mark style="color:purple;">**Stopping & Restarting:**</mark>

```bash
sudo gitlab-runner stop
```

```bash
sudo gitlab-runner restart
```

#### <mark style="color:purple;">**Unregistering a Runner:**</mark>

```bash
sudo gitlab-runner unregister --name [RUNNER_NAME]
```

Replace `[RUNNER_NAME]` with the name or description you provided during the registration.

#### <mark style="color:purple;">**Viewing All Registered Runners:**</mark>

```bash
sudo gitlab-runner list
```

#### <mark style="color:purple;">**Updating the GitLab Runner:**</mark>

```bash
sudo apt-get update
sudo apt-get install gitlab-runner
```

## <mark style="color:red;">**Deploying to Cloud Services like AWS & Azure**</mark>

### <mark style="color:yellow;">**AWS**</mark>

* Add AWS credentials as GitLab environment variables.

#### <mark style="color:purple;">Add a deployment stage in</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`.gitlab-ci.yml`</mark><mark style="color:purple;">:</mark>

```yaml
deploy_aws:
  stage: deploy
  script:
    - aws s3 cp ./dist s3://your-bucket-name/ --recursive
```

### <mark style="color:yellow;">**Azure**</mark>

* Add Azure credentials as GitLab environment variables.

#### <mark style="color:purple;">Deploy using</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`.gitlab-ci.yml`</mark><mark style="color:purple;">:</mark>

```yaml
deploy_azure:
  stage: deploy
  script:
    - az storage blob upload-batch -d <destination_container_name> -s <source_directory>
```

## <mark style="color:red;">**Collaborative Coding with GitLab**</mark>

#### <mark style="color:purple;">**Merge Requests:**</mark>

* Push changes to a new branch.
* On GitLab, select "Merge Requests" > "New Merge Request".
* Review, and when ready, merge.

#### <mark style="color:purple;">**Issues & Milestones:**</mark>

* **Issues**: Used for feature requests, bugs, or tasks.
* **Milestones**: Group issues or merge requests for sprints or versions.

## <mark style="color:red;">**GitLab Container Registry**</mark>

### <mark style="color:yellow;">Manage Docker images within GitLab</mark>

#### <mark style="color:purple;">**Login**</mark><mark style="color:purple;">:</mark>

```bash
docker login registry.gitlab.com
```

#### <mark style="color:purple;">**Build & Push**</mark><mark style="color:purple;">:</mark>

```bash
docker build -t registry.gitlab.com/<username>/<project_name> .
docker push registry.gitlab.com/<username>/<project_name>
```

## <mark style="color:red;">**Monitoring with GitLab**</mark>

GitLab provides Prometheus for monitoring. Navigate to the "Operations" > "Metrics" dashboard in your project to view metrics.
