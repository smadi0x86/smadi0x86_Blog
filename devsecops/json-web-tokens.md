---
description: >-
  JSON Web Tokens (JWT) can be encoded into a URL-friendly string format, which
  can be signed for authentication and/or encrypted for protection.
cover: https://images3.alphacoders.com/109/1091500.jpg
coverY: 0
---

# JSON Web Tokens

## <mark style="color:red;">**Structure of JWT**</mark>

#### <mark style="color:purple;">A JWT typically looks like</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`xxxxx.yyyyy.zzzzz`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">and is divided into:</mark>

* **Header**: Contains token type and the signing algorithm being used.
* **Payload**: Contains the claims which are statements about the entity (typically, the user) and additional data.
* **Signature**: To prevent tampering, the header and payload are combined and encrypted.

***

## <mark style="color:red;">**Setup**</mark>

#### <mark style="color:purple;">Before creating and verifying JWTs, you'll need to install required libraries. Here's how you can do it for a Node.js project:</mark>

```bash
npm install jsonwebtoken
```

***

## <mark style="color:red;">**Creating a JWT**</mark>

```javascript
const jwt = require('jsonwebtoken');

// Sample payload
const payload = {
    userId: 12345,
    role: "admin"
};

// Signing the token
const secretKey = "yourSuperSecretKey";
const token = jwt.sign(payload, secretKey, { expiresIn: '1h' });

console.log(token);
```

***

## <mark style="color:red;">**Verifying a JWT**</mark>

```javascript
const jwt = require('jsonwebtoken');

// Assuming token is received from a client or some source
const receivedToken = "received_token_here";

// Verifying the token
const secretKey = "yourSuperSecretKey";

try {
    const decoded = jwt.verify(receivedToken, secretKey);
    console.log(decoded);
} catch (error) {
    console.error("Invalid token:", error.message);
}
```

***

## <mark style="color:red;">**Using JWT for Authentication**</mark>

A common use case for JWT is authentication. Upon user login, a token is generated and sent to the client. For subsequent requests, the client sends this token back, and the server verifies it.

#### <mark style="color:purple;">**Generating Token on Login**</mark><mark style="color:purple;">:</mark>

```javascript
app.post('/login', (req, res) => {
    // Validate user credentials (e.g., against a database)
    // ...

    // If valid, generate JWT
    const token = jwt.sign({ userId: user.id }, secretKey, { expiresIn: '1h' });

    res.json({ token });
});
```

#### <mark style="color:purple;">**Verifying Token for Protected Routes**</mark><mark style="color:purple;">:</mark>

```javascript
app.get('/protected', (req, res) => {
    const token = req.headers.authorization;

    try {
        const user = jwt.verify(token, secretKey);
        // Continue processing...
    } catch {
        res.status(401).send("Unauthorized");
    }
});
```

***

## <mark style="color:red;">**Best Practices**</mark>

* **Keep it Secret, Keep it Safe**: Never expose your secret key. Use environment variables or secret management tools.
* **Short Lifespan**: Use short-lived JWTs to reduce potential misuse.
* **Use HTTPS**: Always transfer JWTs over a secure layer to prevent Man-In-The-Middle attacks.
* **Handle Expiry Gracefully**: Implement mechanisms on the client side to handle token expiry and re-authentication.

***

## <mark style="color:red;">**Libraries for Other Languages**</mark>

#### <mark style="color:purple;">JWT is supported in many languages. Some popular libraries include:</mark>

* Python: `PyJWT`
* Ruby: `ruby-jwt`
* Java: `java-jwt`
