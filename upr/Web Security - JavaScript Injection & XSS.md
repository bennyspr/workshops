# Web Security: JavaScript Injection & XSS

## Introduction

**Cross-Site Scripting (XSS)** allows attackers to inject malicious code (often JavaScript) into web pages viewed by others. Understanding how XSS works is essential for creating secure web applications.

We’ll explore three types of XSS:
- **Reflected XSS**: The malicious payload is “reflected” back in the HTTP response.
- **Stored XSS**: The malicious payload is **persisted** on the server (e.g., database, file) and served to others.
- **DOM-Based XSS**: The malicious payload is executed purely on the client side via insecure JavaScript/DOM manipulation.

## Prerequisites

- A basic understanding of **HTML**, **JavaScript**, and **HTTP**.
- A minimal PHP environment (e.g., PHP 7+). If you do not have PHP installed, you can still do the DOM XSS example.
- Ability to open `.php` files via a local PHP server. For example, from your terminal, navigate to the folder containing the `.php` files and run `php -S localhost:8080`. Then visit http://localhost:8080 in your browser to view them.

## Setup Instructions

1. Create a new folder on your machine called `xss-workshop`.

2. Inside this folder, create the following files:

   `reflected.php`
   `stored.php`
   `dom.html`

3.  Copy the respective code (provided in each exercise) into these files.

4.  Run a minimal PHP server (if doing the `.php` examples):

    `cd xss-workshop`
    `php -S localhost:8080`

    Then open your browser at http://localhost:8080/. You can access each file by going to:

    - http://localhost:8080/reflected.php
    - http://localhost:8080/stored.php
    - http://localhost:8080/dom.html

## Exercise 1: Reflected XSS (PHP)

### Overview

Reflected XSS occurs when user input is immediately included in the server’s response without proper sanitization. Attackers typically trick users into clicking a link with embedded malicious code.

### Steps

1. **Create the `reflected.php` file:**  

```php
<?php
    // If a 'name' parameter was submitted via GET, store it in a variable.
    // (No sanitization - deliberately vulnerable)
    $name = isset($_GET['name']) ? $_GET['name'] : '';
?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Reflected XSS Example</title>
</head>
<body>
    <h1>Reflected XSS Demo</h1>
    <form method="GET" action="">
        <label for="name">Enter your name:</label>
        <input type="text" id="name" name="name" />
        <button type="submit">Submit</button>
    </form>
    <p>Your input:</p>
    <div>
        <!-- Directly echoing user input (vulnerable) -->
        <?php echo $name; ?>
    </div>
</body>
</html>
```   
    
2. **Open the page in your browser:** 

    http://localhost:8080/reflected.php
    
3. **Test with normal input:**
    
    Type a simple name (e.g., "John") and click Submit. The page will reflect "John" below.

4. **Inject malicious script**:
    
    - In the URL bar (after you submit once), you’ll see something like:
   `http://localhost:8080/reflected.php?name=John` 
        
    - Replace `John` with a script tag, e.g.: 
   `http://localhost:8080/reflected.php?name=<script>alert('Reflected XSS');</script>` 
        
    - Press Enter and observe if an **alert** pops up. If it does, the page is vulnerable to Reflected XSS!

5. **Variations**:
    
   Try `<script>alert("Another test")</script>` and observe how the script is executed in the page’s context.

## Exercise 2: Stored XSS (PHP)

### Overview

Stored XSS (Persistent XSS) occurs when the malicious code is stored on the server (e.g., in a database or file) and then displayed to other users.

### Steps

1.  **Create another PHP file called `stored.php`:**

    
```php
<?php
// Simple text file to 'store' messages (in real life, this might be a database)
$filename = 'messages.txt';

// If a message was submitted, append it to the text file 
if (isset($_POST['message'])) { 
    // Deliberately no sanitization — vulnerable to XSS 
    $message = $_POST['message'];
    // Add a newline so each new message is on a separate line
    file_put_contents($filename, $message . PHP_EOL, FILE_APPEND);
}

// Read all messages from the file into an array 
$messages = file_exists($filename) ? file($filename, FILE_IGNORE_NEW_LINES) : []; ?>

<!DOCTYPE html> 
<html> 
<head> 
<meta charset="UTF-8"> 
<title>Stored XSS Example</title>  
</head> 
<body> 
<h1>Stored XSS Demo</h1>
<form method="POST" action="">
    <label for="msg">Enter a message:</label><br>
    <textarea id="msg" name="message" rows="4" cols="50"></textarea><br><br>
    <button type="submit">Submit</button>
</form>
<hr>
<h2>Messages:</h2>
<div>
    <?php
        // Display each message
        // Note: This is deliberately vulnerable because we're echoing directly
        foreach ($messages as $line) {
            echo "<p>$line</p>";
        }
    ?>
</div>
</body> 
</html>
```

2.  **Open the page** in your browser:

    - http://localhost:8080/stored.php
    
3.  **Add a normal message**:
    
    - Write "Hello, world!" in the text area, then click **Submit**. It should appear in the **Messages** section below the form.

4.  **Add a malicious message**:
    
    - Type: `<script>alert('Stored XSS');</script>`
    - Click **Submit**.
    - The script will be saved into `messages.txt`.

5.  **Refresh or revisit** `stored.php`:
    
    - If the script is executed each time the page is loaded, that confirms a Stored XSS vulnerability.
    
6.  **Experiment**:

    - Try `<img src="invalid.jpg" onerror="alert('Image error XSS')">` 
    - Attempt other HTML/JavaScript injections.

## Exercise 3: DOM-Based XSS (HTML)

### Overview

DOM-Based XSS occurs entirely in the browser. The server might send harmless HTML/JS, but client-side JavaScript changes the DOM based on user input without sanitizing it.

### Steps

1.  **Create the file `dom.html`:**

```html    
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>DOM-Based XSS Example</title>
</head>
<body>
    <h1>DOM-Based XSS Demo</h1>
    <p>
      This page reads data from the URL hash and writes it to the page without sanitization.
    </p>

    <div id="output"></div>

    <script>
        // Grab the part of the URL after the '#' symbol
        let hashContent = window.location.hash.substring(1);
        // Insert it directly into the page (vulnerable usage of innerHTML)
        document.getElementById('output').innerHTML = hashContent; 
    </script>
</body>
</html>
```
    
2.  **Open the file** in your browser:
    
    - If you’re running PHP’s built-in server, go to `http://localhost:8080/dom.html`. Or open `dom.html` directly from your custom web server or file system.

3.  **Add a hash to the URL**:
    
    -   For example: `http://localhost:8080/dom.html#Hello`
    -   You should see “Hello” appear in the `<div id="output"></div>` section.
    
4.  **Inject a script**:
    
    - Modify the URL to `http://localhost:8080/dom.html#<script>alert('DOM XSS');</script>`
    - If the alert pops up, you have a **DOM-Based XSS** vulnerability!

5.  **Experiment**:
    
    - Try HTML tags, e.g.: 
    `dom.html#<img src="invalid.jpg" onerror="alert('Image error XSS')">`
    - Observe how the page executes any code placed in `location.hash`.

## Mitigation Techniques

1.  **Output Encoding / Escaping**
    
    - On the server side, ensure you **HTML-encode** or **escape** user inputs before injecting them into the page.
    - In PHP, use functions like `htmlspecialchars()` to prevent raw script injection.
    
2.  **Use Safe Methods in JavaScript**

    - Avoid using `innerHTML`, `document.write()`, or `eval()` with unsanitized user data.
    - If you must display HTML, consider a sanitization library (like [DOMPurify](https://github.com/cure53/DOMPurify)).
    
3.  **Content Security Policy (CSP)**
    
    - A strong CSP header can reduce the impact of XSS by restricting the sources of executable scripts.
    
4.  **Validate and Sanitize User Input**
    
    - Strip out `<script>` tags and other potentially dangerous content on the server before storing or rendering it.
    
5.  **Use Frameworks with Built-in Protections**
    
    - Modern frameworks (React, Angular, Vue, etc.) often handle encoding automatically.
