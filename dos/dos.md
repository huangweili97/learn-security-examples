# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
The /userinfo endpoint in insecure.ts lacks any request rate limiting or input validation. An attacker can send a large number of rapid requests, each triggering a database query. This exhausts server resources and prevents legitimate users from accessing the service, leading to a Denial-of-Service (DoS) attack.

2. Briefly explain how a malicious attacker can exploit them.
A malicious attacker can exploit this by writing a script that repeatedly sends requests to /userinfo?id=<value>, even with random or invalid IDs. Since each request is processed and triggers a database query, the server’s CPU and memory resources will be overwhelmed, causing the system to slow down or crash.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
secure.ts uses the express-rate-limit middleware to limit the frequency of requests per IP. In this example, each IP is allowed only one request every 5 seconds. This strategy effectively prevents attackers from sending excessive requests in a short time, thereby protecting server resources and mitigating DoS attacks.