# ![](ressources/logo.jpeg)

# Bases de la programmation orientée objet

### IUT Montpellier-Sète – Département Informatique

* **Cours:** [M2103](http://cache.media.enseignementsup-recherche.gouv.fr/file/25/09/7/PPN_INFORMATIQUE_256097.pdf) - support [ici](https://github.com/IUTInfoMontp-M2103/Ressources)
* **Enseignants:** [Marin Bougeret](mailto:marin.bougeret@umontpellier.fr), [Romain Lebreton](mailto:romain.lebreton@umontpellier.fr), [Sophie Nabitz](mailto:sophie.nabitz@univ-avignon.fr), [Victor Poupet](mailto:victor.poupet@umontpellier.fr), [Petru Valicov](mailto:petru.valicov@umontpellier.fr)
* Le [forum Piazza](https://piazza.com/class/kjifrxy1n0i3xa) de ce cours pour poser vos questions
* [Email](mailto:petru.valicov@umontpellier.fr) pour une question d'ordre privée concernant le cours.
* Le [sujet du TP](http://pageperso.lis-lab.fr/~petru.valicov/Cours/M2103/TP4.pdf) en format .pdf téléchargeable et imprimable.

<!--Avant de démarrer le TP, vérifiez que vous n'avez pas atteint votre quota d'espace de stockage autorisé :

* placez-vous dans votre `$HOME` et utilisez les commandes suivantes :
    * `du -sh` pour voir combien d'espace vous avez déjà utilisé
    * `du -sh *` pour voir combien d'espace vous avez déjà utilisé pour chaque fichier (sans fichiers cachés)
    * `du -sch .[!.]* *` pour voir combien d'espace vous avez déjà utilisé pour chaque fichier, y compris les fichiers cachés
* Supprimez les fichiers inutiles.
* Pour éviter des problèmes durant vos TPs d'informatique, vous devriez toujours **garder 300-400 Mo d'espace libre**.
-->

### Consignes
- Sauf indication contraire, **tous** les attributs que vous allez déclarer dans ce TP (et dans les TPs qui suivent) doivent être privés (`private`).
- A priori, la plupart des méthodes devraient être déclarées publiques (`public`). Vous pouvez tout de même déclarer et utiliser des méthodes `private` du moment qu'elles vous sont utiles et que votre programme fonctionne correctement.
- Pensez à respecter les conventions de nommage *Java* (vues en [cours](http://pageperso.lis-lab.fr/~petru.valicov/Cours/M2103/BPOO_Generalites_x4.pdf) ou disponibles sur le site d'Oracle).
- **Sauf indication contraire, vous ne devrez pas modifier la signature des méthodes et des attributs des classes qui vous sont proposées.**
- Date limite de rendu de votre code sur le dépôt GitHub : **Dimanche 14 février à 23h00**.


## TP 4 : Systèmes d'enchères
#### _Thème : encapsulation, cycle de vie d'une application orientée objet_

Comme d'habitude, cliquez sur le lien ci-dessous pour faire votre fork privé du TP :

https://classroom.github.com/a/aT6i1xXH

Vous êtes chargé de développer le système d'enchères **iBaille**. Pour avoir une idée globale du système,
voici le principe général de fonctionnement :

* des produits sont mis en vente avec un prix initial (le prix de base) ;
* des utilisateurs peuvent enchérir sur les produits jusqu'à ce que l'enchère soit arrêtée ;
* pour pouvoir participer les utilisateurs doivent payer un coût de participation (différent pour chaque produit) ;
* à chaque enchérissement, le prix de base du produit augmente ;
* à la fin de la vente, l'utilisateur ayant proposé le prix le plus élevé, remporte le produit ;
* pour éviter des enchères inutiles (de 1 centime par exemple), le même pas d’enchère minimal est défini pour tous les produits ;
* lorsqu'un utilisateur propose un prix pour un produit, il propose également un prix maximal qu'il est prêt à débourser
  en cas d'enchère concurrente; ainsi, si par la suite, un autre utilisateur fait une enchère supplémentaire valide (c'est-à-dire avec un montant au moins égal au prix courant du produit + le pas d'enchère), le prix courant du produit augmente automatiquement. En effet, étant donné un produit, soient **c** son prix courant, **&delta;** le pas d'enchère, **M<sub>1</sub>** le maximum de l'enchère gagnante actuelle. Quand une nouvelle offre d'enchère **(c<sub>2</sub>**, **M<sub>2</sub>)** arrive :
    * elle est valide si **M<sub>2</sub>** &ge; **c<sub>2</sub>**  &ge; **c + &delta;**;
    * si **M<sub>1</sub>** &ge; **M<sub>2</sub>**, alors le gagnant ne change pas et le nouveau prix est **c** &leftarrow; **M<sub>2</sub>**
    * si **M<sub>1</sub>** < **M<sub>1</sub>**, alors la nouvelle enchère est désignée comme gagnante et le nouveau prix courant du produit est **c** &leftarrow; max(**M1, c+&delta;**)
      Par définition, le gagnant est celui dont le prix courant est supérieur au prix maximal proposé par tous les autres enchérisseurs.

Un utilisateur peut déposer une nouvelle offre d'enchère sur le même produit sur lequel il a déjà déposé une offre d'enchère. Par exemple, il pourra le faire si son offre a été "battue" par un autre enchérisseur.



1. La classe `Produit` vous est partiellement donnée mais quelque chose a été oublié : comme indiqué précédemment, le pas d'enchère doit être le même pour tous les produits mais modifiable par l'utilisateur. Modifiez la déclaration de cet attribut afin de satisfaire cette contrainte. Doit-on modifier également la méthode `setPasEnchere()` ?

    **Remarque** : Ne pas confondre la notion d'_utilisateur du logiciel_ (non-informaticien) et l'_utilisateur-programmeur_ qui est censé de se servir de votre application pour poursuivre son développement et pour la maintenance, le débuggage etc.
Ici l'utilisateur c'est l'informaticien.

1. Implémentez la méthode `void demarrerEnchere()` de `Produit` pour qu'elle rende l'objet disponible à l'enchère à la date et à l'heure courante (`LocalDate.now()` et `LocalTime.now()`, respectivement). Vous ajouterez à cette classe les autres éléments qui vous paraissent nécessaires pour que cette méthode fonctionne.

   Implémentez également la méthode réciproque `void arreterEnchere()`.
 
1. Complétez la classe `Compte` en y ajoutant une méthode qui permet de créditer le compte avec une somme donnée.

1. La classe `OffreEnchere` représentera une enchère proposée par un utilisateur pour un produit donné. Certains de ses attributs et méthodes vous sont donnés. Ajoutez dans la classe `OffreEnchere`, une méthode modifieur (_setter_) pour le prix en cours.

    **Remarque :** observez que dans le constructeur de cette classe, aucune vérification concernant la cohérence des attributs de l'offre créée avec ceux du produit n'a été faite (ce n'est pas la responsabilité de l'objet `OffreEnchere`)

1. Écrivez le code de la méthode `public OffreEnchere creerOffre(Produit produit, double prix, double prixMax)` de la classe `Compte`. Cette méthode doit vérifier que les conditions de création de l'offre sont réunies et dans le cas échéant créer une offre d'enchère et l'ajouter à sa liste d'offres d'enchères. La méthode retourne `null` si l'offre n'a pas pu être créée (les conditions n'étaient pas réunies). 

   Pour stocker les offres, on vous conseille d'utiliser une structure de données de type liste prédéfinie en _Java_, comme `java.util.ArrayList` ou `java.util.LinkedList`, mais vous êtes libres d'utiliser d'autres solutions.

    **Pensez à écrire des tests unitaires... beaucoup de tests unitaires !**

1. Complétez la méthode `boolean ajouterOffre(OffreEnchere o)` de la classe `Produit` afin qu'elle vérifie si `o` est __valide__ (en vérifiant le pas d'enchère, le fait que la session d'enchère du produit n'est pas arrêtée, etc.) et dans le cas échéant l'ajoute à la liste d'offres d'enchères de la classe `Produit`. La méthode devra retourner `true` si l'enchère est valide et `false` sinon.

    **Remarque :** dans cette méthode pensez à mettre à jour correctement les différentes entités de votre application.

    **Remarque :** écrire des tests unitaires pour cette fonction est fortement conseillé.

1. Implémentez la méthode `getGagnant()` de la classe `Produit`. Elle devra renvoyer la meilleure offre d'enchère.

1. Simulez votre application dans le programme principal (la classe `IBaille`). Pour cela, vous instancierez un produit et plusieurs comptes (3 au minimum). Pour chacun des comptes vous proposerez à l'utilisateur du logiciel (non-informaticien donc) de déposer des enchères pour ce produit en affichant les informations sur le produit et l'offre gagnante en cours.
   
    Pour récupérer les données saisies par l'utilisateur à la console, vous pouvez utiliser la classe `java.util.Scanner` qui permet de "parser" de manière intelligente une chaîne de caractères. Voici un petit exemple de ce que vous pouvez faire avec :

    ```java
    
    import java.util.Scanner;
    
    class IBaille {
    
        public static void main(String[] args) {
        
            // on attache un objet Scanner au flux d'entrée associée à la console
            Scanner saisie = new Scanner(System.in);
            System.out.println("Veuillez écrire quelque chose :");
        
            // récupération de la chaîne de caractères saisie par l'utilisateur
            String réponse = saisie.next();
        
            // récupération de la chaîne de caractères saisie par l'utilisateur sous forme d'un nombre entier
            int entier = saisie.nextInt();
           
        }
    
    }
    ```
Pour plus de détails sur cette classe, voir l'API : https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Scanner.html
