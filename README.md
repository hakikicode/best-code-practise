# best-code-practise
For review on input validation, Output Encoding:, error handling, Authentication and Authorization, Session Management, Cryptographic Practices, code review.

### Common Secure Coding Practices with HTML5 Examples

1. **Input Validation**
   - **Explanation**: Ensuring that all user inputs are verified and sanitized before processing helps prevent various injection attacks, such as SQL injection and cross-site scripting (XSS).
   - **Example**: Using HTML5 attributes to validate email input in a form.
   - **Code Snippet**:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Input Validation Example</title>
     </head>
     <body>
         <form>
             <label for="email">Email:</label>
             <input type="email" id="email" name="email" required>
             <button type="submit">Submit</button>
         </form>
     </body>
     </html>
     ```

2. **Output Encoding**
   - **Explanation**: Encoding outputs ensures that data is interpreted correctly by browsers and applications, preventing attacks like XSS.
   - **Example**: Using JavaScript to encode user input before displaying it in an HTML element.
   - **Code Snippet**:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Output Encoding Example</title>
     </head>
     <body>
         <div id="output"></div>

         <script>
             function encodeHTML(str) {
                 return str.replace(/&/g, '&amp;')
                           .replace(/</g, '&lt;')
                           .replace(/>/g, '&gt;')
                           .replace(/"/g, '&quot;')
                           .replace(/'/g, '&#039;');
             }

             const userInput = '<script>alert("XSS")</script>';
             const safeOutput = encodeHTML(userInput);
             document.getElementById('output').innerHTML = safeOutput;
         </script>
     </body>
     </html>
     ```

3. **Authentication and Authorization**
   - **Explanation**: Using strong authentication methods (e.g., multi-factor authentication) and enforcing strict authorization checks ensures that only authenticated and authorized users can access specific resources and perform actions.
   - **Example**: HTML and JavaScript for role-based access control (RBAC).
   - **Code Snippet**:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Authentication and Authorization Example</title>
     </head>
     <body>
         <div id="content"></div>

         <script>
             const userRole = 'admin';

             if (userRole === 'admin') {
                 document.getElementById('content').innerHTML = 'Access granted to admin panel.';
             } else {
                 document.getElementById('content').innerHTML = 'Access denied.';
             }
         </script>
     </body>
     </html>
     ```

4. **Error Handling**
   - **Explanation**: Handling errors gracefully and avoiding the exposure of sensitive information prevents attackers from gaining insights into the system's internals.
   - **Example**: JavaScript for logging errors and showing a generic error message.
   - **Code Snippet**:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Error Handling Example</title>
     </head>
     <body>
         <div id="message"></div>

         <script>
             try {
                 // some code that might raise an exception
                 let result = 10 / 0;
             } catch (error) {
                 // log the error details securely (here we log to the console, but in a real app, log to a secure server)
                 console.error('Error:', error);

                 // show a generic error message to users
                 document.getElementById('message').innerHTML = 'An error occurred. Please try again later.';
             }
         </script>
     </body>
     </html>
     ```

5. **Session Management**
   - **Explanation**: Secure session management practices help protect against session hijacking and fixation attacks. This includes secure creation, management, and termination of user sessions.
   - **Example**: Using cookies to manage sessions securely.
   - **Code Snippet**:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Session Management Example</title>
     </head>
     <body>
         <button id="loginButton">Login</button>
         <button id="logoutButton">Logout</button>
         <div id="status"></div>

         <script>
             document.getElementById('loginButton').addEventListener('click', function() {
                 document.cookie = "sessionToken=abc123; Secure; HttpOnly";
                 document.getElementById('status').innerHTML = 'Logged in!';
             });

             document.getElementById('logoutButton').addEventListener('click', function() {
                 document.cookie = "sessionToken=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
                 document.getElementById('status').innerHTML = 'Logged out!';
             });
         </script>
     </body>
     </html>
     ```

6. **Cryptographic Practices**
   - **Explanation**: Using strong, industry-standard cryptographic algorithms and securely managing keys ensures the confidentiality, integrity, and authenticity of data.
   - **Example**: Using the Web Crypto API for data encryption and decryption.
   - **Code Snippet**:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Cryptographic Practices Example</title>
     </head>
     <body>
         <button id="encryptButton">Encrypt</button>
         <button id="decryptButton">Decrypt</button>
         <div id="output"></div>

         <script>
             async function encryptData(data, key) {
                 const encodedData = new TextEncoder().encode(data);
                 const encryptedData = await crypto.subtle.encrypt({name: "AES-GCM", iv: new Uint8Array(12)}, key, encodedData);
                 return encryptedData;
             }

             async function decryptData(encryptedData, key) {
                 const decryptedData = await crypto.subtle.decrypt({name: "AES-GCM", iv: new Uint8Array(12)}, key, encryptedData);
                 return new TextDecoder().decode(decryptedData);
             }

             async function generateKey() {
                 const key = await crypto.subtle.generateKey({name: "AES-GCM", length: 256}, true, ["encrypt", "decrypt"]);
                 return key;
             }

             document.getElementById('encryptButton').addEventListener('click', async function() {
                 const key = await generateKey();
                 const data = "Secret message";
                 const encryptedData = await encryptData(data, key);
                 document.getElementById('output').innerHTML = `Encrypted data: ${new Uint8Array(encryptedData)}`;
                 // Store the key securely
                 window.encryptionKey = key;
             });

             document.getElementById('decryptButton').addEventListener('click', async function() {
                 const key = window.encryptionKey;
                 if (!key) {
                     document.getElementById('output').innerHTML = 'No key found.';
                     return;
                 }
                 const encryptedData = new Uint8Array(document.getElementById('output').textContent.replace('Encrypted data: ', '').split(',').map(Number)).buffer;
                 const decryptedData = await decryptData(encryptedData, key);
                 document.getElementById('output').innerHTML = `Decrypted data: ${decryptedData}`;
             });
         </script>
     </body>
     </html>
     ```

By following these secure coding practices, developers can significantly reduce the risk of vulnerabilities and enhance the overall security of their applications.
