# 📚 Guide de Révisions — Examen Java II.1102

Guide complet et détaillé de toutes les notions qui tombent à l'examen. Plus besoin de chercher ailleurs.

---

## Table des matières

1. [Types primitifs et références](#1-types-primitifs-et-références)
2. [POO : classes, objets, constructeurs](#2-poo-classes-objets-constructeurs)
3. [Visibilité et encapsulation](#3-visibilité-et-encapsulation)
4. [Static et Final](#4-static-et-final)
5. [Héritage, classes abstraites et interfaces](#5-héritage-classes-abstraites-et-interfaces)
6. [Structures de données](#6-structures-de-données)
7. [Itératif vs Récursif](#7-itératif-vs-récursif)
8. [Algorithmes de tri](#8-algorithmes-de-tri)
9. [Complexité algorithmique](#9-complexité-algorithmique)
10. [Passage par valeur vs référence](#10-passage-par-valeur-vs-référence)
11. [Erreurs Java classiques](#11-erreurs-java-classiques)
12. [UML — Diagramme de classes](#12-uml--diagramme-de-classes)

---

## 1. Types primitifs et références

### Types primitifs (8 en Java)

| Type | Taille | Valeurs | Exemple |
|---|---|---|---|
| `byte` | 8 bits | -128 à 127 | `byte b = 100;` |
| `short` | 16 bits | -32 768 à 32 767 | `short s = 1000;` |
| `int` | 32 bits | -2³¹ à 2³¹-1 | `int i = 42;` |
| `long` | 64 bits | -2⁶³ à 2⁶³-1 | `long l = 100000L;` |
| `float` | 32 bits | ±3.4e-38 à ±3.4e38 | `float f = 3.14f;` |
| `double` | 64 bits | ±1.7e-308 à ±1.7e308 | `double d = 3.14;` |
| `char` | 16 bits | Caractère Unicode | `char c = 'A';` |
| `boolean` | 1 bit | true ou false | `boolean b = true;` |

**Règle d'or :** Un type primitif stocke la **valeur directement** dans la variable.

### Types référence

Tout ce qui n'est pas primitif : `String`, `int[]`, `ArrayList`, `Scanner`, vos classes...

**Règle d'or :** Un type référence stocke **l'adresse mémoire** de l'objet (un pointeur).

### String — attention particulière

```java
String s1 = "hello";
String s2 = "hello";
String s3 = new String("hello");

s1 == s2      // true (même référence dans le pool de strings)
s1 == s3      // false (objet différent créé avec new)
s1.equals(s3) // true (comparaison de contenu)
```

**Ne JAMAIS utiliser `==` pour comparer des String. Toujours `.equals()`.**

---

## 2. POO : classes, objets, constructeurs

### Classe vs Objet

- **Classe** : le plan / le moule / le type abstrait
- **Objet** : une instance concrète de la classe (créée avec `new`)

```java
// Définition de la classe (le plan)
public class Etudiant {
    String nom;
    int age;
}

// Création d'objets (les instances concrètes)
Etudiant e1 = new Etudiant();
Etudiant e2 = new Etudiant();
```

### Constructeur

Méthode spéciale appelée automatiquement avec `new`. Même nom que la classe, pas de type de retour.

```java
public class Etudiant {
    String nom;
    int age;

    // Constructeur par défaut (si aucun constructeur défini, Java le met automatiquement)
    public Etudiant() {
        this.nom = "Inconnu";
        this.age = 0;
    }

    // Constructeur avec paramètres
    public Etudiant(String nom, int age) {
        this.nom = nom;     // this.nom = attribut de l'objet, nom = paramètre
        this.age = age;
    }
}

// Utilisation
Etudiant e1 = new Etudiant();               // constructeur par défaut
Etudiant e2 = new Etudiant("Maraa", 22);    // constructeur avec paramètres
```

**Rappels importants :**
- Si tu définis AU MOINS UN constructeur, Java ne met plus le constructeur par défaut
- Un constructeur **ne retourne rien** (pas même `void`)
- `this` fait référence à l'objet courant

### Accesseurs (getters) et mutateurs (setters)

```java
public class Personne {
    private String nom;   // attribut privé → inaccessible de l'extérieur

    // Getter : permet de LIRE l'attribut
    public String getNom() {
        return this.nom;
    }

    // Setter : permet de MODIFIER l'attribut (avec contrôle éventuel)
    public void setNom(String nom) {
        if (nom != null && !nom.isEmpty()) {
            this.nom = nom;
        }
    }
}
```

**Pourquoi ?** Encapsulation. On contrôle l'accès aux données. On peut valider, calculer, logger...

---

## 3. Visibilité et encapsulation

### Les 4 niveaux de visibilité

| Modificateur | Classe | Package | Sous-classe | Monde |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| `default` (rien) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

**Règles pratiques :**
- **Attributs** → toujours `private` (on y accède via getters/setters)
- **Méthodes utilitaires internes** → `private`
- **Constructeurs** → généralement `public`
- **Méthodes destinées aux sous-classes** → `protected`
- **API publique** → `public`

---

## 4. Static et Final

### Static

Appartient à la **classe**, pas à l'objet. Un seul exemplaire partagé.

```java
public class Compteur {
    public static int total = 0;   // un seul compteur, partagé par TOUS les objets
    public int individuel = 0;     // un compteur PAR objet

    public Compteur() {
        total++;
        this.individuel = total;
    }
}

Compteur c1 = new Compteur();  // total devient 1, c1.individuel = 1
Compteur c2 = new Compteur();  // total devient 2, c2.individuel = 2

System.out.println(Compteur.total);       // 2 — accessible via la classe
System.out.println(c1.individuel);        // 1
```

**Différence clé méthode static vs non-static :**

```java
public class Exemple {
    private int valeur = 10;

    public static void methodeStatic() {
        // ✅ Peut appeler d'autres méthodes static
        // ✅ Peut utiliser uniquement des attributs static
        // ❌ PAS de this (pas d'objet associé)
        // ❌ Pas d'accès aux attributs non-static
    }

    public void methodeInstance() {
        // ✅ this.valeur — on a un objet
        // ✅ Peut accéder aux attributs static et non-static
    }
}

// Appel :
Exemple.methodeStatic();         // direct sur la classe
Exemple obj = new Exemple();
obj.methodeInstance();           // besoin d'un objet
```

### Final

- **Variable** → ne peut plus être modifiée (constante)
- **Méthode** → ne peut pas être redéfinie dans une sous-classe
- **Classe** → ne peut pas être héritée (ex: `String`, `Math`)

```java
public final class Immutable {        // cette classe ne peut pas avoir d'enfants
    public final int MAX = 100;       // cette variable ne peut pas changer
    public final void display() {}    // cette méthode ne peut pas être redéfinie
}
```

---

## 5. Héritage, classes abstraites et interfaces

### Héritage simple (`extends`)

```java
public class Animal {
    protected String nom;

    public void manger() {
        System.out.println(nom + " mange");
    }
}

public class Chien extends Animal {
    public void aboyer() {
        System.out.println("Wouf !");
    }
}

// Usage
Chien c = new Chien();
c.nom = "Rex";     // hérité
c.manger();         // hérité
c.aboyer();         // spécifique à Chien
```

**Règles :**
- `extends` → une classe hérite d'**une seule** classe parent (pas d'héritage multiple en Java)
- Le constructeur de la classe parent est appelé avec `super()`
- `super` permet d'accéder aux membres du parent

```java
public class Chien extends Animal {
    public Chien(String nom) {
        super();        // appel au constructeur Animal() — implicite si pas écrit
        this.nom = nom;
    }

    @Override           // bonne pratique : indique qu'on redéfinit une méthode du parent
    public void manger() {
        System.out.println("Le chien mange des croquettes");
    }
}
```

### Classe abstraite

Classe **qui ne peut pas être instanciée directement**. Sert de base commune.

```java
public abstract class Forme {
    protected String couleur;

    public Forme(String couleur) {
        this.couleur = couleur;
    }

    // Méthode concrète (déjà implémentée)
    public String getCouleur() {
        return this.couleur;
    }

    // Méthode abstraite (DOIT être implémentée par les sous-classes)
    public abstract double calculerAire();
}

public class Cercle extends Forme {
    private double rayon;

    public Cercle(String couleur, double rayon) {
        super(couleur);
        this.rayon = rayon;
    }

    @Override
    public double calculerAire() {
        return Math.PI * rayon * rayon;
    }
}

// ❌ Forme f = new Forme("rouge");    // INTERDIT
// ✅ Forme f = new Cercle("rouge", 5); // OK (polymorphisme)
```

**Caractéristiques :**
- Peut avoir des attributs (contrairement aux interfaces avant Java 8)
- Peut avoir des constructeurs
- Peut avoir des méthodes concrètes ET abstraites
- Une classe ne peut hériter que d'UNE SEULE classe abstraite

### Interface

Contrat que les classes s'engagent à respecter.

```java
public interface Volant {
    // Méthode abstraite (implicitement public abstract)
    void voler();

    // Depuis Java 8 : méthode par défaut (avec implémentation)
    default void atterrir() {
        System.out.println("Atterrissage...");
    }

    // Attribut (implicitement public static final)
    int ALTITUDE_MAX = 10000;
}

public class Avion implements Volant {
    @Override
    public void voler() {
        System.out.println("L'avion vole");
    }
}

public class Oiseau implements Volant {
    @Override
    public void voler() {
        System.out.println("L'oiseau vole");
    }
}
```

**Caractéristiques :**
- Toutes les méthodes sont `public abstract` (sauf `default` depuis Java 8)
- Tous les attributs sont `public static final` (des constantes)
- Pas de constructeur
- Une classe peut implémenter **PLUSIEURS** interfaces
- Une interface peut étendre plusieurs autres interfaces

### Comparaison rapide

| | Classe concrète | Classe abstraite | Interface |
|---|---|---|---|
| Instanciable ? | ✅ Oui | ❌ Non | ❌ Non |
| Constructeur ? | ✅ Oui | ✅ Oui | ❌ Non |
| Attributs ? | ✅ N'importe | ✅ N'importe | ⚠️ `public static final` uniquement |
| Méthodes concrètes ? | ✅ Oui | ✅ Oui | ⚠️ `default` ou `static` uniquement |
| Méthodes abstraites ? | ❌ Non | ✅ Oui | ✅ Oui |
| Héritage multiple ? | ❌ Non | ❌ Non | ✅ Oui |
| `extends` / `implements` | `extends` | `extends` | `implements` |

### Quand utiliser quoi ?

- **Classe concrète** → un objet réel qui existe tel quel
- **Classe abstraite** → des classes très liées qui partagent du code ET de l'état (attributs). Ex: `Animal` → `Chien`, `Chat`
- **Interface** → un comportement transversal. Ex: `Volant` → `Avion`, `Oiseau`, `Drone`. Capacité partagée par des classes sans lien hiérarchique.

---

## 6. Structures de données

### Tableaux

```java
// Déclaration + initialisation
int[] tab = new int[5];               // {0, 0, 0, 0, 0}
int[] tab2 = {4, 8, 15, 16, 23, 42}; // initialisation directe

// Accès
tab[0] = 10;           // premier élément
int x = tab[0];

// Parcours
for (int i = 0; i < tab.length; i++) {
    System.out.println(tab[i]);
}

// foreach (quand on n'a pas besoin de l'index)
for (int val : tab2) {
    System.out.println(val);
}
```

**Piège :** `tab.length` = taille. **Dernier index = tab.length - 1**. `tab[tab.length]` → ArrayIndexOutOfBoundsException.

### ArrayList (dans java.util)

```java
import java.util.ArrayList;

ArrayList<Integer> liste = new ArrayList<>();

liste.add(10);          // ajoute à la fin
liste.add(0, 5);        // insère à l'index 0 (décale le reste)
liste.get(1);           // lit l'élément à l'index 1
liste.set(1, 25);       // remplace l'élément à l'index 1
liste.remove(1);        // supprime l'élément à l'index 1
liste.size();           // taille (attention : pas .length !)
liste.contains(10);     // vérifie si l'élément existe
liste.isEmpty();        // true si vide

// Parcours
for (Integer n : liste) { System.out.println(n); }

// ⚠️ Ne pas utiliser de types primitifs : ArrayList<int> est INTERDIT
//    Utiliser les wrapper : Integer, Double, Boolean, Character...
```

### Pile (Stack) — LIFO (Last In, First Out)

```java
import java.util.Stack;

Stack<String> pile = new Stack<>();
pile.push("A");      // empiler
pile.push("B");
String haut = pile.pop();     // dépiler : retourne "B", le retire de la pile
String sommet = pile.peek();  // regarder sans retirer : retourne "A"
boolean vide = pile.isEmpty();
```

**Cas d'usage :** historique de navigation (bouton "Retour"), undo/ctrl+z, parsing d'expressions, appels de fonctions récursifs.

### File (Queue) — FIFO (First In, First Out)

```java
import java.util.LinkedList;
import java.util.Queue;

Queue<String> file = new LinkedList<>();  // LinkedList implémente Queue
file.add("A");        // ajouter (lance une exception si plein)
file.offer("B");      // ajouter (retourne false si plein)
String tete = file.remove();  // retirer : retourne "A" (exception si vide)
String tete2 = file.poll();   // retirer : retourne "B" (null si vide)
String regarder = file.peek(); // regarder sans retirer (null si vide)
```

**Cas d'usage :** file d'attente (impressions, tâches), BFS (parcours en largeur), buffer.

### Map / Dictionnaire (clé → valeur)

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> ages = new HashMap<>();
ages.put("Alice", 22);           // ajouter / modifier
ages.put("Bob", 25);
int age = ages.get("Alice");     // 22 (null si clé inexistante)
int age2 = ages.getOrDefault("Charlie", 0); // 0 si pas trouvé
boolean existe = ages.containsKey("Alice"); // true
ages.remove("Bob");              // supprimer

// Parcourir les clés
for (String nom : ages.keySet()) {
    System.out.println(nom + " → " + ages.get(nom));
}

// Parcourir les entrées
for (Map.Entry<String, Integer> entry : ages.entrySet()) {
    System.out.println(entry.getKey() + " → " + entry.getValue());
}
```

**Pourquoi utiliser une Map ?** Accès direct par clé en O(1) au lieu de parcourir une liste en O(n). Exemple : annuaire téléphonique, compteur de mots, cache.

---

## 7. Itératif vs Récursif

### Méthode itérative

Boucle explicite (for, while, do-while). Simple, efficace, pas de risque de stack overflow.

```java
// Exemple : factorielle en itératif
public static int factorielle(int n) {
    int resultat = 1;
    for (int i = 2; i <= n; i++) {
        resultat *= i;
    }
    return resultat;
}
```

### Méthode récursive

La méthode s'appelle elle-même. **Deux choses obligatoires :**
1. **Cas de base** (condition d'arrêt) — sans ça, la récursion ne s'arrête jamais → StackOverflowError
2. **Appel récursif** qui se rapproche du cas de base

```java
// Exemple : factorielle en récursif
public static int factorielle(int n) {
    // CAS DE BASE
    if (n <= 1) return 1;

    // APPEL RÉCURSIF
    return n * factorielle(n - 1);
}
```

### Patterns récursifs qui tombent à l'examen

**1. Palindrome (déjà tombé)**
```java
// Itératif
public static boolean estPalindrome(String s) {
    int gauche = 0, droite = s.length() - 1;
    while (gauche < droite) {
        if (s.charAt(gauche) != s.charAt(droite)) return false;
        gauche++;
        droite--;
    }
    return true;
}

// Récursif
public static boolean estPalindrome(String s) {
    if (s.length() <= 1) return true;        // cas de base
    if (s.charAt(0) != s.charAt(s.length()-1)) return false;
    return estPalindrome(s.substring(1, s.length()-1));
}
```

**2. Supprimer un caractère (déjà tombé)**
```java
// Itératif
public static String supprimerCaractere(String s, char c) {
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) != c) sb.append(s.charAt(i));
    }
    return sb.toString();
}

// Récursif
public static String supprimerCaractere(String s, char c) {
    if (s.isEmpty()) return "";                           // cas de base
    char premier = s.charAt(0);
    String reste = supprimerCaractere(s.substring(1), c); // appel récursif
    if (premier == c) return reste;
    return premier + reste;
}
```

**3. Compter les occurrences (très probable)**
```java
// Itératif
public static int compterOccurrences(String s, char c) {
    int compteur = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == c) compteur++;
    }
    return compteur;
}

// Récursif
public static int compterOccurrences(String s, char c) {
    if (s.isEmpty()) return 0;                        // cas de base
    int compteur = compterOccurrences(s.substring(1), c); // appel récursif
    if (s.charAt(0) == c) compteur++;
    return compteur;
}
```

**4. Inverser une chaîne (très probable)**
```java
// Itératif
public static String inverser(String s) {
    StringBuilder sb = new StringBuilder();
    for (int i = s.length() - 1; i >= 0; i--) {
        sb.append(s.charAt(i));
    }
    return sb.toString();
}

// Récursif
public static String inverser(String s) {
    if (s.isEmpty()) return "";
    return inverser(s.substring(1)) + s.charAt(0);
}
```

**5. Puissance a^b (probable)**
```java
// Itératif
public static int puissance(int a, int b) {
    int resultat = 1;
    for (int i = 0; i < b; i++) {
        resultat *= a;
    }
    return resultat;
}

// Récursif
public static int puissance(int a, int b) {
    if (b == 0) return 1;                    // a^0 = 1
    return a * puissance(a, b - 1);          // a^b = a * a^(b-1)
}

// Récursif optimisé (diviser pour régner)
public static int puissanceOpti(int a, int b) {
    if (b == 0) return 1;
    int moitie = puissanceOpti(a, b / 2);
    if (b % 2 == 0) return moitie * moitie;
    else return moitie * moitie * a;
}
```

### Complexité comparée

| Problème | Itératif | Récursif simple | Récursif optimisé |
|---|---|---|---|
| Factorielle | O(n) | O(n) | — |
| Fibonacci | O(n) | O(2ⁿ) ❌ | O(n) avec mémoïsation |
| Palindrome | O(n) | O(n) | — |
| Suppression caractère | O(n) | O(n) | — |
| Puissance | O(n) | O(n) | O(log n) ✅ |

---

## 8. Algorithmes de tri

### Tri par sélection (Selection Sort)

```java
public static void triSelection(int[] tab) {
    for (int i = 0; i < tab.length - 1; i++) {
        // Trouver l'index du plus petit élément dans la partie non triée
        int indexMin = i;
        for (int j = i + 1; j < tab.length; j++) {
            if (tab[j] < tab[indexMin]) {
                indexMin = j;
            }
        }
        // Échanger
        int temp = tab[i];
        tab[i] = tab[indexMin];
        tab[indexMin] = temp;
    }
}
```

- **Principe :** À chaque étape, on cherche le minimum dans la partie non triée et on le place au début
- **Complexité :** O(n²) dans tous les cas (meilleur, pire, moyen)
- **Comparaisons :** n(n-1)/2 toujours

### Tri par insertion (Insertion Sort)

```java
public static void triInsertion(int[] tab) {
    for (int i = 1; i < tab.length; i++) {
        int cle = tab[i];
        int j = i - 1;
        // Décaler les éléments plus grands que la clé
        while (j >= 0 && tab[j] > cle) {
            tab[j + 1] = tab[j];
            j--;
        }
        tab[j + 1] = cle;
    }
}
```

- **Principe :** Comme trier des cartes. On prend chaque élément et on l'insère à sa place dans la partie déjà triée
- **Complexité :** O(n²) pire cas, **O(n) meilleur cas** (tableau déjà trié)
- **Stable :** Oui (préserve l'ordre des éléments égaux)

### Tri rapide (Quick Sort)

```java
public static void triRapide(int[] tab, int debut, int fin) {
    if (debut >= fin) return;

    int pivot = partition(tab, debut, fin);
    triRapide(tab, debut, pivot - 1);   // trier la partie gauche
    triRapide(tab, pivot + 1, fin);     // trier la partie droite
}

public static int partition(int[] tab, int debut, int fin) {
    int pivot = tab[fin];         // on prend le dernier élément comme pivot
    int i = debut - 1;            // index du plus petit élément

    for (int j = debut; j < fin; j++) {
        if (tab[j] <= pivot) {
            i++;
            // Échanger tab[i] et tab[j]
            int temp = tab[i];
            tab[i] = tab[j];
            tab[j] = temp;
        }
    }

    // Placer le pivot à sa position finale
    int temp = tab[i + 1];
    tab[i + 1] = tab[fin];
    tab[fin] = temp;

    return i + 1;                 // retourner la position du pivot
}

// Appel : triRapide(tab, 0, tab.length - 1);
```

- **Principe :** Diviser pour régner. Choisir un pivot, partitionner, trier récursivement
- **Complexité :** O(n log n) en moyenne, **O(n²) dans le pire cas** (pivot toujours min ou max)
- **Espace mémoire :** O(log n) (pile d'appels récursifs)

### Tri fusion (Merge Sort) — pas encore tombé

```java
public static void triFusion(int[] tab) {
    if (tab.length <= 1) return;

    // Diviser
    int milieu = tab.length / 2;
    int[] gauche = new int[milieu];
    int[] droite = new int[tab.length - milieu];

    for (int i = 0; i < milieu; i++) gauche[i] = tab[i];
    for (int i = milieu; i < tab.length; i++) droite[i - milieu] = tab[i];

    // Régner (récursivement)
    triFusion(gauche);
    triFusion(droite);

    // Fusionner
    fusionner(tab, gauche, droite);
}

public static void fusionner(int[] tab, int[] gauche, int[] droite) {
    int i = 0, j = 0, k = 0;

    while (i < gauche.length && j < droite.length) {
        if (gauche[i] <= droite[j]) {
            tab[k++] = gauche[i++];
        } else {
            tab[k++] = droite[j++];
        }
    }

    while (i < gauche.length) tab[k++] = gauche[i++];
    while (j < droite.length) tab[k++] = droite[j++];
}
```

- **Principe :** Diviser le tableau en deux, trier chaque moitié, fusionner
- **Complexité :** **O(n log n) dans tous les cas** (meilleur, pire, moyen) ✅
- **Stable :** Oui
- **Inconvénient :** Nécessite O(n) mémoire supplémentaire
- **Pourquoi ça tombe probablement :** Seul tri O(n log n) garanti pas encore demandé

### Comparaison des tris

| Algorithme | Pire cas | Moyen | Meilleur | Mémoire | Stable |
|---|---|---|---|---|---|
| Sélection | O(n²) | O(n²) | O(n²) | O(1) | ❌ |
| Insertion | O(n²) | O(n²) | **O(n)** ✅ | O(1) | ✅ |
| Rapide | O(n²) | **O(n log n)** | O(n log n) | O(log n) | ❌ |
| Fusion | **O(n log n)** ✅ | **O(n log n)** | **O(n log n)** | O(n) | ✅ |

---

## 9. Complexité algorithmique

### Notation Grand O (O)

Décrit comment le temps d'exécution **grandit** quand la taille des données **augmente**.

| Notation | Nom | Exemple | n=10 | n=100 | n=1000 |
|---|---|---|---|---|---|
| O(1) | Constant | Accès tableau `tab[i]` | 1 | 1 | 1 |
| O(log n) | Logarithmique | Dichotomie | ~3 | ~7 | ~10 |
| O(n) | Linéaire | Parcours simple | 10 | 100 | 1000 |
| O(n log n) | Quasi-linéaire | Tris efficaces | ~33 | ~664 | ~9966 |
| O(n²) | Quadratique | Boucles imbriquées | 100 | 10 000 | 1 000 000 |
| O(2ⁿ) | Exponentiel | Fibonacci récursif | 1024 | 10³⁰ | ✨ Big Crunch |

### Comment calculer la complexité

**Règles simples :**
1. Une instruction simple → O(1)
2. Une boucle de n itérations → O(n)
3. Deux boucles imbriquées → O(n²)
4. Une fonction récursive qui s'appelle 1 fois → O(n) si diminue de 1, O(log n) si divise par 2
5. Une fonction récursive qui s'appelle 2 fois → O(2ⁿ) (attention : exponentiel)
6. On garde **le terme dominant** : O(n² + n) = O(n²)
7. On ignore les constantes : O(3n) = O(n)

**Exemples :**
```java
for (int i = 0; i < n; i++) {       // O(n)
    for (int j = 0; j < n; j++) {   // O(n)
        // O(1)
    }
}
// O(n * n) = O(n²)

for (int i = 0; i < n; i++) {       // O(n)
    // O(1)
}
for (int j = 0; j < n; j++) {       // O(n)
    // O(1)
}
// O(n + n) = O(2n) = O(n)

while (n > 0) {                     // O(log n) — n est divisé par 2 à chaque fois
    n = n / 2;
}
```

---

## 10. Passage par valeur vs référence

**Règle absolue en Java :** TOUT est passé par valeur. MAIS :
- Pour les **types primitifs**, on copie la valeur → l'original n'est PAS modifié
- Pour les **références**, on copie l'adresse → on peut modifier l'**objet** mais pas le faire pointer ailleurs

```java
public class Test {
    public static void main(String[] args) {
        int x = 10;
        modifierPrimitif(x);
        System.out.println(x);  // 10 — PAS modifié !

        int[] tab = {1, 2, 3};
        modifierTableau(tab);
        System.out.println(tab[0]);  // 99 — modifié ! (objet modifié via la référence)

        modifierReference(tab);
        System.out.println(tab.length);  // 3 — toujours 3 ! PAS remplacé !
    }

    public static void modifierPrimitif(int a) {
        a = 999;  // on modifie la COPIE, l'original (x) reste 10
    }

    public static void modifierTableau(int[] t) {
        t[0] = 99;  // on modifie l'OBJET via la référence copiée
    }

    public static void modifierReference(int[] t) {
        t = new int[]{100, 200};  // on change la COPIE de la référence, pas l'original
    }
}
```

**Retenir :**
- `int` → jamais modifié par une méthode
- `int[]`, `ArrayList`, `StringBuilder`, vos objets → leur **contenu** peut être modifié
- Mais on ne peut pas remplacer l'objet lui-même pour l'appelant

---

## 11. Erreurs Java classiques

### Erreurs qui tombent systématiquement

```java
// 1. String dans int
int x = "10";
// ❌ Type mismatch: cannot convert from String to int
// ✅ int x = Integer.parseInt("10");

// 2. Point-virgule manquant
System.out.println("Bonjour")
// ❌ Syntax error
// ✅ System.out.println("Bonjour");

// 3. Guillemet fermant manquant
System.out.println("Bonjour);
// ❌ String literal is not properly closed
// ✅ System.out.println("Bonjour");

// 4. Variable non initialisée
String message;
if (message.equals("Bonjour")) { ... }
// ❌ The local variable may not have been initialized / NullPointerException
// ✅ String message = ""; ou String message = "Bonjour";

// 5. Index hors limites
int[] tab = new int[5];
tab[5] = 100;
// ❌ ArrayIndexOutOfBoundsException (taille=5, indices=0..4)
// ✅ tab[4] = 100;

// 6. Point-virgule après for qui tue la boucle
for (int i = 0; i < 5; i++);  // <- ce ;
{
    System.out.println("i = " + i);  // i inexistant ici !
}
// ✅ for (int i = 0; i < 5; i++) {
//        System.out.println("i = " + i);
//    }

// 7. main avec signature erronée
public static void main(String args)  // pas de []
// ✅ public static void main(String[] args)
// ✅ public static void main(String... args)

// 8. Appel de méthode non-static depuis static
public void afficherMessage() { ... }

public static void main(String[] args) {
    afficherMessage();
    // ❌ Cannot make a static reference to the non-static method
    // ✅ MonObjet obj = new MonObjet();
    //    obj.afficherMessage();
    // Ou ✅ déclarer la méthode static
}

// 9. == au lieu de .equals() pour les String
String s1 = "hello";
String s2 = new String("hello");
if (s1 == s2) { ... } // false !
// ✅ if (s1.equals(s2)) { ... }

// 10. Oubli de return dans une méthode qui doit retourner une valeur
public int calculer() {
    int x = 5;
    // ❌ missing return statement
}
// ✅ public int calculer() { return 5; }

// 11. Division entière
double d = 5 / 2;   // 2.0, pas 2.5 !
// ✅ double d = 5 / 2.0;  ou  double d = (double) 5 / 2;

// 12. = au lieu de == dans une condition
if (x = 5) { ... }  // ❌ (assigne 5 à x, ne compare pas)
// ✅ if (x == 5) { ... }
```

---

## 12. UML — Diagramme de classes

### Notation de base

```
┌──────────────────────┐
│     NomClasse        │ ← nom de la classe
├──────────────────────┤
│ - attribut: type     │ ← attributs avec visibilité
│ + attribut: type     │
├──────────────────────┤
│ + methode(): type    │ ← méthodes avec visibilité
│ - methode(param):type│
└──────────────────────┘
```

### Visibilité UML

| Symbole | Signification |
|---|---|
| `+` | public |
| `-` | private |
| `#` | protected |
| `~` | package (default) |

### Relations

**Héritage (extends)** — flèche vide vers le parent

```
       ┌──────────┐
       │ Animal   │
       └──────────┘
            △
            │
       ┌──────────┐
       │ Chien    │
       └──────────┘
```

**Implémentation (implements)** — flèche vide pointillée vers l'interface

```
       ┌──────────────┐
       │ <<interface>> │
       │   Volant     │
       └──────────────┘
            △
        ....│....
       ┌──────────┐
       │ Avion    │
       └──────────┘
```

**Association simple** — trait simple

```
┌────────┐          ┌────────┐
│ Client │──────────│ Adresse│
└────────┘          └────────┘
```
Un client a une adresse.

**Agrégation** (partie faible) — losange vide

```
┌──────────┐ ◇──────────┐
│ Université│           │ Professeur│
└──────────┘           └──────────┘
```
Un professeur peut exister sans l'université.

**Composition** (partie forte) — losange plein

```
┌──────────┐ ◆──────────┐
│ Maison   │           │  Pièce   │
└──────────┘           └──────────┘
```
Une pièce ne peut pas exister sans la maison (cycle de vie lié).

### Multiplicités

| Notation | Signification |
|---|---|
| `1` | Exactement un |
| `0..1` | Zéro ou un |
| `*` | Zéro ou plusieurs |
| `1..*` | Au moins un |
| `0..*` | = `*` |

### Exemple complet — système de réservation

```
┌────────────────────┐       ┌──────────────────────┐
│     Client         │       │     Réservation      │
├────────────────────┤       ├──────────────────────┤
│ - idClient: int    │1      │ - idResa: int        │
│ - nom: String      │◄──────│ - dateDebut: String  │
│ - prenom: String   │*      │ - dateFin: String    │
│ - email: String    │       │ - montant: double    │
├────────────────────┤       ├──────────────────────┤
│ + Client(...)      │       │ + Réservation(...)   │
│ + getters/setters  │       │ + getters/setters    │
│ + ajouterResa(...) │       └──────────────────────┘
└────────────────────┘                  │*
                                        │
                                        │
                              ┌─────────┴─────────┐
                              │     Chambre        │
                              ├───────────────────┤
                              │ - numChambre: int  │
                              │ - categorie: String│
                              │ - tarifNuit: double│
                              │ - disponible: bool │
                              ├───────────────────┤
                              │ + getters/setters  │
                              └───────────────────┘
```

### Méthode pour réussir l'UML

1. **Lire le sujet** et souligner TOUS les noms (classes potentielles)
2. **Identifier les attributs** (nom, date, numéro, etc.)
3. **Identifier les relations** (a un, contient, est un, utilise)
4. **Déterminer les multiplicités** (un client a combien de réservations ?)
5. **Ajouter les méthodes** listées dans le sujet (ajouter, consulter, etc.)
6. **Choisir héritage/interface** si des entités partagent des caractéristiques

**Pièges :**
- Ne pas oublier les attributs d'identification (id, numéro, code)
- Ne pas oublier les multiplicités des deux côtés
- Ne pas mettre de flèche là où il n'y a pas d'héritage
- Ne pas oublier le constructeur (requis pour créer des objets)

---

## Références rapides

### Syntaxe Java à retenir

```java
// Déclaration de classe
public class MaClasse {
    // attribut
    private int monAttribut;

    // constructeur
    public MaClasse(int valeur) {
        this.monAttribut = valeur;
    }

    // méthode
    public int getMonAttribut() {
        return this.monAttribut;
    }
}

// Instanciation
MaClasse obj = new MaClasse(42);

// Boucles
for (int i = 0; i < 10; i++) { }
int i = 0; while (i < 10) { i++; }
int i = 0; do { i++; } while (i < 10);
for (int val : monTableau) { }

// Conditions
if (x > 0) { } else if (x == 0) { } else { }
switch (x) { case 1: break; default: break; }

// Opérateurs
==    // égal (ATTENTION avec les String)
!=    // différent
&&    // ET logique
||    // OU logique
!     // NON logique
x++   // post-incrémentation (utilise x, puis incrémente)
++x   // pré-incrémentation (incrémente, puis utilise x)
a % b // modulo (reste de la division)
```

### Mots-clés importants

| Mot-clé | Utilisation |
|---|---|
| `new` | Créer un objet |
| `this` | Référence à l'objet courant |
| `super` | Référence à la classe parent |
| `null` | Absence de référence |
| `return` | Renvoyer une valeur |
| `void` | Aucun retour |
| `static` | Appartient à la classe |
| `final` | Ne peut pas être modifié/hérité/redéfini |
| `abstract` | Méthode sans corps / classe non instanciable |
| `extends` | Héritage |
| `implements` | Implémentation d'interface |
| `@Override` | Redéfinition d'une méthode héritée |

---

_Guide complet généré par Veille 🔧 — Pour l'examen II.1102 Algorithmique et Programmation_
