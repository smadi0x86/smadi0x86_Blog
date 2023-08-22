---
description: >-
  Environment variables keep secrets out of your code, and have different
  settings for different stages of your application (like development, testing,
  and production).
cover: https://images3.alphacoders.com/109/1091500.jpg
coverY: 0
---

# Environment Variables in Development

## <mark style="color:red;">**What are Environment Variables**</mark>

Environment variables are key-value pairs stored outside of your application's configuration that can affect how it runs.&#x20;

#### <mark style="color:purple;">They're often used for:</mark>

* Configuring different environments (development, production, etc.).
* Storing secrets, like API keys, which should not be hard-coded or stored in the source code.
* Parameterizing settings so that they can be changed without altering the application's code.

## <mark style="color:red;">**Setting Environment Variables**</mark>

### <mark style="color:yellow;">**On Your Local Development Machine**</mark>

#### <mark style="color:purple;">**Windows (Command Prompt)**</mark><mark style="color:purple;">:</mark>

```bash
set MY_VARIABLE=my_value
```

#### <mark style="color:purple;">**Windows (PowerShell)**</mark><mark style="color:purple;">:</mark>

```bash
$env:MY_VARIABLE = "my_value"
```

#### <mark style="color:purple;">**Linux/macOS**</mark><mark style="color:purple;">:</mark>

```bash
export MY_VARIABLE=my_value
```

## <mark style="color:red;">**Reading Environment Variables in Code**</mark>

#### <mark style="color:purple;">**Python**</mark><mark style="color:purple;">:</mark>

```python
import os
my_variable = os.environ.get('MY_VARIABLE')
```

#### <mark style="color:purple;">**Node.js**</mark><mark style="color:purple;">:</mark>

```javascript
const myVariable = process.env.MY_VARIABLE;
```

## <mark style="color:red;">**Using**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**`.env`**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**Files for Local Development**</mark>

For local development, instead of setting variables in the command line every time, you can use `.env` files.&#x20;

Tools like `dotenv` for Node.js and `python-decouple` for Python allow you to load environment variables from `.env` files into your application.

#### <mark style="color:purple;">**Example .env file**</mark><mark style="color:purple;">:</mark>

```makefile
DATABASE_URL=postgres://user:password@localhost:5432/mydatabase
SECRET_KEY=mysecretkey
DEBUG=True
```

## <mark style="color:red;">**Environment Variables in Production**</mark>

#### <mark style="color:purple;">In production, environment variables can be set:</mark>

* **Directly on the server**: Use the methods mentioned above.
* **Using a platform's dashboard**: Platforms like Heroku or AWS Elastic Beanstalk allow you to set environment variables via their dashboard.
* **Container Orchestration Systems**: Systems like Kubernetes allow you to set environment variables for your containers in your deployment configurations.

## <mark style="color:red;">**Best Practices**</mark>

1. **Don't Commit Secrets**: Never commit secrets (API keys, database passwords, etc.) to version control. Instead, use environment variables.
2. **Use `.env` files for Local Development Only**: `.env` files are great for local development but shouldn't be used in production settings. Instead, set environment variables directly on your production servers or through your orchestration tool.
3. **Have Clear Documentation**: If you're working on a team, ensure everyone knows what environment variables are required for the application to run and what they're used for.



{% hint style="info" %}
Some environment variables can be set as key-value in softwares such as gitlab & when you push code it automatically identifies these variables
{% endhint %}
