# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
insecure.ts is vulnerable to session spoofing because it does not enforce secure cookie attributes like httpOnly and sameSite. This allows attackers to steal session cookies through malicious scripts or forge requests via Cross-Site Request Forgery (CSRF), enabling them to impersonate legitimate users and perform sensitive operations.


2. Briefly explain different ways in which vulnerability can be exploited.
Attackers can exploit spoofing in two main ways:

Stealing Cookies: A malicious page like mal-steal-cookie.html can use JavaScript to read cookies from the browser and display them in the console. Since httpOnly is set to false, cookies are accessible via scripts.

Cross-Site Request Forgery (CSRF): Another page like mal-csrf.html can automatically send a POST request to insecure.ts, using the logged-in user's session to perform actions without consent.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
secure.ts prevents spoofing by using a secure session configuration:

httpOnly: true: Prevents JavaScript from accessing cookies, protecting them from being stolen.

sameSite: true: Prevents cookies from being sent in cross-site requests, mitigating CSRF attacks.

These settings significantly improve session security and prevent unauthorized user impersonation.