# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
The /userinfo endpoint in insecure.ts is vulnerable to NoSQL injection. This is because the user-provided username is directly used in the database query without any type validation or input sanitization. An attacker can inject malicious query objects to bypass authentication or alter the query logic.

2. Briefly explain how a malicious attacker can exploit them.
An attacker can exploit the vulnerability by crafting a request like:
http://localhost:3000/userinfo?username[$ne]=
This input is interpreted as { username: { $ne: "" } }, which matches any user whose username is not an empty string. As a result, sensitive user information may be disclosed, leading to information disclosure.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
secure.ts employs the following defense techniques to prevent the information disclosure vulnerability:
a. Type validation: Ensures the username is a string to block injected arrays or objects.
b. Input sanitization: Removes all non-alphanumeric characters (like $, [) using .replace(/[^\w\s]/gi, '') to prevent injection of special query syntax.
c. Proper error handling: Returns a 400 status code for invalid input and avoids exposing internal system details.