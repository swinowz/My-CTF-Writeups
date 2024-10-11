
![[2024-04-21_17-29-41.mp4]]
<u><strong><h3>Partie 1 </h3></strong></u>
Objectif : obtenir un billet sans avoir l'argent requis

On s'inscrit, on va acheter un billet et dans les cookies, on modifie **balance** pour avoit assez d'argent pour acheter le billet 
On va dans mes billet, obtenir et on obtient une image qui contient le flag


<u><strong><h3>Partie 2</h3></strong></u>Objectif : Obtenir un billet VIP alorss que celui-ci n'est plus en stock


Dans la même page ( mes billets ) on active le proxy burp, on clique sur "obtenir" et on change le token de l'ancien ticket exemple : 
{
	"token":"Laterale"
}

On modifie, on forward puis on obtient une nouvelle image avec le flag 

{
	"token":"VIP"
}