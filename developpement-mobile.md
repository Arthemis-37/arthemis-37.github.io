# Développement Mobile — Kotlin : Variables, Types, Opérateurs, Fonctions & Collections

---

## 1. Déclaration des Variables : `val`, `var` et `const`

Kotlin impose une distinction stricte entre les références modifiables et non modifiables :

| Mot-clé | Mutabilité | Description & Comportement |
| :--- | :--- | :--- |
| **`val`** | **Immuable** (lecture seule) | La référence ne peut pas être réaffectée après son initialisation. Équivalent à `final` en Java ou `const` en JavaScript. **À privilégier par défaut.** |
| **`var`** | **Mutable** (modifiable) | La variable peut recevoir une nouvelle valeur à tout moment au fil de l'exécution du programme. |
| **`const val`** | **Constante de compilation** | Définie au moment de la compilation (*compile-time constant*). Réservée aux types primitifs et `String`, déclarée uniquement au niveau supérieur (*top-level*) ou dans un `companion object`. |

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
* **Conventions de nommage** : Le `camelCase` est la convention standard recommandée pour nommer les variables et les fonctions (ex. `campusName`, `userAge`, `isValid`).
* **Inférence de type** : Si la variable est immédiatement initialisée, préciser le type (`: String`, `: Int`) est optionnel, le compilateur le déduisant directement.

---

## 3. Types de Données Primitifs & Interpolation

En Kotlin, tout est manipulé comme un objet (il n'y a pas de types primitifs écrits en minuscule comme `int` en Java), mais le compilateur optimise le tout en types primitifs sous le capot pour les performances.

| Type | Rôle | Format & Exemples |
| :--- | :--- | :--- |
| **`String`** | Chaîne de caractères (texte) | Entouré de guillemets doubles : `"Campus"`, `"Android"` |
| **`Int`** | Nombre entier (32 bits) | `42`, `-7`, `1000` |
| **`Long`** | Nombre entier long (64 bits) | `3000000000L` (suffixé par `L`) |
| **`Double`** | Nombre décimal (64 bits, standard) | `3.14`, `19.99`, `-0.5` |
| **`Float`** | Nombre décimal simple précision (32 bits) | `3.14f`, `10.0f` (suffixé par `f`) |
| **`Boolean`** | Valeur logique | `true` ou `false` (obligatoirement en minuscules) |

> **Attention à la casse des booléens :** En Kotlin, la syntaxe impose les minuscules (`true` et `false`), contrairement à Python (`True`, `False`).

### Les gabarits de chaînes (*String Templates*)
Kotlin permet d'insérer directement des variables ou expressions dans une chaîne grâce au symbole `$` :
```kotlin
val user = "Alex"
val age = 20

// Insertion directe d'une variable
println("Bonjour $user !") 

// Évaluation d'une expression avec ${ }
println("Dans un an, tu auras ${age + 1} ans.")
```

---

## 4. Opérateurs de Comparaison

Ces opérateurs évaluent deux opérandes et renvoient un résultat booléen (`true` ou `false`) :

| Opérateur | Signification | Exemple | Résultat |
| :---: | :--- | :--- | :---: |
| `==` | Égalité structurelle (compare le contenu) | `5 == 5` | `true` |
| `!=` | Différent de / Inégalité | `5 != 3` | `true` |
| `<` | Strictement inférieur | `10 < 20` | `true` |
| `<=` | Inférieur ou égal | `10 <= 10` | `true` |
| `>` | Strictement supérieur | `15 > 30` | `false` |
| `>=` | Supérieur ou égal | `20 >= 18` | `true` |
| `===` | Égalité référentielle (même instance en mémoire) | `objA === objB` | Dépend du pointeur |

---

## 5. Opérateurs Logiques

Permettent d'associer ou d'inverser des conditions booléennes :

* `&&` (**ET** logique) : Renvoie `true` si et seulement si toutes les conditions sont vraies.
* `||` (**OU** logique) : Renvoie `true` si au moins une des conditions est vraie.
* `!` (**NON** logique / Négation) : Inverse l'état d'un booléen (`!true` donne `false`, `!false` donne `true`).

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

En Kotlin, `if` peut fonctionner de deux manières :
1. Comme une **instruction** (exécuter une action selon une condition).
2. Comme une **expression** (calculer et retourner une valeur directement).

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
L'opérateur ternaire classique (`condition ? a : b`) n'existe pas en Kotlin car `if` est lui-même une expression capable de produire une valeur :

```kotlin
val a = 12
val b = 25

// Affectation directe en une seule ligne
val max: Int = if (a > b) a else b

println("Le plus grand nombre est : $max") // 25
```

### C. Blocs multilignes dans une expression
Si les branches contiennent plusieurs instructions entre accolades `{ }`, **la dernière ligne exécutée dans le bloc représente la valeur produite** :

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

> **Règle obligatoire pour le `if` expression :** Lorsqu'il sert à affecter une variable ou retourner une valeur, la branche `else` est **obligatoire**. Le compilateur doit garantir qu'une valeur sera toujours fournie.

---

## 7. La Structure `when` (L'alternative moderne au `switch`)

Le mot-clé `when` remplace le `switch` traditionnel. Il supprime les risques liés aux `break` oubliés (l'exécution s'arrête dès que le cas correspond) et s'utilise aussi bien en instruction qu'en expression.

### A. Utilisation classique (Instruction)
```kotlin
val statusCode = 404

when (statusCode) {
    200 -> println("Succès : OK")
    201 -> println("Succès : Ressource créée")
    400, 422 -> println("Erreur client : Requête invalide") // Plusieurs valeurs possibles
    404 -> println("Erreur : Non trouvé")
    else -> println("Code d'erreur non géré")
}
```

### B. Le `when` en tant qu'expression
```kotlin
val role = "admin"

val accessLevel: Int = when (role) {
    "superadmin" -> 1
    "admin" -> 2
    "editor" -> 3
    else -> 4 // Obligatoire pour garantir l'exhaustivité
}

println("Niveau d'accès : $accessLevel") // 2
```

### C. Conditions avancées : Intervalles (`in`) et Types (`is`)
```kotlin
// 1. Tester un intervalle numérique avec in / !in
val note = 14
val resultat = when (note) {
    in 16..20 -> "Très bien"
    in 12..15 -> "Bien"
    in 10..11 -> "Moyen"
    !in 0..20 -> "Note invalide"
    else -> "Insuffisant"
}

// 2. Tester le type avec is (Smart Cast automatique)
val donnee: Any = "Campus"
when (donnee) {
    is String -> println("Chaîne de ${donnee.length} caractères") // Cast automatique !
    is Int -> println("Entier doublé : ${donnee * 2}")
    is Boolean -> println("Booléen")
    else -> println("Type inconnu")
}
```

---

## 8. Cas Pratique : Comparaison directe `if` vs `when` (sans argument)

Lorsque `when` est utilisé sans paramètre entre parenthèses, il évalue chaque ligne comme une condition booléenne indépendante[cite: 1].

Voici un exemple concret illustrant le même traitement tarifaire écrit d'abord avec un `if` multiligne, puis avec un `when` sans argument[cite: 1] :

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
[cite: 1]

### Ce qu'il faut retenir de cet exemple :
1. **Dans le bloc `if`** : Chaque branche produit sa valeur sur la dernière ligne (ex. `"Gratuit"`, `"Petit Prix"`), qui est ensuite assignée à `priceLabel`[cite: 1].
2. **Dans le bloc `when`** : L'absence de paramètre après `when` permet d'écrire des comparaisons complètes (`price == 0.0`, `price < 10.0`) de façon plus compacte et lisible que l'enchaînement de `else if`[cite: 1].

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
    println("Utilisateur : $nom | Rôle : $role | Actif : $estActif")
}

// Utilisation avec valeurs par défaut
creerProfil("Camille") // Rôle = "Membre", estActif = true

// Utilisation avec arguments nommés (l'ordre n'a plus d'importance)
creerProfil(role = "Admin", nom = "Alex")
```

---

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
[cite: 2]

> **À noter :**
> * L'appel `priceLabel(0.0)` utilise le passage de paramètre classique (par position)[cite: 2].
> * L'appel `priceLabel(price = 0.0)` utilise un argument nommé, ce qui explicite le paramètre ciblé[cite: 2].
> * `return if (...)` renvoie directement la valeur calculée par la branche correspondante de l'expression conditionnelle[cite: 2].
> * Dans la branche `else`, l'appel `price.toString()` est équivalent à l'utilisation d'une chaîne interpolée `"$price"`.

---

### E. Les Expressions Lambdas : Stocker une fonction dans une variable

En Kotlin, les fonctions sont des "citoyens de première classe" (*first-class citizens*). Cela signifie qu'on peut stocker une fonction anonyme (une lambda) directement dans une variable, la passer en paramètre ou la retourner.

#### 1. Syntaxe générale d'une lambda
Une lambda est toujours entourée d'accolades `{}`. Les paramètres sont placés à gauche de la flèche `->`, et le corps de la fonction à droite :

```kotlin
val nomVariable = { param1: Type, param2: Type -> 
    // Corps de la fonction (la dernière ligne est la valeur retournée)
}
```

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

---

### F. La Trailing Lambda (Syntaxe de la lambda en dernier paramètre)

En Kotlin, lorsqu’une fonction accepte une lambda comme **dernier paramètre**, la convention permet de sortir la lambda des parenthèses :

```kotlin
fun appliquerReduction(prix: Double, calcul: (Double) -> Double): Double {
    return calcul(prix)
}

// 1. Syntaxe classique (lambda à l'intérieur des parenthèses)
val prix1 = appliquerReduction(100.0, { it * 0.8 })

// 2. Syntaxe avec Trailing Lambda (recommandée)
val prix2 = appliquerReduction(100.0) { it * 0.8 }
```

Si la lambda est le **seul et unique paramètre**, les parenthèses `()` sont supprimées :

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

## 10. Les Classes & la Programmation Orientée Objet

### A. Déclaration de base & Constructeur principal (*Primary Constructor*)

Les propriétés se déclarent directement entre les parenthèses de la classe en utilisant `val` (lecture seule) ou `var` (modifiable) :

```kotlin
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
}
```

---

### B. Le bloc d'initialisation : `init`

Si des vérifications ou des instructions d'initialisation sont nécessaires à la création de l'objet, on utilise le bloc `init` :

```kotlin
class Utilisateur(val username: String, val age: Int) {
    init {
        require(age >= 0) { "L'âge ne peut pas être négatif !" }
        println("Compte créé pour $username ($age ans)")
    }
}
```

---

### C. Méthodes à l'intérieur d'une classe

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

### D. Les Constructeurs Secondaires (*Secondary Constructors*)

Une classe peut définir des constructeurs secondaires via le mot-clé `constructor`. Tout constructeur secondaire **doit obligatoirement déléguer** au constructeur principal avec la syntaxe `: this(...)` :

```kotlin
class Event(val title: String, val capacity: Int) {
    // Constructeur secondaire qui fournit une capacité par défaut (1000)
    constructor(title: String) : this(title, 1000)
}

fun main() {
    // 1. Appel du constructeur principal
    val event1 = Event(title = "Android developpement", capacity = 18)
    println(event1.title)    // Android developpement
    println(event1.capacity) // 18

    // 2. Appel du constructeur secondaire
    val event2 = Event("Conférence Kotlin")
    println(event2.title)    // Conférence Kotlin
    println(event2.capacity) // 1000
}
```

---

## 11. Design Pattern : Le Singleton avec `object`

Le patron de conception **Singleton** garantit qu'une classe n'a qu'**une seule instance** active en mémoire. Kotlin intègre le Singleton nativement via le mot-clé **`object`**.

### A. Déclaration et utilisation
```kotlin
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
    // Accès direct sans instanciation
    DatabaseManager.connect()
    println(DatabaseManager.databaseName)
    DatabaseManager.disconnect()
}
```

### B. Le `companion object` (Équivalent du `static` en Java)
Pour associer des membres directement à une classe sans instanciation préalable :

```kotlin
class User(val id: Int, val name: String) {
    companion object {
        const val MIN_NAME_LENGTH = 3

        fun createGuest(): User {
            return User(id = 0, name = "Invité")
        }
    }
}

fun main() {
    val guest = User.createGuest()
    println("Longueur min : ${User.MIN_NAME_LENGTH}")
    println(guest.name) // Invité
}
```

---

## 12. Les Classes de Données : `data class`

Utilisées pour les modèles transportant de la donnée (réponses d'API, entités de base de données).

```kotlin
data class Product(
    val id: Int,
    val title: String,
    val price: Double,
    val isAvailable: Boolean = true
)
```

### A. Fonctionnalités générées automatiquement
* `toString()` : affichage lisible de l'objet (ex. `Product(id=1, title=Clavier, ...)`).
* `equals()` et `hashCode()` : comparaison par contenu avec `==`.
* `.copy()` : clonage en modifiant des propriétés à la volée.
* `componentN()` : permet la déstructuration de l'objet.

```kotlin
fun main() {
    val articleA = Product(id = 101, title = "Clavier", price = 49.99)
    val articleB = Product(id = 101, title = "Clavier", price = 49.99)

    println(articleA == articleB) // true (comparaison des valeurs)

    // Clonage avec modification ciblée
    val articleSolde = articleA.copy(price = 39.99)
    println(articleSolde)
}
```

---

### B. La Déstructuration d'une `data class` (*Destructuring Declaration*)

Permet d'extraire les valeurs d'une `data class` dans des variables distinctes :

```kotlin
val product = Product(101, "Souris", 29.99)

// Extraction séquentielle basée sur component1(), component2(), component3()
val (id, title, price) = product
println("Article $id : $title à $price €")

// Ignorer des champs avec l'underscore (_)
val (_, nomSeul) = product
```

---

## 13. Les Fonctions d'Extension (*Extension Functions*)

Permettent d'ajouter de nouvelles fonctionnalités à une classe existante sans modifier son code source ni utiliser l'héritage.

### A. Syntaxe et objet courant (`this`)
Dans le corps de la fonction, **`this`** représente l'instance sur laquelle la méthode est appelée (le récepteur) :

```kotlin
// Extension sur le type Double
fun Double.toEuro(): String {
    return String.format("%.2f €", this)
}

// Extension sur String (this est implicite)
fun String.isCapitalized(): Boolean {
    return isNotEmpty() && this[0].isUpperCase()
}

fun main() {
    val prix = 19.9
    println(prix.toEuro()) // 19,90 €

    println("Lille".isCapitalized()) // true
}
```

### B. Le Chaînage des Fonctions d'Extension (*Chaining*)
Comme chaque extension peut renvoyer un nouvel élément, on peut les enchaîner pour construire un pipeline lisible :

```kotlin
fun Double.remise(pourcentage: Double): Double = this * (1.0 - pourcentage / 100.0)
fun Double.avecLivraison(frais: Double): Double = this + frais
fun Double.enEuros(): String = String.format("%.2f €", this)

fun main() {
    val totalAffiche = 100.0
        .remise(20.0)         // 80.0
        .avecLivraison(4.99)  // 84.99
        .enEuros()            // "84,99 €"

    println(totalAffiche) // 84,99 €
}
```

---

## 14. Les Tableaux (`Array`)

Un tableau (`Array`) représente une structure séquentielle de **taille fixe** allouée en mémoire.

```kotlin
// 1. Initialisation par valeurs
val langages = arrayOf("Kotlin", "Java", "Swift")
langages[1] = "C++" // Modification par index

// 2. Tableaux primitifs optimisés (sans surcoût mémoire)
val entiers = intArrayOf(10, 20, 30)
val doubles = doubleArrayOf(1.5, 3.14)

// 3. Parcourir un tableau
for (i in entiers.indices) {
    println("Index $i : ${entiers[i]}")
}
```

---

## 15. Les Collections : Listes, Ensembles & Dictionnaires

Kotlin sépare rigoureusement les collections en **lecture seule** et **mutables**.

| Type | Ordre préservé | Doublons autorisés | Accès par clé |
| :--- | :---: | :---: | :---: |
| **`List`** (Liste ordonnée) | Oui | Oui | Non (index `[0]`) |
| **`Set`** (Ensemble d'éléments uniques) | Non garanti | **Non** | Non |
| **`Map`** (Paires clé / valeur) | Selon implémentation | Clés uniques / Valeurs en doublon possibles | **Oui** (`map[cle]`) |

```kotlin
fun main() {
    // 1. Set (supprime automatiquement les doublons)
    val tags = setOf("android", "mobile", "android")
    println(tags) // [android, mobile]

    // 2. Map (paires clé -> valeur avec le mot-clé infix 'to')
    val config = mutableMapOf("theme" to "dark", "lang" to "fr")
    config["font_size"] = "14"
    println(config["theme"]) // dark
}
```

---

## 16. Les Listes (`List` et `MutableList`)

### A. Création et modification
```kotlin
fun main() {
    // Liste en lecture seule
    val fruits: List<String> = listOf("Pomme", "Banane", "Orange")

    // Liste modifiable
    val taches: MutableList<String> = mutableListOf("Réviser Kotlin", "Faire du sport")
    taches.add("Créer un projet Android")
    taches.remove("Faire du sport")
    taches[0] = "Prendre un thé"

    println(taches) // [Prendre un thé, Créer un projet Android]
}
```

### B. Accès sécurisé et méthodes utiles
* `.size` : nombre d'éléments.
* `.firstOrNull()` / `.lastOrNull()` : premier/dernier élément sans risquer de crash si la liste est vide.
* `.getOrNull(index)` : renvoie `null` au lieu d'une exception si l'index n'existe pas.

```kotlin
val users = listOf("Alex", "Sam")
println(users.getOrNull(5)) // null
```

### C. Opérations fonctionnelles sur les listes
```kotlin
val notes = listOf(8.5, 14.0, 19.0, 11.5, 6.0)

// Chaînage filter / map
val valides = notes
    .filter { it >= 10.0 }
    .map { "Note : $it/20" }

println(valides) // [Note : 14.0/20, Note : 19.0/20, Note : 11.5/20]

// Calculs directs
val moyenne = notes.sumOf { it } / notes.size
val aReussi = notes.any { it >= 18.0 } // true
```
---

## 17. Cas Pratique : Manipuler une Collection d'Événements

En développement applicatif (par exemple pour un écran d'agenda ou de billetterie), on manipule quasi systématiquement des collections d'objets métier modélisés sous forme de `data class`.

---

### A. Les différents types de listes applicables

Lorsqu'on manipule une liste d'objets, le choix du type dépend de la mutabilité et du besoin d'accès :

| Type d'interface / classe | Caractéristiques | Cas d'usage typique |
| :--- | :--- | :--- |
| **`List<Event>`** | En lecture seule (*read-only*), immuable par défaut. | Données d'affichage UI (Jetpack Compose), retours d'API. |
| **`MutableList<Event>`** | Modifiable (`add`, `remove`, `clear`). | Panier, liste d'attente, gestion d'un formulaire dynamique. |
| **`ArrayList<Event>`** | Implémentation concrète basée sur un tableau dynamique redimensionnable. | Créée en coulisses par `mutableListOf()`, utile si l'on a besoin d'optimiser des accès indexés fréquents. |

---

### B. Modélisation de l'événement (`Event`)

Reprenons notre modèle d'événement avec titre, capacité et prix :

```kotlin
data class Event(
    val id: Int,
    val title: String,
    val capacity: Int,
    val price: Double = 0.0
)
```

---

### C. Opérations et requêtes courantes sur la collection

Voici un ensemble d'opérations concrètes pour filtrer, trier, agréger et transformer une liste d'événements :

```kotlin
fun main() {
    val events: List<Event> = listOf(
        Event(1, "Conférence Kotlin", 150, 0.0),
        Event(2, "Atelier Android", 30, 15.0),
        Event(3, "Hackathon Mobile", 80, 5.0),
        Event(4, "Meetup UI/UX", 50, 0.0),
        Event(5, "Masterclass Compose", 25, 45.0)
    )

    // 1. Filtrer les événements gratuits
    val gratuits = events.filter { it.price == 0.0 }
    println("Événements gratuits : ${gratuits.map { it.title }}")

    // 2. Trouver le premier atelier disponible (< 50 places)
    val petitComite = events.find { it.capacity <= 30 }
    println("Atelier intimiste : ${petitComite?.title}")

    // 3. Trier par capacité décroissante
    val parCapacite = events.sortedByDescending { it.capacity }

    // 4. Calculer la jauge totale de tous les événements réunis
    val capaciteTotale = events.sumOf { it.capacity }
    println("Capacité globale : $capaciteTotale places")

    // 5. Regrouper par type (payant vs gratuit) via groupBy
    val repartition = events.groupBy { if (it.price == 0.0) "Gratuit" else "Payant" }
    println("Total gratuits : ${repartition["Gratuit"]?.size}")
    println("Total payants : ${repartition["Payant"]?.size}")

    // 6. Transformer (map) vers une liste de titres formatés
    val fiches = events.map { (id, title, cap, price) ->
        "[$id] $title — $cap places (${if (price == 0.0) "Offert" else "$price €"})"
    }
    fiches.forEach { println(it) }
}
```

---

### D. Gestion d'une liste modifiable (`MutableList`)

Pour ajouter, modifier ou retirer un événement à la volée :

```kotlin
val planning: MutableList<Event> = events.toMutableList()

// Ajout
planning.add(Event(6, "Keynote Annuelle", 300, 20.0))

// Suppression selon un critère (ex. annulation des sessions de moins de 30 places)
planning.removeAll { it.capacity < 30 }

// Mise à jour ciblée grâce à copy()
val index = planning.indexOfFirst { it.id == 1 }
if (index != -1) {
    planning[index] = planning[index].copy(capacity = 200)
}
```
### E. Parcourir une liste avec la boucle `for`

La boucle `for` s'adapte à plusieurs cas d'usage selon qu'on a besoin uniquement de l'élément, de sa position, ou des deux en même temps.

---

#### 1. Parcourir les éléments directement (`for in`)
C'est la syntaxe la plus simple et la plus lisible quand l'index n'est pas nécessaire :

```kotlin
for (event in events) {
    println("${event.title} (${event.capacity} places)")
}
```

---

#### 2. Parcourir avec l'index et l'élément (`withIndex()`)
Si l'on a besoin d'afficher le rang ou le numéro de ligne en plus de l'objet, on utilise `.withIndex()`, qui déstructure automatiquement la paire `(index, element)` :

```kotlin
for ((index, event) in events.withIndex()) {
    println("#${index + 1} - ${event.title}")
}
```

---

#### 3. Parcourir via les index (`indices`)
Pour manipuler les éléments en accédant directement à leur position mémoire ou pour comparer un élément au suivant :

```kotlin
for (i in events.indices) {
    println("Événement à l'index $i : ${events[i].title}")
}
```

---

#### 4. Parcourir avec un intervalle personnalisé (`until`, `downTo`, `step`)
Pour ne parcourir qu'une partie de la liste ou sauter des éléments :

```kotlin
// Parcourir uniquement les 3 premiers (de l'index 0 à 2)
for (i in 0 until minOf(3, events.size)) {
    println("Top 3 : ${events[i].title}")
}

// Parcourir un événement sur deux
for (i in events.indices step 2) {
    println("Événement pair : ${events[i].title}")
}

// Parcourir à l'envers (du dernier au premier)
for (i in events.size - 1 downTo 0) {
    println("Rétro : ${events[i].title}")
}
```

---

#### 5. Déstructuration directe dans la boucle
Comme `Event` est une `data class`, on peut déstructurer ses attributs directement dans la signature du `for` :

```kotlin
for ((id, title, capacity) in events) {
    println("[$id] $title : jauge de$capacity")
}
```
---

## 18. Bonnes Pratiques : Lire et Analyser du Code Kotlin Efficacement

Comprendre rapidement le code d'un collègue, d'une documentation ou d'une bibliothèque Android repose sur le repérage de quelques marqueurs visuels clés du langage.

---

### A. La grille de lecture rapide

Face à une fonction ou un bloc d'instructions, pose-toi ces questions dans l'ordre :

1. **Instruction ou Expression ?**
   * Y a-t-il un `=` après la signature de la fonction (`fun calculer() = ...`) ou après une variable (`val x = if (...)`) ? Si oui, le bloc produit directement une donnée.
2. **Où est la valeur de retour ?**
   * Dans un bloc `{}` utilisé comme expression (avec `if`, `when` ou une lambda), **la dernière ligne exécutée est la valeur produite**. Pas besoin de chercher un mot-clé `return`.
3. **D'où vient la variable ?**
   * Un identifiant surgit sans être déclaré ? C'est soit le paramètre implicite **`it`** d'une lambda à argument unique, soit une propriété accessible via le **`this`** implicite d'une classe ou d'une fonction d'extension.
4. **Quels types entrent et sortent ?**
   * Grâce à l'inférence de type, les types ne sont pas toujours écrits. Lis les signatures : `(A, B) -> C` indique une fonction qui prend `A` et `B` pour fournir `C`.

---

### B. Décryptage d'exemples pas à pas

#### 1. Déconstruire une lambda et sa trailing syntax
```kotlin
val resultat = events
    .filter { it.price > 0.0 }
    .map { "${it.title} : ${it.price} €" }
```
* **Lecture :** 
  1. `events` est la source (une collection).
  2. `.filter { ... }` est une trailing lambda : les parenthèses `()` ont sauté car la lambda est le seul paramètre.
  3. `it` représente chaque élément un par un pendant le parcours.
  4. `.map { ... }` transforme chaque élément restant en une chaîne formatée via `$it.title`.

---

#### 2. Déconstruire une fonction d'extension chaînée
```kotlin
fun Double.remise(taux: Double) = this * (1.0 - taux / 100.0)
```
* **Lecture :**
  1. `Double.` $\rightarrow$ fonction greffée sur tous les nombres décimaux.
  2. `this` $\rightarrow$ la valeur numérique appelante (ex. dans `50.0.remise(10.0)`, `this` vaut `50.0`).
  3. Le `=` sans accolades indique une fonction à expression unique (*single-expression*), le type de retour `Double` est déduit automatiquement.

---

#### 3. Déconstruire un `when` sans argument
```kotlin
val statut = when {
    score >= 90 -> "A"
    score >= 70 -> "B"
    else -> "C"
}
```
* **Lecture :**
  1. Aucun paramètre entre parenthèses après `when` : chaque ligne est évaluée comme une condition booléenne (`true`/`false`) indépendante, de haut en bas.
  2. Dès qu'une branche est vraie, la valeur à droite de `->` est renvoyée et stockée dans `statut`.

---

### C. Réflexes pour le débogage et la relecture

| Piège fréquent | Symptôme | Cause / Solution |
| :--- | :--- | :--- |
| **Parenthèses invisibles** | `action { ... }` | C'est un appel de fonction standard utilisant la trailing lambda, pas un bloc de classe. |
| **Propriété introuvable** | Erreur sur `it.name` | Vérifier si l'étape précédente n'a pas transformé la liste (ex. après un `.map { it.id }`, `it` devient un `Int`, plus l'objet d'origine). |
| **NullPointer inattendu** | Appel direct sur un résultat | Utiliser `?.` (safe call) si la méthode précédente renvoie un type nullable (comme `.find { }` ou `.firstOrNull()`). |
| **Modification inopérante** | `.filter { ... }` ne modifie rien | Les méthodes de collection en lecture seule renvoient une **nouvelle** liste sans muter la liste de départ ; il faut réassigner le résultat dans une variable. |

### G. Transformer une collection avec la méthode `map`

La méthode **`map`** est l'opération de transformation par excellence en programmation fonctionnelle et en Kotlin. Elle applique une fonction ou une lambda à **chaque élément** d'une collection d'origine et renvoie une **nouvelle liste** contenant les résultats obtenus.

Contrairement à `filter` qui peut réduire la taille de la liste, `map` conserve **toujours exactement le même nombre d'éléments** que la collection de départ, mais peut en modifier la valeur et le type.

---

#### 1. Principe de base : Transformation simple
La variable implicite `it` représente chaque élément au fur et à mesure du parcours. La valeur produite par la lambda constitue le nouvel élément :

```kotlin
val nombres = listOf(1, 2, 3, 4, 5)

// Multiplier chaque élément par 2
val doubles = nombres.map { it * 2 }
println(doubles) // [2, 4, 6, 8, 10]
```

---

#### 2. Changement de type (Projection de données)
L'un des usages les plus fréquents de `map` consiste à transformer une liste d'un type $A$ vers une liste d'un type $B$ (par exemple, extraire un champ d'un objet ou préparer des données pour l'interface graphique) :

```kotlin
// Modèle de données
data class Event(val id: Int, val title: String, val price: Double)

val events = listOf(
    Event(1, "Conférence Kotlin", 0.0),
    Event(2, "Atelier Compose", 25.0)
)

// List<Event> -> List<String> (extraction d'une propriété)
val titres: List<String> = events.map { it.title }
println(titres) // [Conférence Kotlin, Atelier Compose]

// List<Event> -> List<String> (formatage personnalisé)
val etiquettes: List<String> = events.map { 
    "${it.title} (${if (it.price == 0.0) "Gratuit" else "${it.price} €"})" 
}
```

---

#### 3. Chaînage typique : `filter` puis `map`
Dans un pipeline de données, on filtre généralement les données d'abord, puis on transforme les éléments conservés :

```kotlin
val evenementsPayants = events
    .filter { it.price > 0.0 }
    .map { "${it.title} : ${it.price} €" }
```

> **Attention à l'ordre dans la chaîne :**
> Si tu écris `events.map { ... }.filter { ... }`, tu effectues la transformation sur **tous** les éléments avant d'en jeter une partie. Mettre `filter` en premier permet de ne transformer que les éléments réellement utiles, ce qui optimise les calculs et la mémoire.

---

#### 4. Les variantes indispensables de `map`

| Variante | Rôle & Utilité | Exemple |
| :--- | :--- | :--- |
| **`mapNotNull`** | Transforme les éléments et **ignore automatiquement les résultats `null`** (combine un `map` et un `filterNotNull`). | `val valides = list.mapNotNull { it.toIntOrNull() }` |
| **`mapIndexed`** | Fournit à la fois la position et l'élément `(index, item)` lors de la transformation. | `events.mapIndexed { index, e -> "#${index + 1} -${e.title}" }` |
| **`flatMap`** | Transforme chaque élément en une sous-liste, puis **aplatit** le tout en une seule liste unique à une dimension. | `listes.flatMap { it.elements }` |
| **`mapKeys` / `mapValues`** | Dédiées aux dictionnaires (`Map`) pour transformer uniquement les clés ou uniquement les valeurs. | `panier.mapValues { it.value * 1.20 }` |
