# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
The /update-role endpoint in insecure.ts lacks proper authentication and authorization. It relies on the userId provided in the request body instead of verifying the currently logged-in user through a secure session or token. This allows an attacker to forge a request with an admin's userId and escalate their privileges by bypassing access control.

2. Briefly explain how a malicious attacker can exploit them.
An attacker can visit the /send-form page in the browser, modify the userId field in the form to use the admin's ID (e.g., 1), and set newRole to "admin". When the form is submitted, the server accepts the request without verifying the real user identity, allowing the attacker to escalate their privileges and become an admin.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
secure.ts uses express-session to implement session-based authentication. Before updating any user’s role, it checks whether the current session has a valid userId and whether the logged-in user has an admin role. Only authenticated admin users are authorized to perform role changes, which effectively prevents privilege escalation attacks.

