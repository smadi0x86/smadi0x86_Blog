---
description: >-
  YAML is a human-readable data serialization format. It is used for
  configuration files, deployment manifests, and data exchange between languages
  with different data structures.
cover: https://images3.alphacoders.com/109/1091500.jpg
coverY: 0
---

# YAML

## <mark style="color:red;">**Basic Syntax**</mark>

#### <mark style="color:purple;">**Key-Value Pairs**</mark><mark style="color:purple;">:</mark>&#x20;

Simple assignments.

```yaml
name: John Doe
age: 30
```

#### <mark style="color:purple;">**Lists**</mark><mark style="color:purple;">:</mark>&#x20;

Sequential arrays of data.

```yaml
hobbies:
  - reading
  - hiking
  - coding
```

#### <mark style="color:purple;">**Maps**</mark><mark style="color:purple;">:</mark>&#x20;

Nested key-value assignments.

```yaml
person:
  name: Jane
  age: 25
```

#### <mark style="color:purple;">**Comments**</mark><mark style="color:purple;">:</mark>&#x20;

Anything after `#` is a comment.

```yaml
# This is a comment
key: value
```

## <mark style="color:red;">**Advanced Constructs**</mark>

#### <mark style="color:purple;">**Multiline Strings**</mark><mark style="color:purple;">:</mark>

&#x20;Using the `|` character.

```yaml
description: |
  This is a long string
  spanning multiple lines.
```

#### <mark style="color:purple;">**Anchors & Aliases**</mark><mark style="color:purple;">:</mark>&#x20;

Allow for duplicating properties.

```yaml
base: &base
  name: Everyone
person1:
  <<: *base
  age: 30
```

#### <mark style="color:purple;">**Merging Data**</mark><mark style="color:purple;">:</mark>&#x20;

Combine data from two sources.

```yaml
defaults: &defaults
  adapter: postgres
  host: localhost
development:
  database: mydb
  <<: *defaults
```

## <mark style="color:red;">**GitLab's**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**`.gitlab-ci.yml`**</mark>

The `.gitlab-ci.yml` file, employed by GitLab for CI/CD, is an excellent real-world example of YAML's utility.

### <mark style="color:yellow;">**Key Components**</mark>

1. <mark style="color:purple;">**`image`**</mark><mark style="color:purple;">:</mark> Specifies the Docker image for CI/CD tasks.
2. <mark style="color:purple;">**`stages`**</mark><mark style="color:purple;">:</mark> Different stages of CI/CD, like `build`, `test`, and `deploy`.
3. <mark style="color:purple;">**`before_script`**</mark><mark style="color:purple;">:</mark> Commands run before all jobs.
4. <mark style="color:purple;">**`job`**</mark><mark style="color:purple;">:</mark> Defines commands for a specific stage.

#### <mark style="color:purple;">**Example Configuration:**</mark>

```yaml
image: python:3.9

stages:
  - build
  - test
  - deploy

before_script:
  - pip install -r requirements.txt

build:
  stage: build
  script:
    - echo "Building..."
    # Other build commands.

test:
  stage: test
  script:
    - echo "Testing..."
    - pytest

deploy:
  stage: deploy
  script:
    - echo "Deploying..."
    # Deployment commands.
```

## <mark style="color:red;">**Advanced Concepts in**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**`.gitlab-ci.yml`**</mark>

1. <mark style="color:purple;">**`cache`**</mark><mark style="color:purple;">:</mark> Caches directories between runs, like dependencies.
2. <mark style="color:purple;">**`variables`**</mark><mark style="color:purple;">:</mark> Pre-defined environment variables.
3. <mark style="color:purple;">**`tags`**</mark><mark style="color:purple;">:</mark> Labels assigned to runners to control job execution locations.
