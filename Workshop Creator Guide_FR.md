# Guide de Création de Bâtiments pour l'Atelier

Bienvenue, Créateur ! Merci de votre intérêt pour la création de contenu personnalisé ! Vos contributions aideront la communauté à prospérer ! Ce guide vous aidera à créer du contenu conforme aux spécifications et à le télécharger sur le Workshop Steam.

## Exigences Techniques
Le téléchargement de contenu dépend de deux scripts : BuildingData et WorkshopUploadTool. BuildingData est utilisé pour remplir les données du bâtiment, et WorkshopUploadTool est utilisé pour le téléchargement. Vous avez également besoin de Steamworks.NET. Si vous devez mettre à jour Steamworks.NET, vous pouvez le télécharger depuis la page GitHub : https://github.com/rlabrecque/Steamworks.NET/releases, l'importer dans Unity, et attacher le script Steam Manager à un GameObject vide.
J'ai empaqueté les scripts requis et Steamworks.NET dans un fichier de package Unity. Vous pouvez utiliser ce fichier de package ou les télécharger séparément : Lien XX

### Spécifications du Modèle de Bâtiment

> ⚠️ **Rappel des Composants Requis** : Les prefabs de bâtiment doivent contenir les trois composants principaux suivants. L'absence de l'un d'entre eux entraînera des dysfonctionnements :
> - **Mesh Filter** : Affiche le maillage du modèle
> - **Mesh Renderer** : Rend le modèle (nécessite l'assignation d'un matériau)
> - **Collider** (Box Collider) : Utilisé pour la surbrillance au survol de la souris, la détection des clics et la détection de collision de placement

### Formats de Ressources Supportés

- **Modèles** : FBX
- **Textures** : PNG
- **Matériaux** : Unity URP Lit Shader

### Limites de Taille de Fichier

- AssetBundle de bâtiment unique : ≤ 50 Mo
- Image de prévisualisation : ≤ 1 Mo
- Taille totale de téléchargement : ≤ 100 Mo

## Directives de Conception des Bâtiments

### 1. Spécifications de Taille

Concevez en fonction de la taille de grille occupée par le bâtiment. La base du bâtiment (fondation) doit mesurer au minimum 1 mètre de longueur et largeur :
Pour que l'occupation de grille du bâtiment soit correctement calculée, il est recommandé d'utiliser des carrés impairs pour la taille de base du bâtiment, tels que : 1×1, 3×3, 5×5. Les carrés pairs et les formes non carrées ne sont pas recommandés, tels que : 1×2, 2×2, 2×3, 4×4.
Hauteur : Aucune limite stricte, mais il est recommandé de ne pas descendre en dessous de 0,1 mètre

### 2. Style Visuel

Pour maintenir la cohérence du jeu, il est recommandé de :
- Utiliser le style LowPoly
- Assurer les proportions globales et les relations d'échelle du bâtiment (référence d'échelle par défaut du jeu : hauteur du personnage 0,1 mètre)
- Utiliser des designs adaptés à la vue isométrique
- Éviter les détails trop petits (difficiles à voir de loin)
- Utiliser des contours clairs et un contraste de couleurs
- Prendre en compte les détails sous différents angles

### 3. Recommandations d'Optimisation des Performances

- Contrôler raisonnablement le nombre de polygones
- Matériaux transparents (utiliser Alpha Test au lieu d'Alpha Blend)

## Processus de Création

### Étape 1 : Modélisation dans un Logiciel 3D

1. Créer des modèles avec Blender, Maya, 3ds Max, etc.
2. S'assurer que l'origine du modèle est au centre inférieur
En prenant Blender comme exemple :
✅ Correct : Le point central du bâtiment est à l'origine mondiale
((Image 1))
❌ Incorrect : Le bâtiment est décalé ou la rotation n'est pas alignée
((Image 2))
3. Appliquer toutes les transformations (Scale, Rotation)
4. Exporter au format FBX

### Étape 2 : Importer dans Unity

1. Glisser le FBX dans le projet Unity
2. Définir la Position du Transform du prefab de bâtiment sur (0, 0, 0)
3. S'assurer que la base du bâtiment est sur le plan Y=0
4. Le bâtiment doit faire face à la direction positive de l'axe Z

### Étape 3 : Configurer les Matériaux et Textures

1. Créer des matériaux (en utilisant URP Lit Shader)
2. Ajuster les paramètres des matériaux

### Étape 4 : Ajouter les Composants Requis

S'assurer que le prefab de bâtiment contient les trois **composants requis** suivants :

#### 1. Mesh Filter et Mesh Renderer

1. Sélectionner l'objet racine du bâtiment
2. Confirmer que le composant **Mesh Filter** a été ajouté (sinon, cliquer sur Add Component → Mesh Filter)
3. Confirmer que le composant **Mesh Renderer** a été ajouté
4. Assigner des matériaux dans le Mesh Renderer (utiliser Standard ou URP Lit Shader)

> ⚠️ **Important** : Les bâtiments sans Mesh Renderer ne s'afficheront pas et ne déclencheront pas l'effet de surbrillance au survol de la souris !

#### 2. Collider

1. Sélectionner l'objet racine du bâtiment
2. Ajouter un composant **Box Collider** (recommandé) ou **Mesh Collider**
3. Ajuster la taille du Collider pour couvrir tout le bâtiment
4. S'assurer que le Collider ne dépasse pas la portée du bâtiment, car cela entraînerait une collision avec d'autres bâtiments et empêcherait le placement
((Image 3))

> ⚠️ **Important** : Le Collider est une condition nécessaire pour la surbrillance au survol de la souris et la détection des clics ! Les bâtiments sans Collider ne peuvent pas être sélectionnés ou interagis par les joueurs.

### Étape 5 : Créer le Prefab

1. Glisser le bâtiment dans la fenêtre Project pour créer un Prefab
2. Convention de nommage : `{NomBâtiment}_Prefab`
3. Tester que le prefab peut être instancié normalement

### Étape 6 : Créer le BuildingData

1. Clic droit → **Create → City Builder → Building Data**
2. Remplir les champs suivants :

| **Building Name** | Nom d'affichage du bâtiment, peut utiliser un nom ou un numéro | Ex : Ferriswheel, SSmbl2 |
| **Building ID** | Identifiant unique, recommandé d'utiliser le nom du style plus un numéro, ou un numéro pur | Ex : SunsetCity_01, SSmbl2 |
| **Style** ⭐ | **Détermine sous quel onglet de style le bâtiment apparaît dans le menu de construction du jeu**, recommandé d'utiliser votre nom de développeur, nom de package ou nom de thème. Les bâtiments de la même série doivent avoir la même valeur | Ex : Lyon'sPack, SunsetCity |
| **Category** ⭐ | **Détermine dans quelle colonne de catégorie il apparaît sous ce style**, peut utiliser les catégories intégrées au jeu pour la compatibilité (voir tableau ci-dessous), ou remplir selon votre propre classification de bâtiment | Ex : Large, Residential |
| **Grid Size** | Taille d'occupation de grille du bâtiment | Ex : 3x3 |
| **Building Prefab** | Glisser votre prefab de bâtiment | — |

**Référence des Catégories Intégrées au Jeu** :

| `Large` | Grand |
| `Medium` | Moyen |
| `Small` | Petit |

> ⚠️ **Les champs Style et Category sont très importants** : Ils seront empaquetés dans `metadata.json` (écrit automatiquement par l'outil de téléchargement). Lorsque le jeu charge votre mod, il utilise ces deux champs pour déterminer la position du bâtiment dans le menu. Assurez-vous de les remplir, sinon le bâtiment risque de ne pas être correctement catégorisé et affiché.

3. Créer une icône de bâtiment (PNG 128×128 recommandé), la glisser dans le champ **Icon**

### Étape 7 : Tester le Bâtiment

1. Tester le bâtiment dans le jeu :
   - Peut-il être placé correctement
   - La rotation fonctionne-t-elle normalement
   - La détection de collision est-elle correcte
   - L'effet visuel est-il satisfaisant
2. Ajuster jusqu'à satisfaction

### Étape 8 : Préparer le Téléchargement

- Créer une image de prévisualisation (600×600 recommandé)
- Confirmer que **Style** et **Category** dans BuildingData sont correctement remplis (l'outil de téléchargement les lira automatiquement)
Liste de contrôle pré-téléchargement :
   - [ ] Le nombre de triangles du modèle est dans les limites
   - [ ] La résolution des textures est raisonnable
   - [ ] Contient le composant **Mesh Filter**
   - [ ] Contient le composant **Mesh Renderer** (matériau assigné)
   - [ ] Contient le composant **Collider**
   - [ ] Le prefab peut être instancié normalement
   - [ ] Le bâtiment est aligné sur la grille
   - [ ] Aucun matériau ou texture manquant
   - [ ] Tous les champs BuildingData sont remplis
   - [ ] L'image de prévisualisation est nette
   - [ ] Test en jeu réussi

## Étape 9 : Télécharger sur le Workshop

> ⚠️ **Le téléchargement se fait en deux étapes**, car l'empaquetage AssetBundle et le téléchargement Steam ont des exigences différentes pour l'état de l'éditeur, ils doivent donc être effectués séparément.

### Première Étape : Empaqueter les Ressources (Mode Non-Play)

1. S'assurer d'être en **Mode Non-Play** (l'éditeur ne fonctionne pas)
2. Ouvrir **City Builder → Workshop Upload Tool**
3. Sélectionner le type de mod : **Building**
4. Glisser le **BuildingData** (l'outil lira automatiquement Style et Category, peut être modifié manuellement)
5. Glisser le **prefab de bâtiment**
6. Remplir les informations du workshop (principalement le titre, la description peut être modifiée dans le Workshop)
7. Cliquer sur **"Première Étape : Empaqueter les ressources en AssetBundle"**
8. Quand vous voyez le message **"Empaquetage terminé ! Veuillez entrer en mode Play et cliquer sur la deuxième étape pour télécharger"**, c'est réussi

> 💡 Le chemin d'empaquetage sera automatiquement sauvegardé et ne sera pas perdu après être entré en mode Play, pas besoin de réentrer les informations.

### Deuxième Étape : Télécharger sur Steam (Mode Play)

1. Cliquer sur le bouton **Play** de l'éditeur Unity, attendre l'initialisation de SteamManager
2. Confirmer dans la fenêtre de l'outil que Steam est initialisé (aucun message d'avertissement)
3. Cliquer sur **"Deuxième Étape : Télécharger sur le Workshop Steam"**
4. Attendre la fin du téléchargement, noter l'**ID d'élément du Workshop** affiché (pour les mises à jour futures)

> 💡 L'outil de téléchargement générera automatiquement `metadata.json` et l'empaquettera dans le répertoire de contenu AssetBundle, qui contient les informations Style et Category. Après que les joueurs se soient abonnés, le jeu affichera automatiquement dans la bonne catégorie.

### Étape 10 : Publier et Maintenir

1. Compléter les informations sur la page du Workshop Steam
2. Ajouter plus de captures d'écran

## Notes sur les Matériaux de Ciel et de Sol
- Les shaders de ciel et de sol doivent utiliser le pipeline URP
- Les shaders de sol devraient de préférence utiliser des normales procédurales pour gérer les ajustements dynamiques de la taille de la zone de grille

## Erreurs Courantes et Solutions

### Erreur 0 : Erreur d'empaquetage "Building AssetBundles while in play mode is not allowed"

**Cause** : Le bouton d'empaquetage a été cliqué en mode Play, mais l'empaquetage AssetBundle ne peut être exécuté qu'en mode Non-Play

**Solution** :
1. Quitter le mode Play (cliquer sur le bouton d'arrêt)
2. Cliquer à nouveau sur "Première Étape : Empaqueter les ressources en AssetBundle" dans l'outil
3. Entrer en mode Play après la fin de l'empaquetage pour télécharger

### Erreur 1 : Le Bâtiment S'Affiche en Rose

**Cause** : Matériau ou shader manquant

**Solution** :
1. S'assurer que toutes les textures sont correctement assignées
2. Vérifier si les matériaux sont inclus dans l'AssetBundle

### Erreur 2 : Le Bâtiment Ne Peut Pas Être Placé

**Cause** : Composants requis manquants (Mesh Filter/Mesh Renderer/Collider) ou erreur de paramétrage de la taille de grille

**Solution** :
1. Confirmer que le prefab de bâtiment contient les composants **Mesh Filter**, **Mesh Renderer** et **Collider**
2. Vérifier si le Mesh Renderer a des matériaux assignés
3. Ajouter un Box Collider à l'objet racine du bâtiment
4. Vérifier le paramètre Grid Size du BuildingData
5. Confirmer les dimensions du bâtiment

### Erreur 3 : Décalage de Position du Bâtiment

**Cause** : Les paramètres du Transform du prefab sont incorrects

**Solution** :
1. Définir la Position du prefab sur (0, 0, 0)
2. S'assurer que la base du bâtiment est sur le plan Y=0
3. Vérifier la position du Pivot Point du modèle

### Erreur 4 : Décalage du Bâtiment Après Rotation

**Cause** : Le point central du bâtiment n'est pas au centre de la grille

**Solution** :
1. Ajuster l'origine du modèle dans le logiciel 3D
2. Ou ajuster la position des objets enfants dans Unity
3. S'assurer que le bâtiment tourne autour du point central

### Erreur 5 : Fichier Trop Volumineux pour Télécharger

**Cause** : Résolution de texture trop élevée ou modèle trop complexe

**Solution** :
1. Réduire la résolution des textures (ex : de 4K à 2K)
2. Optimiser le modèle pour réduire le nombre de polygones
3. Compresser les textures (utiliser le format DXT5 ou BC7)
4. Supprimer les détails inutiles

## Techniques Avancées

### Ajouter des Animations

Si votre bâtiment contient des animations (comme un moulin à vent rotatif) :
1. Créer un Animator Controller, glisser l'animation dans le contrôleur
2. Ajouter un composant Animator au prefab
3. Glisser l'Animator Controller dans le composant Animator

### Utiliser le Système LOD

Attacher des scripts de culling de détails pour les grands bâtiments :
1. Ajouter le script BuildingDetailCulling
2. Assigner différents niveaux LOD au modèle
3. Définir les distances de culling pour différents niveaux

### Effets de Matériau Personnalisés

Utiliser Shader Graph pour créer des effets spéciaux :
1. Créer un actif Shader Graph
2. Concevoir des effets personnalisés (comme des fenêtres lumineuses)
3. Créer un matériau utilisant ce shader
4. S'assurer que le shader est compatible avec la plateforme cible

## Conclusion

Merci pour vos contributions à la création de contenu communautaire. J'espère que le jeu aura une vitalité durable grâce à votre contenu !

J'ai hâte de voir vos créations ajouter plus de possibilités colorées au jeu !

---

**Version** : 1.0  
**Dernière Mise à Jour** : 2026  
**Applicable à** : Unity 2022.3 LTS
