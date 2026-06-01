# 🔮 Prédictions Examen Java — II.1102 Algorithmique et Programmation

Analyse basée sur **4 sujets d'années précédentes** (2020 à 2025) du même cours.

---

## Format de l'examen (constant depuis 4 ans)

| Partie | Thème | Points |
|---|---|---|
| 1 | Questions de compréhension / cours | ~8 pts |
| 2 | Compilation et correction de code | ~9 pts |
| 3 | Exercices de programmation (2 exos) | ~16 pts |
| 4 | Modélisation UML | ~7 pts |
| **Total** | | **~40 pts** |

Durée : **3 heures** — Aucun document autorisé.

---

## Partie 1 — Questions de compréhension (8 pts)

### Thèmes récurrents (par fréquence)

| Thème | Tombé dans |
|---|---|
| 🔴 `static` vs non-static / attributs de classe vs d'instance | 3/4 sujets |
| 🔴 Visibilité (private, protected, public, default) | 3/4 sujets |
| 🟡 Classe abstraite vs Interface | 2/4 sujets |
| 🟡 Héritage vs Interface | 2/4 sujets |
| 🟡 Constructeurs | 2/4 sujets |
| 🟡 Types primitifs vs reference | 2/4 sujets |
| 🟢 Pile vs File (Stack vs Queue) | 1/4 sujets |
| 🟢 Maps / Dictionnaires | 1/4 sujets |
| 🟢 Getters et Setters | 1/4 sujets |

### Questions probables

1. **Classe abstraite vs Interface** : Quelle est la différence ? Quand utiliser l'un plutôt que l'autre ? Java 8+ a changé quoi ?

2. **Passage par valeur** : Comment Java passe-t-il les paramètres ? Que se passe-t-il quand on passe un objet ? un int ? un tableau ?

3. **`static` et `final`** : Que signifie chaque mot-clé pour un attribut ? une méthode ? une classe ?

4. **Visibilité** : Expliquez `private`, `protected`, `public` et "default" (package-private). Donnez un exemple d'usage pour chacun.

5. **Pile (Stack) vs File (Queue)** : Différence de fonctionnement (LIFO vs FIFO). Donnez un cas d'usage concret pour chaque structure.

---

## Partie 2 — Compilation et correction de code (9 pts)

### 2.1 — Lecture de code (comprendre ce que ça affiche)

**Pattern :** Un programme avec :
- Une **méthode récursive** qui manipule une `ArrayList<Integer>`
- Du **passage par valeur VS référence** pour piéger l'étudiant

Exemple type :
```java
import java.util.ArrayList;
public class Examen {
    public static void main(String[] args) {
        ArrayList<Integer> liste = new ArrayList<>();
        for (int i = 1; i <= 5; i++) liste.add(i);
        int res = mystere(liste, liste.size() - 1);
        System.out.println("Résultat : " + res);
    }
    public static int mystere(ArrayList<Integer> lst, int idx) {
        if (idx < 0) return 0;
        if (lst.get(idx) % 2 != 0)
            return lst.get(idx) + mystere(lst, idx - 1);
        return mystere(lst, idx - 1);
    }
}
```
→ Questions possibles : somme des impairs, somme des pairs, comptage d'occurrences...

### 2.2 — Code avec 8 erreurs à corriger

**Pièges classiques qui tombent à chaque fois :**

```java
int nombre = "10";                    // Erreur : String dans int
System.out.println("texte")           // Erreur : ; manquant
String message;                       // Erreur : pas initialisé
message.equals("x");                  // → NullPointerException
tableau[tableau.length] = 100;        // Erreur : index out of bounds
for (int i = 0; i < 5; i++);         // Erreur : ; qui coupe la boucle
public static void main(String args) // Erreur : pas de []
afficherMessage();                    // méthode non-static appelée depuis static
System.out.println("message);        // Erreur : guillemet fermant manquant
```

---

## Partie 3 — Exercices de programmation (16 pts)

### 3.1 — Itératif ET récursif (toujours, depuis 4 ans)

**Problèmes déjà tombés :** palindrome, suppression d'un caractère, Fibonacci, coefficients binomiaux (Cnk), triangle de Pascal

**Problèmes probables cette session :**

- **Compter les occurrences d'un caractère** dans une chaîne (itératif + récursif)
- **Inverser une chaîne** (itératif + récursif)
- **Vérifier si un tableau est trié** (itératif + récursif)
- **Calculer une puissance** a^b (itératif + récursif)
- **Vérifier si une chaîne contient uniquement des chiffres** (itératif + récursif)

**Attention :** Chaque version est accompagnée de **"Calculez la complexité"**.

### 3.2 — Algorithme de tri ou de recherche

**Déjà tombés :** Quicksort, tri par sélection, tri par insertion, dichotomie

**Probable cette session :**
- **Merge sort (tri fusion)** — seul tri O(n log n) pas encore proposé
- Ou **variante d'un tri existant** (tri décroissant, tri d'ArrayList d'objets avec Comparable)

**Toujours accompagné de :** complexité en comparaisons et opérations, pire cas / meilleur cas.

---

## Partie 4 — Modélisation UML (7 pts)

### Pattern constant

Description textuelle d'un système réel → **diagramme de classes complet** (classes, attributs, méthodes, associations, multiplicités, héritage).

**Sujets déjà traités :**
- Hôtel (réservation de chambres)
- Compagnie aérienne (vols, passagers, réservations)
- Bibliothèques de Paris (ressources, emprunts)
- Moodle (cours, utilisateurs, devoirs)

### Systèmes probables cette session

| Système | Pourquoi |
|---|---|
| **🔴 Gestion de bibliothèque** (livres, auteurs, emprunteurs) | Proche du sujet 2022, variante simplifiée |
| **🟡 Plateforme e-commerce** (produits, panier, commandes, clients) | Classique, jamais tombé |
| **🟡 Gestion d'école** (étudiants, cours, enseignants, notes) | Polyvalent, testé ailleurs |
| **🟢 Système de réservation restaurant** (tables, clients, réservations) | Variante du sujet hôtel |

### Ce qu'il faut maîtriser en UML

- `+` public, `-` private, `#` protected
- Classes, attributs, méthodes
- Héritage (flèche vide), interface (flèche pointillée + `<<interface>>`)
- Associations : simple, agrégation (losange vide), composition (losange plein)
- Multiplicités : `1`, `0..1`, `*`, `1..*`
- Méthodes à mettre ou non (cas d'usage → méthodes du sujet)

---

## Top 5 — Priorité d'entraînement

| # | Sujet | Probabilité | Temps à y passer |
|---|---|---|---|
| 1 | **Itératif vs récursif** (compter, inverser, puissance) | 🔴 Très élevée | 45 min |
| 2 | **Correction de code (8 erreurs)** | 🔴 Très élevée | 20 min |
| 3 | **Merge sort** | 🟡 Élevée | 30 min |
| 4 | **UML : bibliothèque ou e-commerce** | 🟡 Élevée | 30 min |
| 5 | **Questions de cours** (abstraite/interface, static/final, visibilité) | 🔴 Très élevée | 20 min |

---

## Rappels importants

- **Afficher ≠ Retourner** — une méthode qui `println` ne rend pas la valeur à l'appelant
- **Syntaxe Java** — respectez-la. Quelques erreurs tolérées, pas l'oubli systématique
- **Complexité** — soyez prêt à la calculer pour TOUT code que vous écrivez
- **Rédaction français ou anglais** — au choix

---

_Généré par Veille 🔧 — Analyse de sujets : Sujet_Algo_Java_S1 (2024-2025), Sujet_Algo_Java_S2 (2024-2025), examen_java_2122_S1, sujet (2020)_
