# Formation Complète : Vision par Ordinateur avec Python (de Zéro)

> Objectif de cette formation : partir de zéro en vision par ordinateur (Computer Vision) et arriver à un niveau où tu peux coder, comprendre, expliquer et déployer des systèmes de vision robustes — du traitement d'image classique jusqu'au Deep Learning moderne (CNN, YOLO, transfer learning) avec PyTorch.
>
> **Prérequis** : bases solides en Python (variables, fonctions, POO, manipulation de fichiers, notions de NumPy si possible).

---

## Sommaire

1. [Introduction : qu'est-ce que la vision par ordinateur ?](#1-introduction)
2. [Phase 1 — Fondations mathématiques minimales](#phase-1)
3. [Phase 2 — Traitement d'image classique avec OpenCV](#phase-2)
4. [Phase 3 — Deep Learning appliqué à la vision avec PyTorch](#phase-3)
5. [Phase 4 — Vision moderne : détection, segmentation, transfer learning](#phase-4)
6. [Projets pratiques pour t'exercer](#projets)
7. [Ressources pour aller plus loin](#ressources)

---

<a name="1-introduction"></a>
## 1. Introduction : qu'est-ce que la vision par ordinateur ?

La vision par ordinateur (Computer Vision, CV) est le domaine de l'intelligence artificielle qui permet à une machine de **comprendre le contenu d'une image ou d'une vidéo** : reconnaître des objets, détecter des visages, lire du texte, suivre un mouvement, mesurer des distances, etc.

Concrètement, une image pour un ordinateur n'est qu'une **matrice de nombres** (les pixels). Toute la vision par ordinateur consiste à transformer cette matrice de nombres en informations utiles : "il y a un chat ici", "ce texte dit X", "cet objet se déplace vers la droite".

![Illustration réseau de neurones](https://commons.wikimedia.org/wiki/Special:FilePath/Artificial%20neural%20network.svg)

On distingue deux grandes approches, complémentaires plutôt qu'opposées :

- **Vision classique** : on écrit des règles explicites (filtres, seuils, détecteurs de contours) pour extraire de l'information. Rapide, léger, explicable — mais limité face à des scènes complexes.
- **Vision par Deep Learning** : un réseau de neurones apprend lui-même à reconnaître les motifs à partir de milliers d'exemples. Plus puissant sur des tâches complexes, mais plus gourmand en données et en calcul.

Cette formation te fait traverser les deux, dans l'ordre, parce que comprendre la vision classique te donne l'intuition dont tu as besoin pour ne pas traiter le Deep Learning comme une boîte noire.

---

<a name="phase-1"></a>
## 2. Phase 1 — Fondations mathématiques minimales (1-2 semaines)

Pas besoin de devenir mathématicien. Tu as besoin de trois briques, avec l'intuition derrière, pas la théorie complète.

### 2.1 Les images sont des matrices

Une image en niveaux de gris de 100×100 pixels est une matrice 100×100 où chaque valeur va de 0 (noir) à 255 (blanc). Une image couleur (RGB) est en réalité **3 matrices superposées** (une par canal : Rouge, Vert, Bleu).

```python
import numpy as np

# Une "image" en niveaux de gris de 5x5 pixels, à la main
image = np.array([
    [0, 0, 0, 0, 0],
    [0, 255, 255, 255, 0],
    [0, 255, 255, 255, 0],
    [0, 255, 255, 255, 0],
    [0, 0, 0, 0, 0]
])
print(image.shape)  # (5, 5)
```

### 2.2 La convolution — le concept central de toute la CV

La convolution consiste à faire glisser une petite matrice (appelée **kernel** ou **filtre**) sur l'image, et à calculer à chaque position une somme pondérée des pixels sous le filtre. C'est cette opération, répétée avec différents filtres, qui permet de détecter des contours, des textures, puis — dans un CNN — des formes de plus en plus complexes (yeux, roues, visages...).

![Diagramme de convolution CNN](https://commons.wikimedia.org/wiki/Special:FilePath/CNN%20Convolutional%20Layers.svg)

```python
import numpy as np

def convolution_2d(image, kernel):
    kh, kw = kernel.shape
    ih, iw = image.shape
    output = np.zeros((ih - kh + 1, iw - kw + 1))
    for i in range(output.shape[0]):
        for j in range(output.shape[1]):
            region = image[i:i+kh, j:j+kw]
            output[i, j] = np.sum(region * kernel)
    return output

# Un filtre qui détecte les contours verticaux
kernel_vertical = np.array([
    [-1, 0, 1],
    [-1, 0, 1],
    [-1, 0, 1]
])

resultat = convolution_2d(image, kernel_vertical)
print(resultat)
```

**À retenir** : dans le Deep Learning, on n'écrit plus ces filtres à la main — le réseau les **apprend** automatiquement à partir des données. Mais l'opération mathématique reste exactement celle-ci.

### 2.3 Notions de probabilités et de métriques

Tu n'as pas besoin de cours dédié, juste des intuitions que tu croiseras constamment :

- **Probabilité / score de confiance** : un modèle de classification ne dit jamais "c'est un chat", il dit "87% de chance que ce soit un chat".
- **Précision (precision) vs Rappel (recall)** : precision = "parmi ce que j'ai détecté, combien est correct ?", recall = "parmi tout ce qui existait à détecter, combien ai-je trouvé ?". Ces deux métriques reviendront à chaque évaluation de modèle.
- **IoU (Intersection over Union)** : mesure le recouvrement entre une boîte prédite et la vraie boîte, essentielle en détection d'objets (Phase 4).

### Ressources pour cette phase
- Chaîne YouTube **3Blue1Brown** — série "Neural Networks" (excellente intuition visuelle, gratuite)
- Pas besoin d'aller plus loin en maths pour l'instant — tu approfondiras au fil des besoins.

---

<a name="phase-2"></a>
## 3. Phase 2 — Traitement d'image classique avec OpenCV (3-4 semaines)

C'est l'étape la plus souvent négligée par ceux qui se jettent directement sur le Deep Learning — et c'est une erreur. Comprendre comment une image se manipule "à la main" te rend bien meilleur pour déboguer un pipeline de Deep Learning plus tard (prétraitement, augmentation de données, post-traitement des résultats).

### 3.1 Installation

```bash
pip install opencv-python numpy matplotlib
```

### 3.2 Lecture, affichage, espaces colorimétriques

```python
import cv2
import matplotlib.pyplot as plt

# Lecture d'une image (OpenCV charge en BGR, pas RGB !)
image = cv2.imread("photo.jpg")

# Conversion BGR -> RGB pour un affichage correct avec matplotlib
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

# Conversion en niveaux de gris
gris = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Conversion en HSV (utile pour la détection de couleur, plus robuste que RGB)
hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

plt.imshow(image_rgb)
plt.axis("off")
plt.show()
```

**Piège classique à connaître** : OpenCV charge les images en **BGR** (Bleu-Vert-Rouge), pas en RGB. C'est une source de bugs très fréquente chez les débutants — si tes couleurs semblent inversées, c'est probablement ça.

### 3.3 Filtrage et flou

```python
# Flou gaussien (réduit le bruit, souvent une étape de prétraitement)
flou = cv2.GaussianBlur(gris, (5, 5), 0)

# Détection de contours avec Canny (l'algorithme classique de référence)
contours = cv2.Canny(flou, threshold1=50, threshold2=150)

# Détecteur de Sobel (calcule le gradient, donc la direction des contours)
sobel_x = cv2.Sobel(gris, cv2.CV_64F, 1, 0, ksize=3)
sobel_y = cv2.Sobel(gris, cv2.CV_64F, 0, 1, ksize=3)
```

### 3.4 Seuillage et morphologie

Le seuillage sépare une image en zones "pertinentes" et "non pertinentes" en fonction de l'intensité des pixels. C'est la base de beaucoup de systèmes de détection simples (avant deep learning).

```python
# Seuillage binaire simple
_, binaire = cv2.threshold(gris, 127, 255, cv2.THRESH_BINARY)

# Seuillage adaptatif (utile quand l'éclairage n'est pas uniforme)
adaptatif = cv2.adaptiveThreshold(
    gris, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 11, 2
)

# Morphologie : érosion et dilatation (nettoient le bruit dans une image binaire)
kernel = np.ones((5, 5), np.uint8)
erosion = cv2.erode(binaire, kernel, iterations=1)
dilatation = cv2.dilate(binaire, kernel, iterations=1)
```

### 3.5 Détection de formes et de contours

```python
contours_list, _ = cv2.findContours(binaire, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

for c in contours_list:
    aire = cv2.contourArea(c)
    if aire > 500:  # filtrer le bruit
        x, y, w, h = cv2.boundingRect(c)
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)
```

### 3.6 Détection de couleur (avec HSV)

```python
# Détecter la couleur rouge par exemple
bas_rouge = np.array([0, 120, 70])
haut_rouge = np.array([10, 255, 255])
masque = cv2.inRange(hsv, bas_rouge, haut_rouge)

resultat = cv2.bitwise_and(image, image, mask=masque)
```

### 3.7 Mini-projets pour valider cette phase

- **Détecteur de mouvement** : différence entre deux frames successives d'une vidéo webcam, seuillage, affichage d'un rectangle sur les zones de mouvement.
- **Compteur d'objets par couleur** : compter des objets d'une couleur donnée sur une photo (ex : compter des billes rouges).
- **OCR simple** : extraire du texte d'une image avec `pytesseract` (installation : `pip install pytesseract`, nécessite aussi le binaire Tesseract sur le système).

---

<a name="phase-3"></a>
## 4. Phase 3 — Deep Learning appliqué à la vision avec PyTorch (4-6 semaines)

### 4.1 Pourquoi PyTorch ?

PyTorch est aujourd'hui le framework le plus utilisé en recherche et de plus en plus en production. Sa syntaxe est proche de NumPy, le debug est plus naturel (exécution "eager", pas de graphe statique à compiler), et l'écosystème (Ultralytics/YOLO, Hugging Face, torchvision) est immense.

```bash
pip install torch torchvision
```

### 4.2 Intuition sur les réseaux de neurones

Un réseau de neurones "classique" (dense / fully connected) prend un vecteur de nombres en entrée, applique une succession de transformations linéaires + non-linéarités (fonctions d'activation), et produit une sortie. Il apprend en ajustant ses poids pour minimiser une erreur (fonction de perte / loss), via un algorithme appelé **rétropropagation du gradient** (backpropagation).

![Exemple de réseau de neurones](https://commons.wikimedia.org/wiki/Special:FilePath/Neural%20network%20example.svg)

**Tu n'as pas besoin de dériver les maths de la backpropagation à la main.** L'intuition suffit : le réseau se trompe, on mesure de combien, on ajuste chaque poids un peu dans la direction qui réduit l'erreur, on répète des milliers de fois.

### 4.3 Pourquoi un CNN et pas un réseau dense pour les images ?

Un réseau dense classique, appliqué directement sur les pixels d'une image, aurait un nombre de paramètres énorme (une image 224×224 en couleur = 150 528 valeurs d'entrée) et **ne capturerait pas la structure spatiale** de l'image (le fait qu'un pixel est lié à ses voisins).

Un CNN (Convolutional Neural Network) résout ça avec deux idées clés :
- **Poids partagés** : le même filtre (kernel) glisse sur toute l'image (voir Phase 1) → beaucoup moins de paramètres.
- **Hiérarchie de motifs** : les premières couches détectent des contours simples, les couches suivantes combinent ces contours en formes, puis en objets.

![Architecture AlexNet](https://commons.wikimedia.org/wiki/Special:FilePath/Alexnet.svg)

### 4.4 Ton premier CNN avec PyTorch

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class SimpleCNN(nn.Module):
    def __init__(self, nb_classes=10):
        super().__init__()
        self.conv1 = nn.Conv2d(in_channels=3, out_channels=16, kernel_size=3, padding=1)
        self.conv2 = nn.Conv2d(in_channels=16, out_channels=32, kernel_size=3, padding=1)
        self.pool = nn.MaxPool2d(2, 2)
        self.fc1 = nn.Linear(32 * 8 * 8, 128)
        self.fc2 = nn.Linear(128, nb_classes)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))   # -> 16 x 16 x 16
        x = self.pool(F.relu(self.conv2(x)))   # -> 32 x 8 x 8
        x = x.view(x.size(0), -1)              # aplatir
        x = F.relu(self.fc1(x))
        x = self.fc2(x)
        return x

modele = SimpleCNN(nb_classes=10)
print(modele)
```

### 4.5 Entraîner ce CNN sur CIFAR-10

CIFAR-10 est un dataset classique de 60 000 petites images (32×32) réparties en 10 classes (avion, voiture, chat, chien...). Parfait pour un premier entraînement complet.

```python
import torch
import torchvision
import torchvision.transforms as transforms

transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))
])

trainset = torchvision.datasets.CIFAR10(root="./data", train=True, download=True, transform=transform)
trainloader = torch.utils.data.DataLoader(trainset, batch_size=64, shuffle=True)

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
modele = SimpleCNN(nb_classes=10).to(device)

criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(modele.parameters(), lr=0.001)

for epoch in range(5):
    perte_totale = 0.0
    for images, labels in trainloader:
        images, labels = images.to(device), labels.to(device)

        optimizer.zero_grad()
        sorties = modele(images)
        perte = criterion(sorties, labels)
        perte.backward()
        optimizer.step()

        perte_totale += perte.item()
    print(f"Epoch {epoch+1} — perte moyenne : {perte_totale / len(trainloader):.4f}")
```

### 4.6 Vocabulaire essentiel à maîtriser

- **Epoch** : un passage complet sur tout le dataset d'entraînement.
- **Batch** : un petit paquet d'images traité en une fois (pour des raisons de mémoire et de stabilité de l'apprentissage).
- **Loss (perte)** : la mesure de l'erreur du modèle, qu'on cherche à minimiser.
- **Overfitting** : le modèle "apprend par cœur" les données d'entraînement mais généralise mal sur de nouvelles données. Se détecte quand la performance sur le jeu d'entraînement continue de s'améliorer alors que la performance sur le jeu de validation stagne ou se dégrade.
- **Learning rate** : la taille du pas d'ajustement des poids à chaque itération. Trop grand → le modèle diverge. Trop petit → l'apprentissage est très lent.

---

<a name="phase-4"></a>
## 5. Phase 4 — Vision moderne : détection, segmentation, transfer learning

### 5.1 Transfer Learning — l'outil le plus important en pratique

Entraîner un CNN performant "from scratch" nécessite énormément de données et de puissance de calcul (GPU professionnels, jours d'entraînement). En pratique, **on part presque toujours d'un modèle déjà entraîné** sur un immense dataset (ImageNet, 1.2 million d'images) et on l'adapte à notre propre tâche. C'est le transfer learning.

```python
import torchvision.models as models
import torch.nn as nn

# Charger un ResNet18 pré-entraîné sur ImageNet
modele = models.resnet18(weights=models.ResNet18_Weights.DEFAULT)

# Geler les poids du réseau (on ne les réentraîne pas)
for param in modele.parameters():
    param.requires_grad = False

# Remplacer la dernière couche pour l'adapter à NOTRE problème (ex: 3 classes)
modele.fc = nn.Linear(modele.fc.in_features, 3)

# Seule cette dernière couche sera entraînée -> rapide, peu de données nécessaires
```

C'est la technique que tu utiliseras dans l'immense majorité des projets réels, y compris professionnels.

### 5.2 Détection d'objets avec YOLO

Contrairement à la classification (une image = une étiquette), la **détection d'objets** localise et identifie plusieurs objets dans une même image (boîtes englobantes + classes). YOLO (You Only Look Once) est l'architecture de référence : rapide, bien documentée, utilisable quasiment en temps réel, même sur des machines modestes.

```bash
pip install ultralytics
```

```python
from ultralytics import YOLO

# Charger un modèle pré-entraîné
modele = YOLO("yolov8n.pt")  # version "nano", légère et rapide

# Détection sur une image
resultats = modele("photo.jpg")
resultats[0].show()  # affiche l'image avec les boîtes détectées

# Détection en temps réel sur webcam
resultats = modele(source=0, show=True)  # 0 = webcam par défaut
```

Entraîner YOLO sur tes **propres classes** (ex : détecter un objet spécifique à ton projet) demande juste un dataset annoté au format YOLO et quelques lignes :

```python
modele = YOLO("yolov8n.pt")
modele.train(data="mon_dataset.yaml", epochs=50, imgsz=640)
```

### 5.3 Segmentation d'image

La segmentation va plus loin que la détection : au lieu d'une boîte autour de l'objet, elle détermine **exactement quels pixels appartiennent à l'objet**. Utile pour du floutage d'arrière-plan, de l'analyse médicale, du comptage précis, etc.

```python
from ultralytics import YOLO

modele_seg = YOLO("yolov8n-seg.pt")
resultats = modele_seg("photo.jpg")
resultats[0].show()
```

### 5.4 Reconnaissance faciale et OCR avancé (selon ton intérêt)

- **Reconnaissance faciale** : bibliothèque `face_recognition` (basée sur dlib) pour un démarrage rapide, ou des modèles plus avancés comme ArcFace pour la production.
- **OCR avancé** : `EasyOCR` ou `PaddleOCR` sont bien plus robustes que Tesseract sur du texte "en conditions réelles" (photos, angles, éclairages variés).

---

<a name="projets"></a>
## 6. Projets pratiques pour t'exercer

Une liste volontairement variée en difficulté, pour progresser sans avoir à chercher quoi faire ensuite :

1. **Détecteur de mouvement webcam** (Phase 2) — alerte visuelle quand quelque chose bouge dans le champ de la caméra.
2. **Compteur d'objets par couleur** (Phase 2) — compte des objets d'une couleur donnée sur une image fixe.
3. **Scanner de documents** (Phase 2) — détecte les bords d'une feuille de papier photographiée, applique une transformation de perspective pour "redresser" le document.
4. **Classifieur d'images personnalisé** (Phase 3) — entraîne un CNN (ou fais du transfer learning) pour reconnaître tes propres catégories (ex : reconnaître 5 types de fruits).
5. **Détecteur d'objets personnalisé avec YOLO** (Phase 4) — annote un petit dataset (avec un outil comme Roboflow ou LabelImg) et entraîne YOLO à détecter un objet de ton choix.
6. **Système de comptage de personnes/véhicules** (Phase 4) — combine détection YOLO + suivi (tracking) sur une vidéo pour compter les passages.
7. **Application de floutage d'arrière-plan** (Phase 4, segmentation) — façon "flou d'arrière-plan" de visioconférence, avec la segmentation.
8. **OCR + extraction structurée** (Phase 2/4) — extrait le texte d'un reçu ou d'une carte d'identité et structure les champs (nom, date, montant...).
9. **Projet libre / signature** — combine plusieurs briques de cette formation (détection + tracking + interface) sur un cas d'usage qui te tient à cœur, pour en faire un projet démonstratif complet.

---

<a name="ressources"></a>
## 7. Ressources pour aller plus loin

- **PyImageSearch** (Adrian Rosebrock) — tutoriels très orientés pratique, référence pour OpenCV.
- **CS231n – Stanford** ("Convolutional Neural Networks for Visual Recognition") — cours gratuit en ligne, la référence académique une fois les bases OpenCV posées.
- **Documentation officielle Ultralytics (YOLO)** — très accessible, exemples concrets à copier-coller.
- **Documentation PyTorch** (pytorch.org) — tutoriels officiels "60 minute blitz" pour démarrer vite.
- **Chaîne 3Blue1Brown** — intuition visuelle sur les réseaux de neurones (déjà mentionnée en Phase 1).
- **Papers with Code** (paperswithcode.com) — pour suivre l'état de l'art et trouver des implémentations de référence.

---

*Formation rédigée pour un apprentissage en autonomie, avec des bases Python déjà acquises. Chaque phase peut être suivie de manière séquentielle ; les projets de la section 6 sont classés du plus simple au plus avancé et correspondent chacun à une phase de la formation.*
