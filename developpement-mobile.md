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


### E. Les Expressions Lambdas : Stocker une fonction dans une variable

En Kotlin, les fonctions sont des "citoyens de première classe" (*first-class citizens*). Cela signifie qu'on peut stocker une fonction anonyme (une lambda) directement dans une variable, la passer en paramètre ou la retourner.

#### 1. Syntaxe générale d'une lambda
Une lambda est toujours entourée d'accolades `{}`. Les paramètres sont placés à gauche de la flèche `->`, et le corps de la fonction à droite :

```kotlin
val nomVariable = { param1: Type, param2: Type -> 
    // Corps de la fonction (la dernière ligne est la valeur retournée)
}
```

---

#### 2. Déclaration avec typage de la variable
On peut expliciter le type fonctionnel de la variable sous la forme `(TypesParamètres) -> TypeRetour` :

```kotlin
// Variable stockant une fonction prenant un Double et retournant un String
val formatPrice: (Double) -> String = { price ->
    if (price == 0.0) "Gratuit" else "$price €"
}

// Appel de la lambda comme une fonction classique
println(formatPrice(0.0))  // Affiche : Gratuit
println(formatPrice(12.5)) // Affiche : 12.5 €
```

---

#### 3. Le mot-clé implicite `it` (paramètre unique)
Lorsqu'une lambda ne prend qu'**un seul paramètre**, Kotlin permet d'omettre la déclaration du paramètre et la flèche `->`. Le paramètre devient alors accessible via le mot-clé réservé `it` :

```kotlin
// Sans expliciter le nom du paramètre grâce à 'it'
val square: (Int) -> Int = { it * it }

println(square(5)) // Affiche : 25

// Exemple appliqué au formatage de texte
val toUpper: (String) -> String = { it.uppercase() }
println(toUpper("android")) // Affiche : ANDROID
```

### F. La Trailing Lambda (Syntaxe de la lambda en dernier paramètre)

En Kotlin, lorsqu’une fonction accepte une lambda comme **dernier paramètre**, le langage autorise une convention de syntaxe très populaire (notamment utilisée par Jetpack Compose et les fonctions de collections) : **la lambda peut être sortie des parenthèses**.

---

#### 1. Sortie des parenthèses
Si la lambda est le dernier argument de la fonction, on peut fermer les parenthèses avant la lambda et écrire celle-ci directement après :

```kotlin
// Déclaration d'une fonction acceptant une lambda en dernier paramètre
fun appliquerReduction(prix: Double, calcul: (Double) -> Double): Double {
    return calcul(prix)
}

// 1. Syntaxe classique (lambda à l'intérieur des parenthèses)
val prix1 = appliquerReduction(100.0, { it * 0.8 })

// 2. Syntaxe avec Trailing Lambda (recommandée)
val prix2 = appliquerReduction(100.0) { it * 0.8 }
```

---

#### 2. Omission complète des parenthèses
Si la lambda est le **seul et unique paramètre** de la fonction, les parenthèses `()` deviennent totalement superflues et peuvent être supprimées :

```kotlin
fun executerAction(action: () -> Unit) {
    action()
}

// Les parenthèses sont omises :
executerAction {
    println("Tâche en cours d'exécution...")
}
```

---

#### 3. Cas d'usage courant sur Android : Collections et Compose
Cette convention est la clé de la lisibilité du code Kotlin moderne :

```kotlin
val prixList = listOf(5.0, 12.0, 20.0, 0.0)

// filter et map utilisent la syntaxe trailing lambda
val articlesPayants = prixList
    .filter { it > 0.0 }
    .map { "$it €" }

println(articlesPayants) // [5.0 €, 12.0 €, 20.0 €]
```

---

## 10. Les Classes & la Programmation Orientée Objet

En Kotlin, les classes se déclarent avec le mot-clé `class`. La syntaxe est concise : les propriétés et le constructeur principal peuvent être définis directement dans l'en-tête de la classe.

---

### A. Déclaration de base & Constructeur principal (*Primary Constructor*)

Pas besoin d'écrire de boilerplate (getters, setters ou constructeurs verbeux comme en Java). Les propriétés se déclarent directement entre les parenthèses de la classe en utilisant `val` (lecture seule) ou `var` (modifiable) :

```kotlin
// Déclaration d'une classe avec son constructeur principal
class Article(
    val id: Int,
    var name: String,
    var price: Double = 0.0 // Possibilité d'attribuer une valeur par défaut
)

fun main() {
    // Instanciation : pas de mot-clé 'new' en Kotlin !
    val article1 = Article(1, "Clavier mécanique", 79.99)
    val article2 = Article(2, "Sticker", price = 0.0)

    // Accès aux propriétés
    println(article1.name) // Clavier mécanique
    
    // Modification (uniquement possible sur les propriétés 'var')
    article1.price = 69.99
    // article1.id = 5 // ❌ Erreur : 'id' est déclaré avec 'val'
}
```

---

### B. Le bloc d'initialisation : `init`

Le constructeur principal ne contient pas de bloc de code `{}` pour exécuter des instructions. Si des vérifications ou des calculs sont nécessaires au moment de la création de l'objet, on utilise le mot-clé `init` :

```kotlin
class Utilisateur(val username: String, val age: Int) {
    
    init {
        // Exécuté immédiatement à l'instanciation
        require(age >= 0) { "L'âge ne peut pas être négatif !" }
        println("Compte créé pour $username ($age ans)")
    }
}
```

---

### C. Méthodes à l'intérieur d'une classe

Les fonctions déclarées à l'intérieur d'une classe définissent les comportements de l'objet :

```kotlin
class Compteur(var total: Int = 0) {

    fun incrementer() {
        total++
    }

    fun afficherTotal(): String {
        return "Total actuel : $total"
    }
}
```

---

### D. Les `data class` (Classes de données)

Pour les classes dont le but principal est de **transporter de la donnée** (modèles d'API, entités de base de données), Kotlin propose le mot-clé `data class`.

```kotlin
data class Produit(val id: Int, val nom: String, val prix: Double)
```

Le préfixe `data` génère automatiquement et gratuitement sous le capot :
* Une méthode `toString()` lisible : `Produit(id=1, nom=Café, prix=2.5)` au lieu de l'adresse mémoire.
* La méthode `equals()` et `hashCode()` pour comparer le **contenu** de deux objets avec `==`.
* La méthode `.copy()` pour dupliquer facilement un objet en modifiant seulement certains champs :

```kotlin
val p1 = Produit(1, "Café", 2.5)
val p2 = Produit(1, "Café", 2.5)

println(p1 == p2) // true (compare les valeurs des champs, pas la référence mémoire)

// Création d'une copie avec modification d'un attribut
val p3 = p1.copy(prix = 3.0)
println(p3) // Produit(id=1, nom=Café, prix=3.0)
```

---

## 11. Design Pattern : Le Singleton avec `object`

Le patron de conception **Singleton** garantit qu'une classe n'a qu'**une seule instance** active en mémoire tout au long du cycle de vie de l'application, et fournit un point d'accès global à cette instance.

Contrairement à Java ou d'autres langages qui imposent des constructeurs privés, des variables statiques et une synchronisation multithread manuelle, Kotlin intègre le Singleton nativement via le mot-clé **`object`**.

---

### A. Déclaration et utilisation de l'objet Singleton

Il suffit de remplacer `class` par `object`. L'instanciation est automatique, différée (*lazy*) et garantie sans conflit de thread (*thread-safe*) par le runtime :

```kotlin
// Déclaration du Singleton
object DatabaseManager {
    val databaseName: String = "AppDatabase.db"
    private var isConnected: Boolean = false

    fun connect() {
        if (!isConnected) {
            isConnected = true
            println("Connexion établie à $databaseName")
        }
    }

    fun disconnect() {
        isConnected = false
        println("Déconnexion réussie")
    }
}

fun main() {
    // Accès direct sans instanciation (pas de 'new', pas de DatabaseManager())
    DatabaseManager.connect()
    println(DatabaseManager.databaseName)
    DatabaseManager.disconnect()
}
```

---

### B. Le `companion object` (Équivalent du `static` en Java)

Si une classe doit posséder des méthodes ou variables accessibles sans instancier la classe elle-même (comme des factory methods ou des constantes), on utilise un `companion object` au sein de cette classe :

```kotlin
class User(val id: Int, val name: String) {

    // Singleton attaché à la classe User
    companion object {
        const val MIN_NAME_LENGTH = 3

        fun createGuest(): User {
            return User(id = 0, name = "Invité")
        }
    }
}

fun main() {
    // Accès direct via le nom de la classe
    val guest = User.createGuest()
    println("Longueur min : ${User.MIN_NAME_LENGTH}")
    println(guest.name) // Invité
}
```

---

## 12. Les Classes de Données : `data class`

En développement d'applications (particulièrement sous Android), une grande partie du code consiste à manipuler des modèles de données (réponses d'API REST, entités SQL, états d'interface). Le mot-clé **`data class`** évite d'écrire manuellement des dizaines de lignes de code répétitif.

---

### A. Règles de déclaration

Pour créer une `data class`, quelques contraintes s'appliquent :
* Le constructeur principal doit comporter au moins un paramètre.
* Tous les paramètres du constructeur principal doivent obligatoirement être préfixés par `val` ou `var`.
* La classe ne peut pas être `abstract`, `open`, `sealed` ou `inner`.

```kotlin
data class Product(
    val id: Int,
    val title: String,
    val price: Double,
    val isAvailable: Boolean = true
)
```

---

### B. Fonctionnalités générées automatiquement

Dès qu'une classe est marquée avec `data`, Kotlin implémente automatiquement en arrière-plan :

| Méthode générée | Rôle & Comportement |
| :--- | :--- |
| **`toString()`** | Affiche le contenu lisible de l'objet : `Product(id=1, title=Livre, ...)` au lieu d'une adresse mémoire comme `Product@6f494a6`. |
| **`equals()` / `==`** | Compare la **valeur de chaque champ** et non la référence mémoire de l'objet. |
| **`hashCode()`** | Calcule un code de hachage cohérent avec `equals` pour l'utilisation dans des `Set` ou `Map`. |
| **`.copy()`** | Permet de cloner un objet tout en modifiant à la volée une ou plusieurs propriétés. |
| **`componentN()`** | Autorise la **déstructuration** de l'objet dans des variables distinctes. |

---

### C. Exemple d'usage concret

```kotlin
fun main() {
    val articleA = Product(id = 101, title = "Clavier", price = 49.99)
    val articleB = Product(id = 101, title = "Clavier", price = 49.99)

    // 1. Affichage propre
    println(articleA) 
    // Sortie : Product(id=101, title=Clavier, price=49.99, isAvailable=true)

    // 2. Égalité structurelle (contenu identique)
    println(articleA == articleB) // true

    // 3. Duplication avec modification ciblée (.copy)
    val articleSolde = articleA.copy(price = 39.99)
    println(articleSolde) 
    // Sortie : Product(id=101, title=Clavier, price=39.99, isAvailable=true)

    // 4. Déstructuration (extraction directe des valeurs)
    val (id, title, price) = articleSolde
    println("Article $id :$title coûte $price €")
}
```

### D. La Déstructuration d'une `data class` (*Destructuring Declaration*)

La déstructuration permet de décomposer une instance d'une `data class` en plusieurs variables distinctes en une seule ligne d'instruction.

---

#### 1. Fonctionnement sous le capot (`componentN()`)
Pour chaque propriété déclarée dans le constructeur principal d'une `data class`, le compilateur Kotlin génère automatiquement des méthodes appelées `component1()`, `component2()`, `component3()`, etc., selon **l'ordre de déclaration des attributs**.

```kotlin
data class User(val id: Int, val username: String, val email: String)

fun main() {
    val user = User(1, "alex", "alex@example.com")

    // Syntaxe de déstructuration
    val (userId, userName, userEmail) = user

    // Ce que fait Kotlin en arrière-plan :
    // val userId = user.component1()
    // val userName = user.component2()
    // val userEmail = user.component3()

    println("ID: $userId, Pseudo: $userName, Contact: $userEmail")
}
```

> **Attention à l'ordre :** La déstructuration se base sur **l'ordre des propriétés dans le constructeur**, et non sur le nom des variables réceptrices.

---

#### 2. Ignorer des propriétés avec l'underscore (`_`)
Si seules certaines propriétés t'intéressent, tu peux ignorer les autres en utilisant le tiret bas `_` pour éviter d'allouer des variables inutiles :

```kotlin
val user = User(2, "camille", "camille@example.com")

// On ignore l'id et on ne récupère que le username et l'email
val (_, name, mail) = user

println("Nom : $name, Email : $mail")
```

---

#### 3. Cas d'usage : Parcourir une liste ou une `Map`
La déstructuration est particulièrement efficace dans les boucles pour manipuler directement les champs d'objets ou les paires clé/valeur :

```kotlin
val users = listOf(
    User(1, "alex", "alex@test.com"),
    User(2, "sara", "sara@test.com")
)

// Déstructuration directe dans la boucle for
for ((id, username) in users) {
    println("Utilisateur n°$id -> $username")
}

// Exemple avec une Map (clé, valeur)
val credentials = mapOf("admin" to "secret123", "guest" to "guest123")
for ((login, mdp) in credentials) {
    println("Compte : $login")
}
```

---

#### 4. Déstructuration dans une Lambda
On peut aussi déstructurer directement le paramètre d'une fonction lambda en entourant les champs de parenthèses `(...)` :

```kotlin
val articles = listOf(
    Product(1, "Clavier", 49.99),
    Product(2, "Souris", 19.99)
)

// Déstructuration directe dans le paramètre de la lambda
articles.forEach { (id, title, price) ->
    println("$title (#$id) coûte $price €")
}
```

---

## 13. Les Fonctions d'Extension (*Extension Functions*)

Kotlin permet d'étendre une classe existante en lui ajoutant de nouvelles fonctionnalités **sans avoir besoin d'hériter de cette classe ni de modifier son code source d'origine**. 

Cette mécanique est particulièrement utilisée en développement Android pour enrichir les classes du framework (ex. `Context`, `View`, `String`, `Double`).

---

### A. Syntaxe générale

Pour déclarer une extension, on préfixe le nom de la fonction par le **type récepteur** (*Receiver Type*), c'est-à-dire la classe à laquelle on souhaite greffer la méthode. 

À l'intérieur du corps de la fonction, le mot-clé **`this`** fait référence à l'instance courante de cet objet :

```kotlin
fun TypeRecepteur.nomDeFonction(parametres): TypeRetour {
    // 'this' représente l'objet sur lequel la méthode est appelée
    return valeur
}
```

---

### B. Exemple concret sur un type primitif (`Double`)

On peut par exemple ajouter une méthode de mise en forme monétaire directement sur tous les `Double` :

```kotlin
// Déclaration de la fonction d'extension sur Double
fun Double.toEuro(): String {
    return String.format("%.2f €", this)
}

fun main() {
    val prix = 19.9
    
    // Appel de l'extension comme si elle appartenait nativement à Double
    println(prix.toEuro()) // Affiche : 19,90 €
    println(4.5.toEuro())  // Affiche : 4,50 €
}
```

---

### C. Exemple concret sur `String`

```kotlin
// Extension pour vérifier si une chaîne commence par une majuscule
fun String.isCapitalized(): Boolean {
    return this.isNotEmpty() && this[0].isUpperCase()
}

fun main() {
    val ville = "Lille"
    println(ville.isCapitalized()) // true

    val code = "mobile"
    println(code.isCapitalized()) // false
}
```

---

### D. Propriétés d'extension (*Extension Properties*)

De la même manière qu'une fonction, on peut ajouter des propriétés calculées à une classe existante en définissant un `getter` personnalisé :

```kotlin
// Ajout d'une propriété calculée sur String
val String.halfLength: Int
    get() = this.length / 2

fun main() {
    val mot = "Android"
    println(mot.halfLength) // 3
}
```

> **Règle importante :** Une propriété d'extension ne possède pas de champ de stockage en mémoire (*backing field*). Elle doit obligatoirement être calculée via un `get()` explicite et ne peut pas être initialisée avec `= valeur`.

---

### E. Résolution statique : Ce qu'il faut savoir

Les fonctions d'extension ne modifient pas la classe réelle : elles sont résolues **statiquement** à la compilation. 
* Si une classe possède déjà une méthode membre ayant exactement la même signature (même nom et mêmes paramètres), **la méthode membre gagne toujours** et l'extension est ignorée.
* Une extension ne peut pas accéder aux membres privés (`private`) ou protégés (`protected`) de la classe ciblée.

### F. Utilisation de l'objet courant (`this`) dans une extension

Dans le corps d'une fonction d'extension, l'instance de la classe sur laquelle la méthode est appelée est désignée comme le **récepteur** (*receiver object*).

---

#### 1. Le mot-clé `this`
On utilise **`this`** pour manipuler explicitement l'objet courant et accéder à ses propriétés ou méthodes publiques :

```kotlin
fun String.entourerDeGuillemets(): String {
    // 'this' représente la chaîne appelante
    return "\"$this\""
}

println("Bonjour".entourerDeGuillemets()) // "Bonjour"
```

---

#### 2. Omission facultative de `this` (accès implicite)
Comme dans une méthode de classe classique, le mot-clé `this` est **implicite**. Tu peux appeler directement les fonctions membres et propriétés de l'objet sans le préfixer :

```kotlin
// Avec 'this' explicite
fun String.estVideOuCourt(): Boolean {
    return this.isEmpty() || this.length < 3
}

// Version identique avec 'this' implicite (plus idiomatic Kotlin)
fun String.estVideOuCourt(): Boolean {
    return isEmpty() || length < 3
}
```

---

#### 3. Levée d'ambiguïté avec label (`this@Nom`)
Si l'extension se trouve à l'intérieur d'une autre classe ou d'une lambda et qu'il y a conflit de nom, on utilise un label pour préciser quel `this` cibler :

```kotlin
class Panier {
    val remise = 0.10

    fun Double.appliquerRemise(): Double {
        // this -> le Double (le prix)
        // this@Panier -> l'instance englobante de la classe Panier
        return this * (1.0 - this@Panier.remise)
    }
}
```

### G. Le Chaînage des Fonctions d'Extension (*Chaining*)

Comme chaque fonction d'extension peut renvoyer un nouvel objet (ou le même objet transformé), il est possible de les **enchaîner à la suite** avec l'opérateur point `.`. 

Cela permet d'écrire des pipelines de transformation de données lisibles de gauche à droite (ou de haut en bas), sans imbrication de parenthèses illisibles.

---

#### 1. Exemple simple : Nettoyage et formatage d'un texte

Au lieu d'imbriquer des appels de fonctions classiques comme `ajouterPrefixe(nettoyerEspaces(texte))`, on enchaîne les extensions :

```kotlin
// 1re extension : supprime les espaces superflus et met en minuscule
fun String.nettoyer(): String = this.trim().lowercase()

// 2de extension : ajoute un préfixe
fun String.avecPrefixe(prefixe: String): String = "$prefixe: $this"

fun main() {
    val saisie = "   MonIdentifiant_123   "

    // Chaînage fluide des deux extensions
    val resultat = saisie
        .nettoyer()
        .avecPrefixe("USER")

    println(resultat) // Affiche : USER: monidentifiant_123
}
```

---

#### 2. Exemple métier : Pipeline de calcul sur des prix

Le type de retour de chaque fonction devient le type récepteur de la fonction suivante dans la chaîne :

```kotlin
// Double -> Double (applique un pourcentage de réduction)
fun Double.remise(pourcentage: Double): Double = this * (1.0 - pourcentage / 100.0)

// Double -> Double (ajoute les frais de port)
fun Double.avecLivraison(frais: Double): Double = this + frais

// Double -> String (met en forme pour l'affichage)
fun Double.enEuros(): String = String.format("%.2f €", this)

fun main() {
    val prixInitial = 100.0

    // Pipeline : remise -> livraison -> formatage final en String
    val totalAffiche = prixInitial
        .remise(20.0)         // 80.0
        .avecLivraison(4.99)  // 84.99
        .enEuros()            // "84,99 €"

    println(totalAffiche) // Affiche : 84,99 €
}
```

---

#### 3. Chaînage avec les fonctions de portée (*Scope Functions*)
Kotlin intègre déjà des fonctions d'extension chaînables sur tous les objets (`let`, `also`, `apply`, `run`) pour effectuer des opérations intermédiaires :

```kotlin
fun Double.appliquerTva(): Double = this * 1.20

fun main() {
    val total = 50.0
        .appliquerTva()
        .also { println("Total intermédiaire TTC : $it €") } // Inspection sans casser la chaîne
        .enEuros()

    println("Affichage final : $total")
}
```
