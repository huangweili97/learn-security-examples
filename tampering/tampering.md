# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
insecure.ts does not sanitize user input, especially in the username field. If a user submits a string containing a <script> tag, the server reflects it directly into the HTML output, causing Cross-Site Scripting (XSS). This is a classic script injection vulnerability.

2. Briefly explain how a malicious attacker can exploit them.
An attacker can input a malicious script like:
<script>document.body.innerHTML = "<a href='https://malicious.site'>Gotcha</a>"</script>

When submitted, the server reflects this input into the response. The browser executes it, which alters the page or steals user data. This is an example of client-side content tampering, which can lead to phishing or data leakage.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
secure.ts applies an escapeHTML() function to sanitize user input before saving it in the session. This function escapes special HTML characters like <, >, ", and ', preventing the browser from executing them as scripts. This input sanitization effectively protects against XSS and script injection attacks.