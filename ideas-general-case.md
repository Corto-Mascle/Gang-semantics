# Borne inf

## Pour 2EXPTIME :

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

## Pour TOWER

On va itérer l'argument précédent.
On note T(n) pour la valeur de la tour d'exponentielles de 2 de hauteur n.

L'idée est que si on peut compter jusqu'à T(n), alors on peut mettre en place le système suivant : 
Pour simplifier on va considérer que tous les processus peuvent accéder à un compteur allant de 0 à T(n).
Un processus central souhaite compter jusqu'à T(n+1). Pour celà il est assisté par des processus auxiliaires.

Chaque processus auxiliaire commence par s'attribuer un numéro entre 0 et T(n)-1 et un bit, puis il broadcaste son identifiant avec son numéro, en binaire, bit par bit.
En parallèle, il reçoit l'identifiant et le numéro d'un autre processus auxiliaire, et vérifie au vol que son numéro est bien l'incrément de celui-ci. Ensuite il reçoit un nombre de bits correspondant à son numéro, et les broadcaste en ajoutant son bit à la fin. Cette dernière étape est répétée deux fois.

Le processus central reçoit un identifiant d'un processus auxiliaire et son numéro, qui doit être T(n)-1 (que des 1 en binaire).
Ensuite il reçoit de la part de ce processus une séquence de T(n) bits nuls.
Puis il  reçoit un identifiant d'un processus auxiliaire et son numéro, qui doit être T(n)-1 aussi. Il reçoit T(n) bits du premier et du deuxième en parallèle et vérifie que le deuxième est bien l'incrément du premier. Il continue ainsi jusqu'à recevoir une séquence de T(n) bits tous à 1.
À ce stade il aura fait T(n+1) incrémentations.

# Décidabilité de la couverture

__To begin with we assume that processes do not check equality of receives with their initial values.__

In that case we can define a directed graph with processes as nodes and an edge from p1 to p2 if p1 broadcasts a value stored by p2. We call this graph the __communication graph__.

> ## Lemma 1: 
> If there is a run covering a state s, then there is one whose communication graph is a tree with all edges directed towards the root, and furthermore every value is copied at most once throughout the whole run. 
>>### Proof
>> This can be shown by induction on the length of the run: As a process cannot compare two of its registers, we can always assume that each one is communicating independently with a disjoint set of processes. Similarly whenever a process records a received value into a register, there is no way to compare it to a previous value, hence we can assume that it comes from a disjoint part of the system.
The copycat lemma allows us to create as many copies of the run as needed to ensure that this is feasible.

We now study the __local runs__. To each prefix of a local run over registers $r_1, ..., r_k$ we associate a tuple of k words $w_1, \ldots, w_k$ over the alphabet $\mathcal{M}$ of messages.
The word $w_i = m_1^i \cdots m_n^i$ is the sequence of messages that have been received with an equality test $=?r_i$ since a value was last copied into $r_i$.
We call this the __vision__ of that prefix.

The __output__ of a register in a local run is the sequence of messages broadcasting that initial register value.

> ## Lemma 2: 
> Let $k$ be the number of registers, we set $\leq$ to be the product ordering over $(\Sigma^*)^k$ with the subword ordering over each copy of $\Sigma^*$. 
> If a run is such that either :
> 1. There is a local run with two prefixes that end in the same state, such that the values of all registers have been received with the same messages, and the larger one has a greater vision than the other one for $\leq$. 
> 2. There is a branch of the tree such that a process has a greater output than one of its ancestors.
>
> Then there is a shorter run with the same final configuration.
>
>>### Proof 
>> In both cases, we use the fact that we can choose not to receive any arbitrary subsequence of messages from a process. For the first case we can replace the larger prefix with the shorter one in which the last values are replaced with the ones of the larger prefix and still have a correct run. For the second case we can replace the ancestor with its descendant to obtain a smaller tree. 

> ## Theorem 1: 
> Coverability is decidable for RBNA.
>
>>### Proof 
>> Higman's lemma combined with Lemma 2 guarantee us that all runs that do not match one of the two conditions are finite, hence we can explore them all to get the set of reachable configurations.
However, we still have to get rid of the hypothesis presented at the start of the section.
To do so, we compute the __repeatability__ relation $R$ over $\mathcal{M}$, which is the transitive closure of the relation containing $(m,m')$ if and only if there is a local run that receives $m$ and stores the corresponding value and later broadcasts that same value with the message $m'$.
We can compute (a sufficient subset of) this relation incrementally, by repeated calls to the coverability problem. Given (a subset of this relation), we allow a process to receive a message $m'$ with one of its initial values (with an equality test) if and only if it previously broadcast that value with a message $m$ such that $m R m'$.

# Undecidability of Büchi conditions 

We have two types of processes: the $start$ and $tape$.
We are going to simulate an accepting run of a Turing machine.

Each process broadcasts its identifier and receives another one.
Each process has a finite memory among all the possible contents of a tape cell of the TM (letter + state).
$start$ sends a sequence of messages, which are transitions of the TM.
Upon receiving a message $m$, a $tape$ process either applies the transition or passes the message (there are a couple details to deal with, as several processes need to apply the transition if the head moves, but they are easy to implement assuming all messages are transmitted accurately). If a process applies the transition, then it sends a marked version of the message $\overline{m}$.
After sending each transition, $start$ waits for a marked message with the same transition. 
Once it receives it, it sends the next transition.

When $start$ receives a marked transition to an accepting state, it goes to a special state part of the protocol, from which it alternately broadcasts a message $acc$ and receives that same message.

It wins if it receives infinitely many such messages.

It is easy to construct an accepting run of the BRNA from one of the TM.
For the other direction, observe that all processes are either in a cycle of the communication graph or one of its ancestors is. 

Furthermore, observe that if a member of a circle receives $acc$, then it has sent and received exactly the same number of messages, and the same goes for all members of that circle. As a result, all messages must have been transmitted in full, hence we have a run of the TM.

If some process receives infinitely many $acc$ messages, then so does its predecessor in the communication graph, and the predecessor of its predecessor, and so on...
As there are finitely many processes, eventually we must find a circle in which a process receives $acc$, hence the TM has an accepting run.