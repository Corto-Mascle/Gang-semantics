
**Borne inf**

Pour 2EXPTIME :

2EXPTIME = AEXPSPACE, on va donc réduire la terminaison d'une machine de Turing alternante en espace exponentiel.
Une telle machine a des états existentiels et universels.
Depuis un état existentiel la machine choisit de manière non-déterministe la transition suivante. Depuis un état universel, la machine lance en parallèle un calcul par transition possible (ou demande à un adversaire de choisir la transition suivante).
On peut supposer qu'il y a toujours exactement deux transitions possibles.

Une exécution acceptante de cette machine est un arbre fini étiqueté par des configurations (donc chacune de longueur exponentielle). Les feuilles doivent être étiquetées par des configurations avec un état acceptant, et les noeuds internes ont un fils si ils ont un état existentiel (avec une configuration obtenue en appliquant une transition), deux si ils ont un état universel (obtenus en appliquant les deux transitions possibles).

On simule une exécution ainsi :

- Un processus peut utiliser ses registres comme des cases mémoires, il peut donc mémoriser des nombres binaires jusqu'à une valeur exponentielle par exemple.

- Donc un processus peut compter jusqu'à $2^N$ en utilisant N registres.

- Un processus P peut broadcaster son identifiant puis attendre de recevoir ceux de deux successeurs P1 et P2. Puis P peut choisir un état de la machine et une suite de $2^N$ lettres représentant une configuration. À chaque lettre :

    -  P attend de recevoir une lettre de la part de P1 et P2 (si son état est universel), juste P1 (si son état est existentiel) ou de personne (si son état est final).

    - P incrémente son compteur

    - P broadcaste sa lettre avec son identifiant.

    - P vérifie au fur et à mesure que les configurations décrites par se successeurs sont respectent les transitions de la machine de Turing

La MT a une exécution acceptante si et seulement si le protocole ci-dessus a un run où un processus parvient à lire entièrement une configuration initiale: En effet pour qu'un processus puisse aller au bout de sa configuration, il faut que ses successeurs aussi. Donc pour qu'un processus décrivant la configuration initiale termine son exécution, il doit être la racine d'un arbre fini où toutes les feuilles ont des états acceptants de la MT.

**Borne sup**

???????????

Piste : Observer qu'on peut toujours supposer qu'un processus ne reçoit jamais deux messages broadcastés par le même processus, sauf si c'était une valeur initiale de ce processus.
Puis prouver que les runs locaux n'ont pas besoin d'être plus qu'exponentiellement longs, avec un argument du type "si on est deux fois dans le même état avec la même relation d'égalité entre les registres  et qu'on a fait les mêmes broadcasts des valeurs de ces registres... bla bla bla alors on peut raccourcir le run local" 
Et transformer ces conditions en un automate d'arbres de taille doublement exponentielle sur des arbres étiquetés par des exécutions du protocole de taille simplement exponentielle.  