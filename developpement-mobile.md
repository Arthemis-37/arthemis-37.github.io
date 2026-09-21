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
