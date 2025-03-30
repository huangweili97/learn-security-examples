# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
The service in insecure.ts lacks any form of authentication and does not log user actions. This means any user can send messages or access data anonymously, and the system has no way to trace who performed the actions. A malicious user can carry out harmful activities and later deny them, as there is no evidence. This is a classic repudiation vulnerability.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.
In secure.ts, a logging mechanism is introduced. Each request—including timestamp, request method, and IP address—is written to a server.log file. This ensures that even if a user tries to deny their actions, the system retains evidence. Additionally, access to send and retrieve messages is tied to basic authentication logic, improving accountability and traceability.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
secure.ts adopts the Audit Logging design pattern. This pattern focuses on recording important user actions, access timestamps, and IP addresses. When suspicious or malicious activities occur, administrators can trace them back using the logs. This mechanism is essential for legal compliance, accountability, and forensic analysis.