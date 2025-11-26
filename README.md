# Boutique Diayma - TD C# et Technologies .NET

Université Cheikh Anta Diop - ESP  
Année Universitaire 2025/2026  
Étudiant : Boubacar Diouf  
Dépôt GitHub : [BDioufDiayma2026](https://github.com/BoubacarDiouf/BDioufDiayma2026)

---

## Question 1 : Récupération du code

Le code a été récupéré depuis le dépôt GitHub du professeur et cloné localement.

---

## Question 2 : Projets de la solution

La solution `Diayma.sln` contient 1 seul projet :

- Nom du projet : Diayma
- Fichier projet : `P2FixAnAppDotNetCode\Diayma.csproj`

---

## Question 3 : Version SDK .NET utilisée

Version : .NET Core 2.0 (`netcoreapp2.0`)

Cette information se trouve dans le fichier `Diayma.csproj` :
```xml
<TargetFramework>netcoreapp2.0</TargetFramework>
```

---

## Question 4 : Installation du SDK

SDKs installés sur la machine :
- .NET Core 2.0.0 (pour exécuter le projet)
- .NET 10.0.100 (version moderne)

Commande de vérification :
```powershell
dotnet --list-sdks
```

Note : .NET Core 2.0 n'est plus supporté depuis octobre 2018, ce qui explique les avertissements de sécurité lors de la compilation.

---

## Question 5 : Création du dépôt GitHub

Dépôt GitHub créé et configuré :

URL : https://github.com/BoubacarDiouf/BDioufDiayma2026

---

## Question 6 : Bugs identifiés

### Bug 1 : Traduction espagnole non fonctionnelle

Description : La langue espagnole ne s'applique pas correctement à l'interface utilisateur.

Reproduction :
1. Lancer l'application
2. Sélectionner "Español" dans le menu de langue
3. Observer que l'interface ne se traduit pas

Comportement attendu : L'interface devrait s'afficher en espagnol

Comportement observé : La traduction ne s'applique pas ou affiche des erreurs

### Bug 2 : Calcul incorrect du sous-total dans le panier

Description : Lorsqu'un utilisateur ajoute un produit plusieurs fois au panier (ou augmente la quantité d'un produit déjà présent), le sous-total de ce produit ne se met pas à jour correctement. Le prix reste figé à la valeur initiale au lieu de se recalculer en fonction de la nouvelle quantité.

Reproduction :
1. Ajouter un produit au panier (ex: Cuisinière à Gaz à 11 500,00 €)
2. Le sous-total s'affiche correctement : 11 500,00 €
3. Ajouter le même produit une deuxième fois (quantité passe à 2)
4. Observer que le sous-total reste à 11 500,00 € au lieu de 23 000,00 €
5. Le Total général et la Moyenne sont également incorrects

Comportement attendu :
- Quantité : 1 → Sous-total : 11 500,00 €
- Quantité : 2 → Sous-total : 23 000,00 € (11 500 × 2)
- Total : Somme correcte de tous les sous-totaux
- Moyenne : Total ÷ nombre de produits distincts

Comportement observé :
- Le sous-total ne se recalcule pas dynamiquement
- Les valeurs de Total et Moyenne sont fausses

---

## Question 7 : Points d'arrêt placés

Les points d'arrêt suivants ont été placés dans le code :
- a) CartSummaryViewComponent ligne 12
- b) ProductController ligne 15
- c) OrderController ligne 17
- d) CartController ligne 15
- e) Startup ligne 20

---

## Question 8 : Flux d'exécution avant l'affichage des produits

### Namespaces visités

1. P2FixAnAppDotNetCode - Namespace principal de l'application
2. P2FixAnAppDotNetCode.Controllers - Contrôleurs MVC
3. P2FixAnAppDotNetCode.Components - Composants de vue

### Classes instanciées

1. Program - Point d'entrée de l'application
2. Startup - Configuration de l'application et du pipeline HTTP
3. ProductController - Contrôleur pour gérer l'affichage des produits
4. CartSummaryViewComponent - Composant de vue pour afficher le résumé du panier

### Flux d'exécution détaillé

#### Démarrage de l'application

**Program.Main** (`Program.cs:10`)
- Namespace : `P2FixAnAppDotNetCode`
- Classe : `Program`
- Méthode : `Main(string[] args)`
- Rôle : Point d'entrée de l'application
- Mode de débogage : Pas à pas principal (F10)

↓

**Program.BuildWebHost** (`Program.cs:14`)
- Namespace : `P2FixAnAppDotNetCode`
- Classe : `Program`
- Méthode : `BuildWebHost(string[] args)`
- Rôle : Construction et configuration de l'hôte web
- Mode de débogage : Pas à pas principal (F10)

↓

**Startup.Configure** (`Startup.cs:20`)
- Namespace : `P2FixAnAppDotNetCode`
- Classe : `Startup`
- Méthode : `Configure(IApplicationBuilder app)`
- Rôle : Configuration du pipeline HTTP (middleware, routing, gestion des erreurs, fichiers statiques, MVC)
- Mode de débogage : Pas à pas détaillé (F11) pour observer la configuration des middlewares

#### Traitement de la requête HTTP pour la page d'accueil

**ProductController.Index** (`ProductController.cs:15`)
- Namespace : `P2FixAnAppDotNetCode.Controllers`
- Classe : `ProductController`
- Méthode : `Index()`
- Rôle : Récupération de la liste des produits depuis le service/repository et transmission à la vue
- Mode de débogage : Pas à pas détaillé (F11) pour observer la récupération des données

↓

**CartSummaryViewComponent.Invoke** (`CartSummaryViewComponent.cs:12`)
- Namespace : `P2FixAnAppDotNetCode.Components`
- Classe : `CartSummaryViewComponent`
- Méthode : `Invoke()`
- Rôle : Affichage du résumé du panier dans la barre de navigation (nombre d'articles, montant total)
- Mode de débogage : Pas à pas principal (F10) pour observer le rendu du composant

### Ordre chronologique complet

```
1. Program.Main (ligne 10)
   └─> Point d'entrée de l'application
   
2. Program.BuildWebHost (ligne 14)
   └─> Construction de l'hôte web ASP.NET Core
   
3. Startup.Configure (ligne 20)
   └─> Configuration du pipeline HTTP
       ├─ app.UseDeveloperExceptionPage()
       ├─ app.UseStaticFiles()
       ├─ app.UseRequestLocalization()
       └─ app.UseMvc()
   
4. [Requête HTTP GET /]
   
5. ProductController.Index (ligne 15)
   └─> Récupération des produits
       └─ return View(products)
   
6. [Rendu de la vue Index.cshtml]
   
7. CartSummaryViewComponent.Invoke (ligne 12)
   └─> Affichage du résumé du panier
       └─ return View(cart)
```

---

## Question 9 : Déploiement de l'application

L'application a été déployée en tant qu'exécutable Windows autonome.

Commande utilisée :
```powershell
dotnet publish -c Release -r win-x64 --self-contained true -o ./publish
```

Paramètres de déploiement :
- Configuration : Release (optimisé pour la production)
- Runtime : Windows x64
- Mode : Self-contained (inclut le runtime .NET Core 2.0)
- Dossier de sortie : `./publish`

Note : L'option `PublishSingleFile` n'est pas disponible pour .NET Core 2.0 (nécessite .NET Core 3.0+). L'application est donc publiée sous forme de dossier contenant l'exécutable `Diayma.exe` et toutes ses dépendances.

---

## Question 10 : Lien vers l'exécutable

L'application déployée est disponible via Google Drive :

**Lien de téléchargement :** [Diayma - Application déployée](https://drive.google.com/file/d/1DQx8KqKR16LRc6Kqv5fUgQFW3tJz9btt/view?usp=drive_link)

Le fichier ZIP contient :
- `Diayma.exe` - Exécutable principal
- Toutes les DLL nécessaires
- Fichiers de configuration
- Ressources statiques (CSS, JS, images)

Instructions d'exécution :
1. Télécharger et extraire le fichier ZIP
2. Exécuter `Diayma.exe`
3. Ouvrir un navigateur sur `http://localhost:62929`

---

## Conclusion

Ce TD a permis de :
- ✅ Configurer un environnement de développement .NET
- ✅ Analyser une application ASP.NET Core existante
- ✅ Identifier des bugs critiques (calcul du panier) et moyens (traduction)
- ✅ Utiliser efficacement le débogueur avec différents modes de navigation
- ✅ Comprendre le flux d'exécution d'une application web MVC
- ✅ Gérer le code source avec Git et GitHub
- ✅ Déployer une application .NET en exécutable Windows
