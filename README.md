# azure-network-infrastructure-lab
Azure lab implementing hybrid network infrastructure with hub‑and‑spoke topology, UDR, and private/public DNS zones.

azure-network-infrastructure-lab/
│
├── README.md
├── scripts/
│   ├── create-vnets.ps1
│   ├── configure-udr.ps1
│   ├── configure-private-dns.ps1
│   ├── configure-public-dns.ps1
│   └── cleanup-lab.ps1
├── diagrams/
│   └── topology.png
└── notes/
    └── lab-steps.md

---

## 🎯 Objectifs pédagogiques

À la fin de ce laboratoire, vous serez capable de :

- Implémenter le **routage de réseau virtuel** dans Azure  
- Configurer une topologie **Hub-and-Spoke**  
- Tester la transitivité du peering VNet  
- Implémenter des **User Defined Routes (UDR)**  
- Configurer la **résolution DNS privée** via Azure Private DNS Zones  
- Configurer la **résolution DNS publique** via Azure DNS  
- Déprovisionner proprement l’environnement Azure

---

## 🏗️ Architecture du laboratoire

L’environnement déployé comprend :

- 1 réseau virtuel **Hub**  
- 2 réseaux virtuels **Spoke**  
- Peering Hub ↔ Spokes  
- Tables de routage personnalisées (UDR)  
- Zones DNS privées Azure  
- Zone DNS publique Azure  
- 3 machines virtuelles Azure (Standard_D2s_v3 ou B1s selon quota)

Topologie :

+------------------+
|      Hub VNet    |
|   (az800l08-vnet0)
+---------+--------+
|
-------------------------
|                       |
+---------------+      +----------------+
| Spoke VNet 1  |      | Spoke VNet 2   |
| az800l08-vnet1|      | az800l08-vnet2 |
+---------------+      +----------------+


---

## ⚙️ Exercice 1 — Routage réseau virtuel dans Azure

### 🔹 Étapes principales

1. Déployer l’infrastructure via un template ARM  
2. Configurer le peering Hub ↔ Spokes  
3. Tester la transitivité du peering (non transitive par défaut)  
4. Activer le routage sur la VM du Hub  
5. Créer des UDR pour forcer le trafic Spoke1 → Hub → Spoke2  
6. Valider la connectivité via Network Watcher

### 🔹 Exemples de commandes PowerShell

Création du groupe de ressources :
```powershell 
$location = '<Azure_region>'
$rgName = 'AZ800-L0801-RG'
New-AzResourceGroup -Name $rgName -Location $location 
#
New-AzResourceGroupDeployment `
   -ResourceGroupName $rgName `
   -TemplateFile $HOME/L08-rg_template.json `
   -TemplateParameterFile $HOME/L08-rg_template.parameters.json

Install-WindowsFeature RemoteAccess -IncludeManagementTools
Install-WindowsFeature -Name Routing -IncludeAllSubFeature
Install-RemoteAccess -VpnType RoutingOnly
Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled 

```
⚙️ Exercice 2 — Résolution DNS dans Azure
🔹 DNS privé (Private DNS Zone)
Création de la zone : contoso.org

Liaison automatique aux 3 VNets

Enregistrement automatique des VMs

Validation via Network Watcher (FQDN → IP privée)

🔹 DNS public (Azure DNS)
Création d’une zone publique

Ajout d’un enregistrement A (ex : www)

Test via nslookup :
nslookup www.<domain> <NameServer1>

🧹 Exercice 3 — Déprovisionnement de l’environnement
Suppression des ressources :
Get-AzResourceGroup -Name 'AZ800-L08*' | Remove-AzResourceGroup -Force -AsJob

🧠 Compétences développées
Architecture réseau Azure
Routage personnalisé (UDR)
Peering VNet
DNS privé et DNS public
Network Watcher
Automatisation PowerShell
Déploiement ARM





Laboratoire : Mise en œuvre d’une infrastructure de réseau hybride dans Azure
Ce laboratoire a pour objectif de créer un environnement de test dans Microsoft Azure composé de plusieurs réseaux virtuels organisés en topologie en étoile (hub‑and‑spoke).
L’environnement inclut la mise en œuvre du routage personnalisé (User Defined Routes), la configuration de la résolution DNS inter‑réseaux via des zones DNS privées Azure, ainsi que l’évaluation de la résolution de noms externes via Azure DNS.

⏱️ Durée estimée : 60 minutes  
🧪 Type : Laboratoire pratique Azure (AZ‑800 / AZ‑104)

🎯 Objectifs du laboratoire
À la fin de ce laboratoire, vous serez capable de :

Implémenter le routage de réseau virtuel dans Azure (UDR, routage via hub)

Implémenter la résolution de noms DNS entre réseaux virtuels Azure

Configurer et utiliser des zones DNS privées Azure

Tester la résolution de noms externes via Azure DNS

Déprovisionner proprement l’environnement Azure

🏗️ Architecture du laboratoire
1 réseau virtuel Hub

2 réseaux virtuels Spoke

Peering Hub ↔ Spokes

Tables de routage personnalisées (UDR)

Zones DNS privées Azure

Machines virtuelles réparties dans chaque réseau virtuel

⚙️ Étapes principales
Création des réseaux virtuels (Hub + Spokes)

Configuration du peering VNet‑to‑VNet

Création et association des User Defined Routes

Déploiement des machines virtuelles Azure

Création d’une zone DNS privée Azure

Liaison de la zone DNS aux réseaux virtuels

Tests de résolution DNS inter‑réseaux

Tests de résolution DNS externe via Azure DNS

Nettoyage et suppression de l’environnement

🖥️ Prérequis
Les machines virtuelles suivantes doivent être en cours d’exécution :

AZ‑800T00A‑SEA‑DC1

AZ‑800T00A‑ADM1

Les autres VMs du cours peuvent être actives mais ne sont pas nécessaires.

🧠 Compétences développées
Architecture réseau Azure

Routage personnalisé (UDR)

DNS privé Azure

Peering VNet

Déploiement et gestion de machines virtuelles

Automatisation et bonnes pratiques Azure

🧹 Nettoyage
Le laboratoire inclut une section dédiée au déprovisionnement complet de l’environnement Azure afin d’éviter des coûts inutiles.

📁 4. Structure recommandée du dépôt GitHub
Code
Azure-Hybrid-Network-Lab/
│
├── README.md
├── diagrams/
│   └── topology.png
├── scripts/
│   ├── create-vnets.ps1
│   ├── create-udr.ps1
│   ├── create-private-dns.ps1
│   └── cleanup.ps1
└── notes/
    └── lab-steps.md



# 🌐 Laboratoire : Mise en œuvre d’une infrastructure de réseau hybride dans Azure

Ce laboratoire pratique a pour objectif de déployer une infrastructure réseau hybride dans Microsoft Azure, incluant une topologie **Hub-and-Spoke**, du **routage personnalisé (UDR)**, la **résolution DNS privée**, la **résolution DNS publique**, ainsi que des tests de connectivité via **Network Watcher**.

⏱️ Durée estimée : 60 minutes  
📘 Niveau : Intermédiaire (AZ‑800 / AZ‑104)  
🧪 Type : Laboratoire Azure guidé

---

## 🎯 Objectifs pédagogiques

À la fin de ce laboratoire, vous serez capable de :

- Implémenter le **routage de réseau virtuel** dans Azure  
- Configurer une topologie **Hub-and-Spoke**  
- Tester la transitivité du peering VNet  
- Implémenter des **User Defined Routes (UDR)**  
- Configurer la **résolution DNS privée** via Azure Private DNS Zones  
- Configurer la **résolution DNS publique** via Azure DNS  
- Déprovisionner proprement l’environnement Azure

---

## 🏗️ Architecture du laboratoire

L’environnement déployé comprend :

- 1 réseau virtuel **Hub**  
- 2 réseaux virtuels **Spoke**  
- Peering Hub ↔ Spoke  
- Tables de routage personnalisées (UDR)  
- Zones DNS privées Azure  
- Zone DNS publique Azure  
- 3 machines virtuelles Azure (Standard_D2s_v3 ou B1s selon quota)

Topologie :  
+------------------+
|      Hub VNet    |
|   (az800l08-vnet0)
+---------+--------+
|
-------------------------
|                       |
+---------------+      +----------------+
| Spoke VNet 1  |      | Spoke VNet 2   |
| az800l08-vnet1|      | az800l08-vnet2 |
+---------------+      +----------------+

Code

---

## ⚙️ Exercice 1 — Routage réseau virtuel dans Azure

### 🔹 Étapes principales

1. **Déployer l’infrastructure** via un template ARM  
2. **Configurer le peering** Hub ↔ Spokes  
3. **Tester la transitivité** du peering (non transitive par défaut)  
4. **Activer le routage** sur la VM du Hub  
5. **Créer des UDR** pour forcer le trafic Spoke1 → Hub → Spoke2  
6. **Valider la connectivité** via Network Watcher

### 🔹 Scripts utilisés (extraits du lab)

Déploiement du groupe de ressources :
```powershell
$location = '<Azure_region>'
$rgName = 'AZ800-L0801-RG'
New-AzResourceGroup -Name $rgName -Location $location
Déploiement des ressources via ARM :

powershell
New-AzResourceGroupDeployment `
   -ResourceGroupName $rgName `
   -TemplateFile $HOME/L08-rg_template.json `
   -TemplateParameterFile $HOME/L08-rg_template.parameters.json
Activation du routage sur la VM Hub :

powershell
Install-WindowsFeature RemoteAccess -IncludeManagementTools
Install-WindowsFeature -Name Routing -IncludeAllSubFeature
Install-RemoteAccess -VpnType RoutingOnly
Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled

```
⚙️ Exercice 2 — Résolution DNS dans Azure
🔹 DNS privé (Private DNS Zone)
Création de la zone : contoso.org

Liaison automatique aux 3 VNets

Enregistrement automatique des VMs

Validation via Network Watcher (FQDN → IP privée)

🔹 DNS public (Azure DNS)
Création d’une zone publique

Ajout d’un enregistrement A (ex : www)

Test via nslookup :

powershell
nslookup www.<domain> <NameServer1>
🧹 Exercice 3 — Déprovisionnement de l’environnement
Suppression des ressources :

powershell
Get-AzResourceGroup -Name 'AZ800-L08*' | Remove-AzResourceGroup -Force -AsJob
📁 Structure recommandée du dépôt
Code
azure-network-infrastructure-lab/
│
├── README.md
├── scripts/
│   ├── create-vnets.ps1
│   ├── configure-udr.ps1
│   ├── configure-private-dns.ps1
│   ├── configure-public-dns.ps1
│   └── cleanup-lab.ps1
├── diagrams/
│   └── topology.png
└── notes/
    └── lab-steps.md
🧠 Compétences développées
Architecture réseau Azure

Routage personnalisé (UDR)

Peering VNet

DNS privé et DNS public

Network Watcher

Automatisation PowerShell

Déploiement ARM

✔️ Résultats obtenus
À la fin du laboratoire, vous aurez :

Déployé une topologie Hub‑and‑Spoke

Testé la transitivité du peering

Configuré un routeur Azure basé sur VM

Implémenté des UDR pour le trafic inter‑Spoke

Configuré Azure Private DNS Zones

Configuré Azure Public DNS

Nettoyé l’environnement Azure

👤 Auteur
DJEUMEDOM  
Laboratoire réalisé dans le cadre de la formation AZ‑800 / AZ‑104.

Code

---

# 🔥 Ton README est prêt à être collé dans GitHub.

Si tu veux, je peux aussi te générer :

✅ les **scripts PowerShell**  
✅ un **diagramme ASCII** ou image  
✅ une **version anglaise** du README  
✅ un **badge GitHub** (Azure, PowerShell, Networking)

Tu veux quoi en plus pour finaliser ton dépôt ?
qu'est-ce que je mets finalement dans le readme?
DJEUMEDOM, tu veux savoir exactement quoi mettre dans ton README, sans hésitation, sans flou, sans surcharge.

👉 Voici la version finale, complète, propre, professionnelle, prête à coller dans ton dépôt GitHub.  
Elle est basée à 100 % sur TON laboratoire (celui que tu as fourni dans le document).

Tu n’as qu’à copier‑coller tout ce bloc dans ton fichier README.md.

✅ README.md — Version finale à mettre dans ton dépôt GitHub
markdown
# 🌐 Laboratoire : Mise en œuvre d’une infrastructure de réseau hybride dans Azure

Ce laboratoire pratique consiste à déployer une infrastructure réseau hybride dans Microsoft Azure en utilisant une topologie **Hub-and-Spoke**, du **routage personnalisé (UDR)**, la **résolution DNS privée**, la **résolution DNS publique**, ainsi que des tests de connectivité via **Network Watcher**.

⏱️ Durée estimée : 60 minutes  
📘 Niveau : Intermédiaire (AZ‑800 / AZ‑104)  
🧪 Type : Laboratoire Azure guidé

---

## 🎯 Objectifs pédagogiques

À la fin de ce laboratoire, vous serez capable de :

- Implémenter le **routage de réseau virtuel** dans Azure  
- Configurer une topologie **Hub-and-Spoke**  
- Tester la transitivité du peering VNet  
- Implémenter des **User Defined Routes (UDR)**  
- Configurer la **résolution DNS privée** via Azure Private DNS Zones  
- Configurer la **résolution DNS publique** via Azure DNS  
- Déprovisionner proprement l’environnement Azure

---

## 🏗️ Architecture du laboratoire

L’environnement déployé comprend :

- 1 réseau virtuel **Hub**  
- 2 réseaux virtuels **Spoke**  
- Peering Hub ↔ Spokes  
- Tables de routage personnalisées (UDR)  
- Zones DNS privées Azure  
- Zone DNS publique Azure  
- 3 machines virtuelles Azure (Standard_D2s_v3 ou B1s selon quota)

Topologie :

+------------------+
|      Hub VNet    |
|   (az800l08-vnet0)
+---------+--------+
|
-------------------------
|                       |
+---------------+      +----------------+
| Spoke VNet 1  |      | Spoke VNet 2   |
| az800l08-vnet1|      | az800l08-vnet2 |
+---------------+      +----------------+

Code

---

## ⚙️ Exercice 1 — Routage réseau virtuel dans Azure

### 🔹 Étapes principales

1. Déployer l’infrastructure via un template ARM  
2. Configurer le peering Hub ↔ Spokes  
3. Tester la transitivité du peering (non transitive par défaut)  
4. Activer le routage sur la VM du Hub  
5. Créer des UDR pour forcer le trafic Spoke1 → Hub → Spoke2  
6. Valider la connectivité via Network Watcher

### 🔹 Exemples de commandes PowerShell

Création du groupe de ressources :
```powershell
$location = '<Azure_region>'
$rgName = 'AZ800-L0801-RG'
New-AzResourceGroup -Name $rgName -Location $location
Déploiement ARM :

powershell
New-AzResourceGroupDeployment `
   -ResourceGroupName $rgName `
   -TemplateFile $HOME/L08-rg_template.json `
   -TemplateParameterFile $HOME/L08-rg_template.parameters.json
Activation du routage sur la VM Hub :

powershell
Install-WindowsFeature RemoteAccess -IncludeManagementTools
Install-WindowsFeature -Name Routing -IncludeAllSubFeature
Install-RemoteAccess -VpnType RoutingOnly
Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled
⚙️ Exercice 2 — Résolution DNS dans Azure
🔹 DNS privé (Private DNS Zone)
Création de la zone : contoso.org

Liaison automatique aux 3 VNets

Enregistrement automatique des VMs

Validation via Network Watcher (FQDN → IP privée)

🔹 DNS public (Azure DNS)
Création d’une zone publique

Ajout d’un enregistrement A (ex : www)

Test via nslookup :

powershell
nslookup www.<domain> <NameServer1>
🧹 Exercice 3 — Déprovisionnement de l’environnement
Suppression des ressources :

powershell
Get-AzResourceGroup -Name 'AZ800-L08*' | Remove-AzResourceGroup -Force -AsJob
📁 Structure recommandée du dépôt
Code
azure-network-infrastructure-lab/
│
├── README.md
├── scripts/
│   ├── create-vnets.ps1
│   ├── configure-udr.ps1
│   ├── configure-private-dns.ps1
│   ├── configure-public-dns.ps1
│   └── cleanup-lab.ps1
├── diagrams/
│   └── topology.png
└── notes/
    └── lab-steps.md
🧠 Compétences développées
Architecture réseau Azure

Routage personnalisé (UDR)

Peering VNet

DNS privé et DNS public

Network Watcher

Automatisation PowerShell

Déploiement ARM

✔️ Résultats obtenus
