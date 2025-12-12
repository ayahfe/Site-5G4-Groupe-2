+++
title = "Notes de cours"
weight = 1
+++

## 1. Introduction

### 1.1 Contexte général

Unity technologies a été créé en 2004 par David Helgason, Joachim Ante et Nicholas Francis et présenté en juin 2005 à Apple Inc. Lors d’une conférence de développeur. Cette plateforme de développement a été créé pour contrer trois principaux problèmes qui régnais dans l’industrie des jeux vidéo à cette époque.
• Le développement complexe et élitiste
• Les coûts démesurer
• La fragmentation multiplateforme

### 1.2 Son impact aujourd’hui

Lors de sa sortie, Unity révolutionne l’industrie des jeux vidéo. Elle donne place à la créativité de milliers de créateur et leurs donnes une plateforme sur lequel ils peuvent faire naitre leurs créations avec facilité.
En 2016 Gartner, une compagnie de recherche, estimais que 50% des applications mobiles étais développer en utilisant Unity. Aujourd’hui son succès est sans équivoque et son impact vas au-delà de l’industrie des jeux vidéo. En effet, elle s’est étendue dans les domaines suivant :
• Cinéma, animation et effet spéciaux
• Secteur de la Santé
• Architecture
• Éducation  
• Automobile et aviation
• Intelligence artificielle

---

## 2. Concepts Fondamentaux

### 2.1 Définition de base

En premier lieux définition ce qu’est un moteur de jeux? Un moteur de jeux est un logiciel complet (Framework) qui fournit une base technique et des outils pour simplifier et accélérer le développement de jeux vidéo.
Unity technologies lui propose une Framework de développement temps réel, basé sur du C# et .NET. Il offre également des pipelines\* de rendu personnalisables et une panoplie d’outils (Test Framework, DevOps, Asset Store) pour ainsi crée des applications interactives de manière structurer et optimisé.

\*ensemble des étapes qui transforme un modèle en image afficher à l’écran.

---

### 2.2 Interface de Unity

#### 2.2.1 Fenêtre Scène (Scene View)

![Scene View](https://docs.unity3d.com/2022.2/Documentation/uploads/Main/scene-view.png)

La Fenêtre de vue sur Unity editor sert à visualiser, manipuler et organiser l’interface que l’on crée. C’est l’endroit où on construit et édite l’environnement de jeux avant qu’il ne soit tester dans le game views.

**Tableau des éléments manipulables dans Scene View**

| Fonctions                              | Descriptions                                                                                   |
| -------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Overlays                               | Permet d’afficher ou de masquer des panneaux et outils personnalisables.                       |
| Grid Snapping                          | Aligne les objets sur la grille pour un placement précis                                       |
| Sélection des GameObjects              | Sélect les objets de la scène (modèles 3D, lumières, caméras, etc..)                           |
| Positionnements des objets (Transform) | Modification de la positions, rotations et échelle directement dans la vue, avec les handles\* |
| Navigation dans Scene View             | Déplacement libre dans la scène (zoom, pan, rotation de la caméra)                             |
| Scene Camera                           | Configuration de la caméra de la Scene View (angle, FOV\*, mode orthographique/perspective)    |
| Contrôle en vue FPS                    | Déplacement dans la scène en première personnes.                                               |
| Scene Visibilité                       | Masquer ou afficher certains objets pour mieux travailler.                                     |
| Gizmos                                 | Activer ou désactiver les repères visuels                                                      |
| Menu contextuel                        | Accès rapide aux actions courantes : créer un objet, dupliquer, supprimer, aligner, etc.       |
| Custom Editor Tools                    | Outils personnalisés développés en C# pour faciliter la création ou l’édition.                 |

- « handles » désignes les petits outils visuels qui apparaisse lorsqu’on sélectionne un objet dans la scène.  
  \*FOV : angle de vision de la caméra

---

#### 2.2.2 Game View

La Game View est la fenêtre qui permet de voir le jeu tel qu’il apparaîtra pour le joueur. Elle utilise la caméra principale pour afficher la scène avec tous les éléments visuels, animations et interactions en temps réel. Contrairement à la Scene View, elle sert à tester et prévisualiser le jeu plutôt qu’à éditer les objets. C’est un outil essentiel pour vérifier le rendu, l’UI, la physique et le gameplay avant de publier le jeu.
La différence principale entre la Scene View et la Game View réside dans leur rôle : la Scene View permet de créer, modifier et organiser les objets dans la scène, tandis que la Game View sert à tester et visualiser le résultat final, tel que le joueur le verra.

---

#### 2.2.3 Hierarchy

La Hierarchy est la fenêtre qui liste tous les GameObjects présents dans la scène. Elle montre la structure hiérarchique des objets et permettant de sélectionner et organiser facilement les éléments de la scène.

Rôle principale de la Hierarchy :
• Afficher tous les GameObjects d’une scène
• Montrer les relations parent/enfant entre les objets
• Permettre la sélection rapide pour modification
• Facilite l’organisation des scène complète

---

#### 2.2.4 Inspector

La fenêtre Inspector dans Unity permet de visualiser et de modifier toutes les propriétés des objets actuellement sélectionnés dans la scène ou dans le projet comme la position, la rotation et l'échelle par exemple. Comme chaque objet Unity est constitué de différents composants, l’inspector affiche les paramètres pour chacun de ces composants et permet de les ajuster directement.

---

#### 2.2.5 Project window

![Scene View](https://docs.unity3d.com/2021.3/Documentation/uploads/Main/project-window-context.png)
Le Project window affiche les dossiers utiliser du projet. C’est la manière principale de naviguer entre les fichiers de l’application. Lorsqu’un nouveau projet est créé cette page est afficher par défaut.

---

## 2.3 GameObjects

GameObject est un conteneur pour tout ce que l’on voit dans Unity et avec lequel on interagit dans le jeu Unity. Par exemple une caméra, un objet 3D, un arbre, une arme ou une source de lumière. Chaque GameObject peut avoir différents composants attachés à lui, qui définissent son comportement et son apparence. On peut par exemple ajouter un composant Sprite Renderer qui sert d’afficher une image, un composant Collider pour détecter les collisions avec d’autres objets, Rigidbody pour donner une physique ou AudioSource pour jouer un son.

Prenons l’exemple d’un GameObject nommé “Cercle”. Celui-ci pourrait contenir deux composants :  
Transform : Qui prend en charge la position, la rotation et l’échelle du GameObject.  
Sprite Renderer : Ce composant affiche l’image du cercle à l’écran.

Unity utilise une organisation d’arbre hirérchique. Chaque objet dans Unity peut être lié à un autre objet pour former une relation Parent / Enfant. Une structure hiérachique. Le parent est l’objet principal, tandis que l’enfant dépend du parent et hérite de certains de ces mouvements (ex. Se déplacer avec lui). Cette relation est représentée dans la fenêtre Hiérarchie de Unity où les objets enfants sont listés sous leurs parents avec une indentation.

---

## 2.4 Components (Unity)

Les composant sur Unity sont les éléments qui ajoute une fonctionnalité a un GameObjects. Donc chaque component on des propriétés pouvant être modifier pour définir le comportement d’un GameObject

---

# 3. La scene et ses éléments (Scene System)

## 3.1 Le Transform

Le composant transform sert à positionner, orienter et dimensionner les objets dans Unity. Il fonctionne comme un « GPS » du GameObject : il indique où l’objet se trouve, comment il est tourné et quelle est sa taille.
Dans un script, lorsqu’on écrit transform, on fait directement référence au composant Transform du GameObject auquel le script est attaché.
• Transform : contient la position, la rotation et l’échelle d’un GameObject  
• GameObject : objet de la scène contrôlé par le script

Grâce à transform, on peut accéder et manipuler les propriétés pour contrôler le comportement du GameObject.

Exemple :  
transform.position -> donne la position du GameObject dans le monde du jeu.

Exemple de déplacement :  
Transform.Translate(Vector2.up, Space.Self);

Explication :  
Transform : composant qui stocke position, rotation et échelle  
Translate : méthode qui déplace le GameObject  
Vector2.up : direction « vers le haut »  
Space.Self : déplacement selon les axes locaux du GameObject.

---

## 3.2 La Caméra dans Unity

Les caméras sont des composants dans Unity qui permettent de capturer et afficher la vue de notre jeu. Elles déterminent ce que le joueur voit à l’écran et jouent un rôle crucial dans la création d’une expérience immersive et interactive. . Chaque caméra Unity possède plusieurs propriétés. En voici quelques-unes :

Clear Flags : Détermine ce que la caméra efface avant de rendre une nouvelle image. Ces options incluent :  
• Skybox : Affiche le ciel ou une image de fond  
• Solid Color : Efface l’écran avec une couleur unie.  
• Depth Only : Effacement uniquement les informations de profondeur (utile pour les superpositions)  
• Don’t Clear : Ne rien effacer (effets spéciaux

Background : La couleur utilisée pour effacer l’écran si Solid Color est sélectionné

Projection : Définit comment la caméra projette la scène sur l’écran. Deux modes :  
• Perspective : Vision 3D réaliste (effet de profondeur)  
• Orthographic : Vision 2D (pas de perspective)

Field of View (FOV) : Angle de vue en mode Perspective  
Clipping Planes : Distance minimale et maximale à laquelle la caméra rend les objets  
Viewport Rect : Définit quelle partie de l’écran la caméra occupe :  
• X : La position horizontale  
• Y : La position verticale  
• Width : La largeur de la vue  
• Height : La hauteur de la vue

---

## 3.3 Les Lumières

Les lumières sont des composants essentiels dans Unity qui permettent de créer des environnements visuellement immersifs et réalistes. Elles influencent l’éclairage, les ombres et l’ambiance visuelle de la scène. Il existe 4 types principaux de lumières :

### 1. Directional Light :

• Simule le soleil (lumière venant de très loin)  
• Même intensité partout indépendamment de la distance  
• Paramètres: direction, intensity, color, shadow type, indirect multiplier.  
• Idéale pour simuler un cycle jour/nuit en modifiant sa rotation.

### 2. Le point Light :

• Lumière qui part d’un point dans toutes les directions (ex-ampoule)  
• Utilisée pour éclairer les zones localisées  
• Paramètres: range, intensity, color, shadow, type, fallof.

### 3. Le Spot Light :

• Lumière en forme de cône (ex-projecteur, phare de voiture)  
• Paramètres: range, spot, angle, intensity, color, shadow type, fallof

### 4. Area light

• Lumière émise depuis une surface rectangulaire.  
• Produit un éclairage doux et réaliste.  
• Paramètres: size, intensity, color, indirect, contribution, shape.  
• Nécessite d’ouvrir Lighting Window et de cliquer sur Generate Lighting pour qu’elle soit visible.

---

# 4. Scripting en C#

## 4.1 Pourquoi Unity utilise C#

Unity utilise C#, parce qu’il est facile à apprendre et accessible pour les débutants. C’est un langage sécurisé, performant grâce à .NET et orienté objet, ce qui correspond parfaitement à l’architecture d’Unity basée sur les GameObjects et les composants.

---

## 4.2 Structure d’un script Unity

• Les namespaces : Situé en haut du script. Permettent d’utiliser certaines fonctionnalités. Ex : using UnityEngine;  
• Classe : Correspond au script lui-même. Son nom (ex : MonScriptDestinee) doit être identique au nom du fichier. Peut hériter de MonoBehaviour qui permettront d’utiliser Start(), Update(), etc.  
• Portée {} : Les accolades définissent les blocs de code d’une classe ou d’une méthode. Tout le contenu entre {} appartient à cette portée.  
• Commentaires : // pour une un commentaires sur une seule ligne et /\* \*/ pour un commentaire sur plusieurs lignes.  
• Méthode Start() : Méthode appelée au lancement du jeu ou lorsque l’Object contenant le script apparaît dans la scène.  
• Méthode Update() : Méthode exécutée automatiquement à chaque frame. Idéale pour les actions répétitives (mouvements, contrôles joueur, vérifications en continu.)

---

## 4.3 Manipulation des GameObjects par script

Pour interagir avec les composants, Unity fournit plusieurs méthodes essentielles. Elles permettent de rechercher, d’accéder et de modifier les propriétés de composants spécifiques, comme le Rigidbody, le BoxCollider ou les composants personnalisés.

• GetComponent<T>() : Récupère le composant du type spécifié attaché au même GameObject. Ex : Rigidbody rb = GetComponent<Rigidbody>();  
• GetComponentInChildren<T>() : Récupère un composant du type spécifié présent dans un des enfants du GameObject.  
• GameObject.Find() : Recherche un GameObject dans toute la scène à partir de son nom. Ex : GameObject joueur = GameObject.Find("Player");  
• GetComponentInParent<T>() : Récupère un composant du parent.  
• GameObject.AddComponent<T>() : Ajoute dynamiquement un composant. Ex : gameObject.AddComponent<Rigidbody>();

---

# 5. Conclusion

Résumé des concepts appris  
Dans ces notes de cours, nous avons découvert les bases essentielles pour comprendre et utiliser Unity. Nous avons commencé par présenter le contexte dans lequel Unity a été créé et parlé de son impact majeur sur l’industrie des jeux vidéo et sur d’autres domaines comme le cinéma, l’architecture et la santé. Nous avons ensuite vu ce qu’est un moteur de jeu et comment Unity fournit un environnement complet basé sur le C# et .NET pour développer des applications interactives.

Nous avons exploré l’interface de Unity et ses différentes fenêtres : la Scene View pour construire l’environnement, la Game View pour visualiser le jeu tel qu’il sera vu par le joueur, la Hierarchy pour organiser les objets, l’Inspector pour modifier leurs propriétés, et le Project Window pour gérer les fichiers du projet.

Les concepts fondamentaux de GameObjects et de Components été abordés. En effet, un GameObject représente tout élément présent dans la scène et les composants déterminent son comportement. Nous avons expliqueé le fonctionnement des composants essentiels comme Transform, les Caméras et les Lumières, qui jouent un rôle clé dans la construction d’un jeu.

Enfin, nous avons introduit le scripting en C#, en expliquant pourquoi Unity utilise ce langage, la structure d’un script, et les méthodes permettant de manipuler les GameObjects par code, comme GetComponent(), GetComponentInChildren() ou AddComponent().

En résumé, ces notions constituent les fondations nécessaires pour commencer à développer efficacement dans Unity et comprendre comment ses éléments interagissent.

---

# 6. Références et sources

Source :

https://www.vinode.io/blog-posts/what-is-unity-3d-and-why-you-should-use-it  
https://medium.com/@wota_mmorpg/unity-development-history-and-the-influence-of-this-game-engine-on-the-game-development-36dc7a7a3b9d  
https://learn.unity.com/  
https://docs.unity3d.com/Manual/index.html  
Comprendre l'Inspector Unity  
Apprendre GameObject dans Unity | Écrivez Votre Premier Script  
Comprendre la Hiérarchie Parent-Enfant dans Unity  
Apprendre Composant de Transformation | Écrivez Votre Premier Script  
Comprendre les caméras dans Unity  
Types de lumières dans Unity pour un rendu réaliste  
Types de lumières dans Unity pour un rendu réaliste  
Comprendre les Scripts C# Unity  
Techniques avancées pour gérer les composants Unity
