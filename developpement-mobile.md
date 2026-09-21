# Développement Mobile — Kotlin : Variables, Types, Opérateurs & Conditions

---

## 1. Déclaration des Variables : `val`, `var` et `const`

Kotlin propose une distinction stricte entre les références modifiables et non modifiables :

| Mot-clé | Mutabilité | Description & Comportement |

| :--- | :--- | :--- |

| **`val`** | Immuable (lecture seule) | La référence ne peut pas être réaffectée après son initialisation. Équivalent à `final` en Java ou `const` en JavaScript. À privilégier par défaut. |

| **`var`** | Mutable (modifiable) | La variable peut recevoir une nouvelle valeur à tout moment au fil de l'exécution du programme. |

| **`const val`** | Constante de compilation | Définie au moment de la compilation (*compile-time constant*). Réservée aux types primitifs et `String`, déclarée uniquement au niveau supérieur (*top-level*) ou dans un `companion object`. |

### Syntaxe générale

```kotlin

val nomVariable: Type = valeur

```

### Exemples

```kotlin

// Variable immuable avec type explicite

val campusName: String = "Campus"

  

// Variable réaffectable

var score: Int = 10

score = 25 // Valide car déclarée avec var

  

// Inférence de type (Kotlin déduit automatiquement le type String)

val city = "Lille"

```

---
## 2. Règles de Syntaxe & Conventions

* **Points-virgules (`;`)** : Facultatifs en fin de ligne. Ils ne sont nécessaires que si plusieurs instructions sont écrites sur la même ligne (ex. `val a = 5; val b = 10`).

* **Conventions de nommage** : Le `camelCase` est la convention standard recommandée pour nommer variables et fonctions (ex. `campusName`, `userAge`, `isValid`).

* **Inférence de type** : Si la variable est immédiatement initialisée, préciser le type (`: String`, `: Int`) est optionnel, le compilateur le déduisant directement.

---
## 3. Types de Données Primitifs & Gabarits de Chaînes

En Kotlin, tout est manipulé comme un objet (pas de types primitifs avec minuscule comme le `int` en Java), mais le compilateur optimise le code machine sous le capot pour préserver les performances.

| Type | Rôle | Format & Exemples |

| :--- | :--- | :--- |

| **`String`** | Chaîne de caractères (texte) | Entouré de guillemets doubles : `"Campus"`, `"Android"` |

| **`Int`** | Nombre entier (32 bits) | `42`, `-7`, `1000` |

| **`Long`** | Entier grand format (64 bits) | `3000000000L` (suffixe `L`) |

| **`Double`** | Nombre décimal standard (64 bits, virgule flottante) | `3.14`, `19.99`, `-0.5` |

| **`Float`** | Nombre décimal simple précision (32 bits) | `3.14f`, `10.0f` (suffixe `f`) |

| **`Boolean`** | Valeur booléenne / logique | `true` ou `false` (obligatoirement en minuscules) |

  

> **Attention à la casse des booléens :** En Kotlin, la syntaxe impose les minuscules : `true` et `false` (contrairement à Python où l'on écrit `True` et `False`).

### Gabarits de chaînes (*String Templates*)

Kotlin permet d'insérer des variables ou expressions directement dans une chaîne via le caractère `$` :

```kotlin

val user = "Alex"

val age = 20

  

// Insertion directe d'une variable

println("Bonjour $user !")

  

// Évaluation d'une expression avec ${}

println("Dans un an, tu auras ${age + 1} ans.")

```

---
## 4. Opérateurs de Comparaison

Ces opérateurs évaluent deux opérandes et renvoient un résultat booléen (`true` ou `false`) :

| Opérateur | Signification | Exemple | Résultat |

| :---: | :--- | :--- | :---: |

| `==` | Égalité de valeur (compare le contenu) | `5 == 5` | `true` |

| `!=` | Différent de / Inégalité | `5 != 3` | `true` |

| `<` | Strictement inférieur | `10 < 20` | `true` |

| `<=` | Inférieur ou égal | `10 <= 10` | `true` |

| `>` | Strictement supérieur | `15 > 30` | `false` |

| `>=` | Supérieur ou égal | `20 >= 18` | `true` |

| `===` | Égalité référentielle (même instance en mémoire) | `objA === objB` | Dépend du pointeur |

---

## 5. Opérateurs Logiques (Combiner les Conditions)

Permettent d'associer ou inverser plusieurs conditions booléennes :

* **`&&` (ET logique)** : Renvoie `true` si et seulement si toutes les conditions sont vraies.

* **`||` (OU logique)** : Renvoie `true` si au moins une des conditions est vraie.

* **`!` (NON logique / Négation)** : Inverse l'état d'un booléen (`!true` donne `false`, `!false` donne `true`).

```kotlin

val age = 22

val hasTicket = true

  

// Combinaison ET (&&)

if (age >= 18 && hasTicket) {

    println("Accès autorisé")

}

  

// Inversion logique (!)

val isBlocked = false

if (!isBlocked) {

    println("Le compte est actif")

}

```

---
## 6. Structure Conditionnelle : `if` / `else`

Le mot-clé `if` exécute un bloc de code uniquement si la condition fournie est vraie. `else` (ou `else if`) permet de définir le comportement alternatif ("sinon").

En Kotlin, `if` fonctionne sous deux formes :

1. Comme une **instruction** classique (exécuter des actions).

2. Comme une **expression** (produire et renvoyer une valeur).

### A. Utilisation classique (Instruction)

```kotlin

val batteryLevel = 15

  

if (batteryLevel <= 10) {

    println("Batterie critique, passage en mode économie")

} else if (batteryLevel <= 20) {

    println("Batterie faible")

} else {

    println("Niveau de batterie optimal")

}

```

### B. Le `if` en tant qu'expression (Remplacement du ternaire)

Contrairement à d'autres langages (Java, C, JS), le `if` en Kotlin peut être utilisé pour produire un résultat et l'affecter directement à une variable ou le renvoyer. Cela remplace directement l'opérateur ternaire classique (`condition ? true : false`) qui n'existe pas en Kotlin :

```kotlin

val a = 12

val b = 25

  

// Affectation directe du résultat de la condition

val max: Int = if (a > b) a else b

  

println("Le plus grand nombre est : $max") // 25

```
### C. Blocs de code multilignes (`{ }`)

Si la condition contient plusieurs lignes d'instructions, la dernière ligne exécutée dans le bloc devient la valeur produite par ce bloc :

```kotlin

val score = 85

  

val appreciation: String = if (score >= 90) {

    println("Calcul pour mention excellente...")

    "Excellent" // Valeur produite

} else if (score >= 70) {

    println("Calcul pour mention honorable...")

    "Bien" // Valeur produite

} else {

    println("Calcul pour rattrapage...")

    "Insuffisant" // Valeur produite

}

  

println(appreciation) // Affiche : Bien

```
### D. Règle obligatoire : le `else`

Quand le `if` est utilisé comme une expression (pour assigner une variable ou retourner une valeur), **la branche `else` est obligatoire**. Le compilateur doit avoir la garantie absolue qu'une valeur sera produite, quel que soit le résultat du test conditionnel.


```kotlin

// ❌ Erreur de compilation : 'if' must have both main and 'else' branches if used as an expression

val message = if (isLogged) "Connecté"

  

// ✅ Valide

val message = if (isLogged) "Connecté" else "Déconnecté"

```

---
## 7. La Structure `when` (Le remplaçant moderne du `switch`)


En Kotlin, le mot-clé `when` remplace le traditionnel `switch` que l'on trouve en C, Java ou JavaScript.

Il est plus puissant, plus lisible et élimine le piège des `break` obligatoires (il s'arrête automatiquement dès qu'une branche correspond).

  
Comme pour le `if`, le `when` peut être utilisé :

* Comme une instruction (exécuter du code selon un cas).

* Comme une expression (produire et retourner une valeur).

### A. Utilisation classique (Instruction)

Chaque branche s'écrit avec la flèche `->` :


```kotlin

val statusCode = 404

  

when (statusCode) {

    200 -> println("Succès : OK")

    201 -> println("Succès : Ressource créée")

    400, 422 -> println("Erreur client : Requête invalide") // Plusieurs valeurs séparées par une virgule

    404 -> println("Erreur : Non trouvé")

    else -> println("Code d'erreur non géré")

}

```


* **Pas de mot-clé `break`** : Seule la première branche vraie est exécutée.

* **`else`** : Équivalent du `default` dans un switch.

### B. Le `when` en tant qu'expression (production d'une valeur)

  
Le `when` peut évaluer et affecter directement une valeur à une variable :


```kotlin

val role = "admin"

  

// when produit directement la valeur stockée dans 'accessLevel'

val accessLevel: Int = when (role) {

    "superadmin" -> 1

    "admin" -> 2

    "editor" -> 3

    else -> 4 // Obligatoire si utilisé en expression

}

  

println("Niveau d'accès : $accessLevel") // Affiche : Niveau d'accès : 2

```
  

> **Obligation d'exhaustivité :** Utilisé en expression, le `when` doit être exhaustif : toutes les possibilités doivent être couvertes (généralement en ajoutant une branche `else`, ou sans `else` s'il teste un `enum` ou une `sealed class` dont tous les cas sont listés).

### C. Vérification d'intervalles (`in`) et de types (`is`)

Le `when` de Kotlin accepte des conditions bien plus avancées que de simples égalités de valeurs :

#### 1. Tester un intervalle numérique avec `in` / `!in`


```kotlin

val note = 14

  

val resultat = when (note) {

    in 16..20 -> "Très bien"

    in 12..15 -> "Bien"

    in 10..11 -> "Moyen"

    !in 0..20 -> "Note invalide"

    else -> "Insuffisant"

}

```

#### 2. Tester le type d'un objet avec `is` (Smart Cast automatique)


```kotlin

val donnee: Any = "Campus"

  

when (donnee) {

    is String -> println("C'est une chaîne de ${donnee.length} caractères") // Cast automatique en String !

    is Int -> println("C'est un entier : ${donnee * 2}")

    is Boolean -> println("C'est un booléen")

    else -> println("Type inconnu")

}

```

### D. Le `when` sans argument (remplaçant des `if` / `else if` en cascade)

Si tu n'indiques aucun paramètre entre parenthèses, le `when` agit comme une suite de tests booléens indépendants :
  

```kotlin

val temperature = 28

val isRaining = false

  

when {

    temperature > 30 && !isRaining -> println("Canicule")

    temperature in 20..30 && !isRaining -> println("Temps idéal")

    isRaining -> println("Prenez un parapluie")

    else -> println("Temps frais")

}

```
  

> **Règle du bloc multiligne dans une branche :** Tout comme avec `if`, si une branche d'un `when` contient plusieurs lignes entre `{ }`, la dernière ligne exécutée est la valeur produite par l'expression.


---
## 8. Cas Pratique : Comparatif `if` vs `when` (sans argument)


Voici l'application directe des deux approches pour calculer un libellé de tarif selon le prix :

```kotlin

fun main(){

val price = 8.0

  

val priceLabel = if (price == 0.0){

    "Gratuit"

} else if(price <10.0){

    "Petit Prix"

} else {

    "Plein Trarif"

}

  

println(priceLabel)

  
  

val priceLabel2 = when {

    price == 0.0 ->"Gratuit"

    price < 10.0 -> "Petit Prix"

    else -> "Plein Prix"

}

println(priceLabel2)

}

```

### Points clés de cet exemple :

1. **Assignation directe (`val priceLabel = if (...)`)** : chaque bloc conditionnel renvoie sa dernière ligne (`"Gratuit"`, `"Petit Prix"`, `"Plein Trarif"`).

2. **Alternative avec `when { ... }`** : l'absence d'argument permet d'évaluer chaque ligne comme un prédicat booléen (`price == 0.0`, `price < 10.0`), offrant une syntaxe plus aérée et directe que l'enchaînement de `else if`.

---

## 9. Les Fonctions (`fun`)

En Kotlin, une fonction se déclare avec le mot-clé `fun`. Les paramètres doivent obligatoirement être typés, et le type de retour est indiqué après les parenthèses, précédé de deux-points `:`.

### A. Syntaxe générale & Fonction standard

```kotlin
fun nomDeLaFonction(param1: Type, param2: Type): TypeDeRetour {
    // Instructions
    return valeurDeRetour
}
```

* Si la fonction ne renvoie aucune valeur utile, son type de retour est `Unit` (l'équivalent de `void` en Java ou C). Il est facultatif de l'écrire.

```kotlin
// Fonction avec retour explicite
fun addition(a: Int, b: Int): Int {
    return a + b
}

// Fonction sans retour (Unit implicite)
fun afficherMessage(nom: String) {
    println("Bonjour $nom !")
}
```

---

### B. Fonctions à expression unique (*Single-Expression Functions*)

Quand une fonction se résume à une seule expression ou un calcul direct, on peut omettre les accolades `{}` et le mot-clé `return`. Le compilateur déduit automatiquement le type de retour :

```kotlin
// Syntaxe ultra-compacte
fun multiplier(x: Int, y: Int) = x * y

// Équivalent avec le if-expression
fun trouverMax(a: Int, b: Int) = if (a > b) a else b
```

---

### C. Valeurs par défaut & Arguments nommés

Kotlin permet d'attribuer des valeurs par défaut aux paramètres pour éviter de surcharger inutilement des méthodes :

```kotlin
fun creerProfil(nom: String, role: String = "Membre", estActif: Boolean = true) {
    println("Utilisateur : $nom | Rôle : $role \vert{} Actif :$estActif")
}

// Utilisation avec valeurs par défaut
creerProfil("Camille") // Rôle = "Membre", estActif = true

// Utilisation avec arguments nommés (l'ordre n'a plus d'importance)
creerProfil(role = "Admin", nom = "Alex")
```
### D. Exemple pratique : Fonction avec retour conditionnel et arguments nommés

Une fonction peut directement retourner le résultat d'une structure `if` / `else`[cite: 2]. Lors de l'appel, les arguments peuvent être passés par position ou nommés explicitement (`nomDuParametre = valeur`)[cite: 2] :
### D. Exemple pratique : Fonction avec retour conditionnel et arguments nommés

Une fonction peut directement retourner le résultat d'une structure `if` / `else`[cite: 2]. Lors de l'appel, les arguments peuvent être passés par position ou nommés explicitement (`nomDuParametre = valeur`)[cite: 2] :

```kotlin
fun main() {
    println(priceLabel(0.0))
    println(priceLabel(price = 0.0))
}

fun priceLabel(price: Double): String {
    return if (price == 0.0) {
        "Gratuit"
    } else {
        price.toString()
    }
}
```

> **À noter :**
> * L'appel `priceLabel(0.0)` utilise le passage de paramètre classique (par position)[cite: 2].
> * L'appel `priceLabel(price = 0.0)` utilise un argument nommé, ce qui explicite le paramètre ciblé[cite: 2].
> * `return if (...)` renvoie directement la valeur calculée par la branche correspondante de l'expression conditionnelle[cite: 2].
> * Dans la branche `else`, l'appel `price.toString()` est équivalent à l'utilisation d'une chaîne interpolée `"$price"`.
