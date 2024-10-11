TAGS : #WEB 
En retard, billet non valide pour entrer... 
Codesource script : 
```javascript
<script>
    setInterval(function() {
        var panneau = document.querySelector('.panneau');
        var barre = document.querySelector('.barre-en-fer');
        if (!panneau && barre) {
            barre.remove();
        }
    }, 100);

    document.querySelector('.entrer-dans-le-stade').addEventListener('click', function() {
        window.location.href = '/donnez-moi-mon-ticket-pitie';
    });

    window.onload = function() {
        var cookies = document.cookie.split('; ');
        var fraudeur = cookies.find(row => row.startsWith('fraude='));

        if (fraudeur && fraudeur.split('=')[1] === 'true') {
            var zoneTexte = document.querySelector('.zone-texte');
            zoneTexte.textContent = 'Vous avez essayé de frauder, mais le vigile vous a aperçu et vous a ramené à l\'entrée...';
            zoneTexte.style.display = 'block';
        }
    };
</script>
```
On trouve un autre lien dans le code source : /donnez-moi-mon-ticket-pitie

Sur le code source de cette nouvelle page, on trouve l'ID de notre billet  :  **053HJ28LOS**

On fait donc un curl sur la page de base en faisant une requete post 
```
curl -I "billet.id=053HJ28LOS" -X POST https://en-retard.challenges.404ctf.fr/set_cookie
```
