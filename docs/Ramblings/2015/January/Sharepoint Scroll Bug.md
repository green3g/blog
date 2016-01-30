# Fix Sharepoint's Scroll Bug

While working with Sharepoint, I've discovered that quite often, the page will not allow scrolling, and the ribbon functionality built into Sharepoint seems broken. I've found that what appears to be happening is that Sharepoint's javascript function that sets up the page scrolling, and the ribbon is not being called. Microsoft placed this function call in the body onload event, which appears to work in IE and Firefox, but not in Chrome for whatever reason. So this simple script I found on the web fixes the issue completely by checking to see if Sharepoint's function has been called, and if not, call it to set up the page. It uses jQuery's document.ready to ensure the necessary resources have completely loaded.

```html
<script>
$(document).ready(function() {

     var interval;

     // Give SP a chance..
     setTimeout(function () { interval = window.setInterval(loopCheck, 30); }, 120);
});

function loopCheck() {
     try { //typeof may fail on some browsers if _spBodyOnLoadWrapper isn't initialized
          if (typeof (_spBodyOnLoadWrapper) !== "undefined" && _spBodyOnLoadCalled == false)
               _spBodyOnLoadWrapper();

          else
               window.clearInterval(interval);
        }
     } catch (e){
          //log the error if you want
          //console.log("typeof error occurred");

     }
}
</script>
```

[Source](http://withinsharepoint.com/archives/256)
