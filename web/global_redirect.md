# Global Redirect

A new "world-class" website is claiming to have top-tier security worthy of an international stage. Unfortunately for them, their defenses are far from impressive. Show how easy it is to navigate their weaknesses and gain admin access.

Global Redirect is the easiest of the web challenges at Bearcat CTF and has a very nice and quick solve.

## Website Analysis
- going to the website we can see that there is a simple log in page that wont let us sign in to the website
- inspecting the website we see there's a `<script>` tag sitting in the site's html that handles the log in mechanics of the site

```js
      function loginAuthenticate(){
        var password = document.getElementById("password");
        var hash = sjcl.hash.sha256.hash(password);
        var hexRepresentation = sjcl.codec.hex.fromBits(hash);
        if (hexRepresentation == "2a70282a868c0ca9e6fe5bb5cf2ac2ea6b523062102bada26fb87091d511e3f1"){
          alert("welcome Home Admin");
          window.location = "./0078f62f00305b73de6ccace8f9fc1f68a8f1dcec865d33fcacbaf255ddefaa7";
        }else{
          alert("Incorrect Password. Please Try Again");
        }
        alert(hash);

      }
```

- looking at the code there's a mess of hashing of the given password and checking it against a reference value
- there's no reason to try to figure out what the hash is because the following logic solves the answer for us
- `windows.location` tells us that it's going to redirect the user when the correct password is entered
	- however, it just tells us where it redirects instead of hiding it
- going to `<webserver>/0078f62f00305b73de6ccace8f9fc1f68a8f1dcec865d33fcacbaf255ddefaa7` it gives us the flag!