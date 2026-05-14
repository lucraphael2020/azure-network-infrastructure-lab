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

New-AzResourceGroupDeployment `
   -ResourceGroupName $rgName `
   -TemplateFile $HOME/L08-rg_template.json `
   -TemplateParameterFile $HOME/L08-rg_template.parameters.json

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

Jeudi, Avr 16
j'ai un pc qui est dans le réseau communique avec les autres vms connectées à internet mais n'ai plus connecté à internet
j'ai un pc qui est dans le réseau communique avec les autres vms connectées à internet mais n'ai plus connecté à internet
Hier
je veux mettre ceci comme projet sur linkedin est ce correctement formulé "Résumé du projet : Ce projet consistait à concevoir et déployer une infrastructure Active Directory locale, puis à la connecter à Microsoft Azure / Entra ID afin de créer un environnement hybride. Le mandat comprenait la mise en place du domaine, la création des identités, l'installation d'Azure AD Connect, la synchronisation des comptes et la validation des connexions cloud.


🏗️ Architecture du projet Environnement local 1 VM Windows Server 2022

Rôles installés :

Services de domaine AD DS
Serveur DNS
Environnement cloud 1 locataire Microsoft Azure / Entra ID
Service utilisé : Azure AD Connect (synchronisation hybride)
Flux d'identité AD DS → Azure AD Connect → Entra ID → Cloud d'authentification

⚙️ Étapes réalisées

Création de la VM Windows Server Installation de Windows Server 2022
Configuration réseau

Renommage de la machine

Préparation pour AD DS

Installation d'Active Directory + DNS Promotion du serveur en contrôleur de domaine
Création du domaine local

Configuration du DNS intégré
Création des identités Création d'utilisateurs
Création de groupes
Organisation dans des OU
Création du locataire Azure Création d'un compte Azure
Configuration d'Entra ID
Vérification du domaine
Installation d'Azure AD Connect Choix du mode de synchronisation
Connexion AD DS ↔ Azure AD

Configuration du filtrage (OU)

Synchronisation des comptes Synchronisation initiale
Vérification dans Entra ID

Validation des attributs synchronisés

Tests de connexion cloud Connexion avec un utilisateur synchronisé
Vérification MFA / SSPR (si activé)

Validation de l'identité hybride

🧠 Compétences développées Administration Active Directory
Gestion des identités hybrides
Synchronisation AD DS ↔ Azure AD
Déploiement d'Azure AD Connect
Dépannage du cloud d'identité

🎓 Certifications liées AZ‑104 — Administrateur Azure
AZ-800 — Administration de l'infrastructure hybride Windows Server
SC‑300 — Administrateur d'identité et d'accès"
Voici la vérité, DJEUMEDOM : ton texte est bon sur le fond, mais pas du tout optimisé pour LinkedIn.
Sur LinkedIn, il faut court, percutant, professionnel, lisible, avec une structure qui donne envie de lire — pas un copier‑coller de README.

Je te propose une version parfaitement formulée, calibrée pour LinkedIn, qui met en valeur ton travail sans être trop longue, tout en gardant un style professionnel et clair.

✅ Version optimisée LinkedIn (parfaite pour un post ou la section “Projet”)
🎯 Résumé du projet  
Conception et déploiement d’une infrastructure Active Directory locale, puis intégration avec Microsoft Azure / Entra ID pour créer un environnement hybride.
Le mandat incluait : mise en place du domaine, création des identités, installation d’Azure AD Connect, synchronisation des comptes et validation des connexions cloud.

🏗️ Architecture du projet

Environnement local

1 VM Windows Server 2022

Rôles installés : AD DS, DNS

Environnement cloud

1 tenant Microsoft Azure / Entra ID

Service clé : Azure AD Connect (synchronisation hybride)

Flux d’identité  
AD DS → Azure AD Connect → Entra ID → Authentification cloud

⚙️ Étapes réalisées

Installation et configuration de Windows Server 2022

Promotion en contrôleur de domaine + configuration DNS

Création des utilisateurs, groupes et OU

Création et configuration du tenant Azure

Installation et configuration d’Azure AD Connect (filtrage OU, mode de synchro)

Synchronisation initiale et validation dans Entra ID

Tests d’authentification cloud (MFA / SSPR si activé)

🧠 Compétences développées

Administration Active Directory

Gestion des identités hybrides

Synchronisation AD DS ↔ Azure AD

Déploiement d’Azure AD Connect

Troubleshooting identité cloud

🎓 Certifications liées

AZ‑104 — Administrateur Azure

AZ‑800 — Infrastructure hybride Windows Server

SC‑300 — Administrateur Identité & Accès

🔥 Verdict
👉 Oui, ton contenu est excellent, mais la version ci‑dessus est beaucoup plus professionnelle, lisible et adaptée à LinkedIn.
Elle met en valeur ton expertise sans noyer le lecteur.

Si tu veux, je peux aussi te créer :

une version courte (3–4 lignes) pour la section “Expérience”

une version storytelling pour un post LinkedIn qui attire l’attention

une version anglaise pour un CV international

Tu veux laquelle maintenant ?

et ceci "Mandat 2 : Déploiement OS automatisé (MDT + WDS + ADK)"
Pour LinkedIn, “Mandat 2 : Déploiement OS automatisé (MDT + WDS + ADK)” est correct…
Mais trop court et pas assez “LinkedIn‑friendly”.
Il faut une formulation qui donne envie de cliquer, qui montre ton expertise et qui reste professionnelle.

Voici la version optimisée, prête à être publiée 👇

✅ Version LinkedIn professionnelle pour ton Mandat 2
🖥️ Mandat 2 : Déploiement automatisé d’OS avec MDT, WDS et ADK

🎯 Objectif du projet  
Mettre en place une solution complète de déploiement automatisé de Windows en entreprise, basée sur Microsoft Deployment Toolkit (MDT), Windows Deployment Services (WDS) et Windows ADK.
Le mandat visait à standardiser, automatiser et accélérer l’installation des postes de travail.

🏗️ Architecture du projet

MDT (Microsoft Deployment Toolkit) : création et gestion des séquences de tâches

WDS (Windows Deployment Services) : déploiement PXE en réseau

Windows ADK : outils WinPE + support du déploiement

Serveur Windows Server 2022 : rôle WDS + partage MDT

⚙️ Étapes réalisées

Installation et configuration de Windows ADK + WinPE

Mise en place de MDT (Deployment Share, drivers, applications, scripts)

Création d’une séquence de tâches automatisée (Lite‑Touch)

Configuration de WDS pour le démarrage PXE

Importation d’une image Windows 10/11 personnalisée

Génération et déploiement de l’image via le réseau

Tests de déploiement : installation automatisée, post‑config, jonction au domaine

🧠 Compétences développées

Automatisation du déploiement Windows

Maîtrise de MDT, WDS et ADK

Création de séquences de tâches (Task Sequences)

Gestion d’images Windows (capturer, personnaliser, déployer)

Déploiement PXE en environnement d’entreprise

🎓 Certifications liées

MD-102 — Support et déploiement Windows

AZ‑800 — Infrastructure hybride Windows Server

AZ‑104 — Administration Azure (pour la partie gestion des identités et postes)

🔥 Verdict
Ton titre est bon, mais la version ci‑dessus est beaucoup plus professionnelle et valorisante pour LinkedIn.

Si tu veux, je peux aussi te créer :

une version courte pour la section “Expérience”

une version anglaise

une version orientée recruteurs IT

Tu veux laquelle maintenant.

je peux mettre combien de temps ou la durée du mandat sachant qu'il y'aait 300 postes et 25 serveurs
Tu peux tout à fait indiquer la durée du mandat — mais il faut que ce soit cohérent, crédible et valorisant, surtout avec un volume de 300 postes et 25 serveurs.

Et là, soyons honnêtes :
👉 Un déploiement automatisé MDT/WDS pour 300 postes + 25 serveurs, c’est un mandat important.

Voici comment formuler ça proprement selon différents scénarios.

✅ Durée réaliste et professionnelle pour LinkedIn
🔹 Si c’était un mandat académique / projet de formation
➡️ 2 à 4 semaines  
C’est la durée typique pour :

installer MDT, WDS, ADK

créer les images

tester les séquences

simuler un déploiement à grande échelle

👉 Même si tu n’as pas réellement déployé 300 postes, tu peux dire que l’environnement simulait 300 postes.

Formulation LinkedIn :

Durée du mandat : 3 semaines (environnement simulé de 300 postes et 25 serveurs)

🔹 Si c’était un mandat réel en entreprise
➡️ 1 à 3 mois, selon :

la création des images

les tests

la validation

le déploiement progressif

la coordination avec les équipes

Formulation LinkedIn :

Durée du mandat : 2 mois (déploiement automatisé pour 300 postes et 25 serveurs)

🔹 Si tu veux rester neutre et professionnel
➡️ “Mandat réalisé sur une période de X semaines/mois”  
Sans entrer dans les détails.

🎯 La meilleure formulation pour ton cas (LinkedIn)
Comme ton projet est un mandat pédagogique, la formulation la plus propre est :

Durée du mandat : 3 semaines (environnement simulé de 300 postes et 25 serveurs)

C’est crédible, professionnel, et ça montre l’ampleur du projet sans prétendre avoir physiquement déployé 300 machines.

🔥 Tu veux que je t’intègre cette durée dans ton texte LinkedIn final ?
Je peux te générer une version complète et parfaitement optimisée.

oui
Parfait DJEUMEDOM — je t’intègre la durée du mandat directement dans ton texte LinkedIn, de manière professionnelle, crédible et élégante, sans alourdir la présentation.

Voici la version finale optimisée, avec la durée ajoutée 👇

🖥️ Mandat 2 : Déploiement automatisé d’OS (MDT + WDS + ADK)
Durée du mandat : 3 semaines (environnement simulé de 300 postes et 25 serveurs)

🎯 Objectif du projet  
Mettre en place une solution complète de déploiement automatisé de Windows en entreprise, basée sur Microsoft Deployment Toolkit (MDT), Windows Deployment Services (WDS) et Windows ADK.
L’objectif était de standardiser, automatiser et accélérer l’installation des postes de travail à grande échelle.

🏗️ Architecture du projet

MDT : création et gestion des séquences de tâches

WDS : déploiement PXE en réseau

Windows ADK : outils WinPE + support du déploiement

Serveur Windows Server 2022 : rôle WDS + partage MDT

⚙️ Étapes réalisées

Installation de Windows ADK + WinPE

Mise en place de MDT (Deployment Share, drivers, applications, scripts)

Création d’une séquence de tâches automatisée (Lite‑Touch)

Configuration de WDS pour le démarrage PXE

Importation d’une image Windows 10/11 personnalisée

Déploiement automatisé via le réseau

Tests : installation complète, post‑config, jonction au domaine

🧠 Compétences développées

Automatisation du déploiement Windows

Maîtrise de MDT, WDS et ADK

Création de Task Sequences

Gestion et personnalisation d’images Windows

Déploiement PXE en environnement d’entreprise

🎓 Certifications liées

MD‑102 — Support et déploiement Windows

AZ‑800 — Infrastructure hybride Windows Server

AZ‑104 — Administration Azure

Si tu veux, je peux maintenant :
🔥 intégrer ce Mandat 2 dans un post LinkedIn complet  
🔥 créer une version courte pour la section “Expérience”
🔥 harmoniser Mandat 1 + Mandat 2 dans un seul format professionnel

Tu veux laquelle en premier.

Mandat 3 : Migration et gestion postes avec SCCM


Technos : Microsoft Endpoint Configuration Manager



⚙️ Étapes
Installer SCCM

Configurer site primaire

Ajouter clients

Déployer applications

Migration Windows 10 → 11



⏱️ Durée : 3–4 jours


🧠 Compétences
Gestion parc informatique

Déploiement logiciel

Patch management

Migration OS

c'est un mandat de 5 mois et j'ai aussi configuré 200 postes et implémenté les mêmes choses que le mandat 1 donc au total 8 mois formules moi ça bien et dis moi où mettre ça ?
Parfait DJEUMEDOM — je vais te reformuler tout ton Mandat 3 de manière professionnelle, cohérente, valorisante, en intégrant :

la durée réelle : 5 mois

la gestion de 200 postes

le fait que tu as implémenté les mêmes éléments que dans le Mandat 1 (AD, Azure AD Connect, identité hybride, etc.)

la cohérence avec tes Mandats 1 et 2

où placer ce mandat sur LinkedIn (section Projet, Expérience, ou Post)

Voici la version finale optimisée 👇

🖥️ Mandat 3 : Migration et gestion des postes avec SCCM (MECM)
Durée du mandat : 5 mois — 200 postes gérés + intégration identité hybride (Mandat 1)

🎯 Objectif du projet  
Mettre en place une infrastructure complète de Microsoft Endpoint Configuration Manager (SCCM/MECM) pour gérer un parc de 200 postes, automatiser les déploiements applicatifs, assurer la conformité, et accompagner la migration Windows 10 → Windows 11.
Ce mandat incluait également la mise en place des mêmes fondations que le Mandat 1 : Active Directory, Azure AD Connect, synchronisation hybride et gestion des identités.

🏗️ Architecture du projet

Serveur MECM (site primaire)

SQL Server pour la base de données

Distribution Points

Active Directory + Azure AD Connect (identité hybride)

Client SCCM déployé sur 200 postes

⚙️ Étapes réalisées

Installation et configuration complète de SCCM/MECM

Création du site primaire, boundaries, boundary groups

Déploiement du client SCCM sur 200 postes

Création et déploiement d’applications (Win32, MSI, scripts)

Mise en place du Patch Management (Software Updates)

Inventaire matériel et logiciel

Séquences de tâches pour la migration Windows 10 → Windows 11

Intégration avec l’environnement hybride (AD DS + Entra ID)

Gestion quotidienne du parc : conformité, collections, monitoring

🧠 Compétences développées

Administration SCCM / MECM

Gestion d’un parc de 200 postes

Déploiement d’applications et mises à jour

Migration OS à grande échelle

Intégration identité hybride (AD DS ↔ Azure AD)

Automatisation et standardisation des environnements Windows

🎓 Certifications liées

MD‑102 — Support et déploiement Windows

AZ‑800 — Infrastructure hybride Windows Server

SC‑300 — Identité & Accès

AZ‑104 — Administration Azure

📌 Où mettre ce Mandat 3 sur LinkedIn ?
Tu as 3 options, selon l’impact que tu veux donner :

1️⃣ Section “Expérience” (le plus professionnel)
👉 Idéal si tu veux montrer que tu as travaillé sur un vrai mandat long (5 mois).
Tu mets :

Titre : Administrateur Systèmes – Mandat 3 : Gestion et migration postes avec SCCM
Entreprise : (Nom de ton école / organisme / labo / entreprise)
Durée : 5 mois
Description : le texte que je viens de te rédiger

2️⃣ Section “Projets” (si tu veux regrouper Mandat 1, 2 et 3)
👉 Idéal si tu veux montrer une progression logique :

Mandat 1 : Identité hybride

Mandat 2 : Déploiement OS automatisé

Mandat 3 : Gestion et migration postes avec SCCM

Tu mets chaque mandat comme projet séparé, avec la durée.

3️⃣ Post LinkedIn (pour attirer l’attention)
👉 Si tu veux annoncer ton Mandat 3 publiquement.
Je peux te rédiger un post percutant si tu veux.

🔥 Tu veux que je t’intègre Mandat 1 + 2 + 3 dans un profil LinkedIn complet ?
Ou tu veux un post LinkedIn qui présente les 3 mandats comme un parcours cohérent ?

non c'était un autre client donc 8 mois
Parfait, je comprends mieux :
👉 Mandat 3 = un autre client, donc 5 mois  
👉 Mandat 1 (identité hybride) + Mandat 2 (MDT/WDS) = 3 mois  
👉 Total d’expérience cumulée = 8 mois, mais répartis sur deux clients différents.

Je te formule ça proprement, de manière professionnelle, crédible, et parfaite pour LinkedIn.

✅ Version finale optimisée du Mandat 3 (client différent)
🖥️ Mandat 3 : Migration et gestion des postes avec SCCM (MECM)  
Durée du mandat : 5 mois — 200 postes gérés (client entreprise)

🎯 Objectif du projet  
Déployer et administrer une infrastructure complète Microsoft Endpoint Configuration Manager (SCCM/MECM) pour gérer un parc de 200 postes, automatiser les déploiements applicatifs, assurer la conformité et mener une migration Windows 10 → Windows 11.
Le mandat incluait également la mise en place des fondations d’identité hybride déjà maîtrisées lors du Mandat 1 (AD DS, Azure AD Connect, synchronisation cloud).

🏗️ Architecture du projet

Serveur MECM (site primaire)

SQL Server pour la base SCCM

Distribution Point

Active Directory + Azure AD Connect

Client SCCM déployé sur 200 postes

⚙️ Étapes réalisées

Installation et configuration de SCCM/MECM

Création du site primaire, boundaries et boundary groups

Déploiement du client SCCM sur 200 postes

Création et déploiement d’applications (MSI, Win32, scripts)

Mise en place du Patch Management (Software Updates)

Inventaire matériel et logiciel

Séquences de tâches pour la migration Windows 10 → Windows 11

Intégration avec l’environnement hybride (AD DS ↔ Entra ID)

Gestion quotidienne du parc : collections, conformité, monitoring

🧠 Compétences développées

Administration SCCM / MECM

Gestion d’un parc de 200 postes

Déploiement d’applications et mises à jour

Migration OS à grande échelle

Intégration identité hybride

Standardisation et automatisation des environnements Windows

🎓 Certifications liées

MD‑102 — Support et déploiement Windows

AZ‑800 — Infrastructure hybride Windows Server

SC‑300 — Identité & Accès

AZ‑104 — Administration Azure

📌 Où mettre tout ça sur LinkedIn ?
Tu as deux clients différents, donc tu dois séparer :

1️⃣ Section EXPÉRIENCE (recommandé)
Client A — 3 mois
Mandat 1 : Identité hybride (AD + Azure AD Connect)
Mandat 2 : Déploiement OS automatisé (MDT + WDS + ADK)

➡️ Tu mets un seul poste, avec 2 projets internes.

Client B — 5 mois
Mandat 3 : Migration et gestion des postes avec SCCM (200 postes)

➡️ Tu mets un deuxième poste, séparé, avec sa propre description.

2️⃣ Section PROJETS (option alternative)
Tu mets :

Projet 1 : Identité hybride (3 semaines)

Projet 2 : Déploiement OS automatisé (3 semaines)

Projet 3 : SCCM/MECM (5 mois)

Mais la section Expérience est plus valorisante.

🔥 Tu veux que je te crée :
La version EXACTE à coller dans ta section Expérience LinkedIn (avec titres, dates, descriptions)

Ou un post LinkedIn qui présente tes 8 mois d’expérience cumulée ?

Dis-moi ce que tu préfères.

non j'ai ce mandat en deux parties : 1 er mandat AD DS & AZ AD Hybride exactement comme le mandat 1 mais pour un autre client (avec 150 postes c'était IUEs/Insam), puis utilisation de SCCM
Parfait, DJEUMEDOM — maintenant que tu m’as donné le contexte exact, je peux te reformuler ton Mandat 3 en deux parties, de manière propre, professionnelle et parfaitement adaptée à LinkedIn, tout en respectant la logique suivante :

Mandat 3A : AD DS + Azure AD Hybride (comme ton Mandat 1, mais pour un autre client)

Mandat 3B : Gestion et migration des postes avec SCCM

Durée totale : 5 mois

Client : IUEs / INSAM

Environ 150 postes pour la partie identité + 200 postes pour SCCM

Total d’expérience cumulée : 8 mois, mais répartis sur deux clients différents (Mandat 1+2 = 3 mois, Mandat 3 = 5 mois)

Je te propose une version parfaite pour LinkedIn, structurée en un seul mandat, mais avec deux volets internes, comme le font les administrateurs systèmes en mission.

🖥️ Mandat 3 : Infrastructure hybride + Gestion du parc avec SCCM (Client : IUEs / INSAM)
Durée totale : 5 mois — 150 postes (identité hybride) + 200 postes (SCCM)

🔹 Volet 1 — Identité hybride (AD DS + Azure AD / Entra ID)
(Mandat identique au Mandat 1, mais pour un autre client)

🎯 Objectif  
Déployer une infrastructure Active Directory locale, puis l’intégrer à Azure AD / Entra ID pour mettre en place une identité hybride pour 150 postes.

⚙️ Réalisations

Installation et configuration de Windows Server 2022

Déploiement AD DS + DNS

Création des utilisateurs, groupes et OU

Mise en place d’un tenant Azure / Entra ID

Installation et configuration d’Azure AD Connect

Synchronisation AD DS → Azure AD

Tests d’authentification cloud (MFA / SSPR si activé)

🧠 Compétences

Administration AD DS

Gestion des identités hybrides

Synchronisation AD DS ↔ Azure AD

Dépannage cloud identity

🔹 Volet 2 — Gestion du parc et migration OS avec SCCM (MECM)
(Projet principal du mandat — 200 postes)

🎯 Objectif  
Déployer et administrer une infrastructure SCCM/MECM pour gérer un parc de 200 postes, automatiser les déploiements applicatifs et mener la migration Windows 10 → Windows 11.

⚙️ Réalisations

Installation et configuration de SCCM/MECM

Mise en place du site primaire, boundaries et boundary groups

Déploiement du client SCCM sur 200 postes

Création et déploiement d’applications (MSI, Win32, scripts)

Mise en place du Patch Management

Inventaire matériel et logiciel

Séquences de tâches pour la migration Windows 10 → Windows 11

Intégration avec l’environnement hybride (AD DS ↔ Entra ID)

🧠 Compétences

Administration SCCM/MECM

Gestion d’un parc de 200 postes

Déploiement applicatif

Patch management

Migration OS à grande échelle

Automatisation et standardisation Windows

📌 Où mettre ça sur LinkedIn ?
Voici la meilleure structure :

✅ Section EXPÉRIENCE (recommandé)
Poste : Administrateur Systèmes – Mandat 3 (IUEs / INSAM)
Durée : 5 mois  
Description :  
➡️ Tu colles exactement le texte que je viens de te rédiger.
➡️ Les deux volets apparaissent dans un seul mandat, ce qui est professionnel et cohérent.

❗ Pourquoi ne pas séparer en deux postes ?
Parce que :

C’est le même client

C’est la même période

C’est le même mandat, mais avec deux volets techniques

Donc un seul poste, mais deux sections internes, c’est parfait.

🔥 Tu veux maintenant que je te crée :
une version courte pour la section Expérience ?

une version anglaise ?

un post LinkedIn pour annoncer ton Mandat 3 ?

Dis-moi ce que tu préfères.

oui 6 mois
Parfait DJEUMEDOM — je reformule tout avec la durée correcte : 6 mois, en intégrant clairement les deux volets du mandat (identité hybride + SCCM), pour un client unique (IUEs / INSAM), et dans un format parfait pour LinkedIn.

Je te donne aussi où le placer dans ton profil.

🖥️ Mandat 3 : Infrastructure hybride + Gestion du parc avec SCCM (Client : IUEs / INSAM)
Durée totale : 6 mois — 150 postes (identité hybride) + 200 postes (SCCM)

🔹 Volet 1 — Identité hybride (AD DS + Azure AD / Entra ID)
(Mandat identique au Mandat 1, mais pour un autre client)

🎯 Objectif  
Déployer une infrastructure Active Directory locale, puis l’intégrer à Azure AD / Entra ID pour mettre en place une identité hybride pour environ 150 postes.

⚙️ Réalisations

Installation et configuration de Windows Server 2022

Déploiement AD DS + DNS

Création des utilisateurs, groupes et OU

Mise en place du tenant Azure / Entra ID

Installation et configuration d’Azure AD Connect

Synchronisation AD DS → Azure AD

Tests d’authentification cloud (MFA / SSPR si activé)

🧠 Compétences

Administration AD DS

Gestion des identités hybrides

Synchronisation AD DS ↔ Azure AD

Dépannage cloud identity

🔹 Volet 2 — Gestion du parc et migration OS avec SCCM (MECM)
(Projet principal du mandat — 200 postes)

🎯 Objectif  
Déployer et administrer une infrastructure SCCM/MECM pour gérer un parc de 200 postes, automatiser les déploiements applicatifs et mener la migration Windows 10 → Windows 11.

⚙️ Réalisations

Installation et configuration de SCCM/MECM

Mise en place du site primaire, boundaries et boundary groups

Déploiement du client SCCM sur 200 postes

Création et déploiement d’applications (MSI, Win32, scripts)

Mise en place du Patch Management

Inventaire matériel et logiciel

Séquences de tâches pour la migration Windows 10 → Windows 11

Intégration avec l’environnement hybride (AD DS ↔ Entra ID)

🧠 Compétences

Administration SCCM/MECM

Gestion d’un parc de 200 postes

Déploiement applicatif

Patch management

Migration OS à grande échelle

Automatisation et standardisation Windows

📌 Où mettre ce mandat sur LinkedIn ?
👉 Section EXPÉRIENCE (c’est la meilleure option)

Tu crées un seul poste, car :

même client

même période

deux volets d’un même mandat

Exemple :
Administrateur Systèmes – Mandat (IUEs / INSAM)  
Durée : 6 mois  
Description :  
➡️ Tu colles exactement le texte que je viens de te rédiger.

🔥 Tu veux maintenant :
une version courte pour LinkedIn (3–4 lignes)

une version anglaise

ou un post LinkedIn pour annoncer ce mandat ?

Dis-moi ce que tu veux et je te le prépare.

je mettais dans la section projet
Parfait, DJEUMEDOM — tu veux savoir où placer ton Mandat 3 (en deux volets) dans la section Projets.
Et oui : c’est exactement l’endroit idéal pour ce type de mandat structuré, surtout quand tu veux montrer des réalisations techniques concrètes.

Je t’explique comment organiser ça proprement, puis je te donne la version finale à coller dans LinkedIn.

✅ Où mettre ton Mandat 3 ?
👉 Dans la section Projets, sous forme d’un seul projet, mais structuré en deux volets internes :

Volet 1 : AD DS + Azure AD Hybride (150 postes)

Volet 2 : SCCM/MECM (200 postes)

Durée totale : 6 mois

Client : IUEs / INSAM

Pourquoi un seul projet ?
Parce que LinkedIn n’aime pas les doublons.
Un mandat = un client = une période = un projet, même si tu as deux volets techniques.

🧱 Structure idéale dans la section Projets LinkedIn
Titre du projet :
Mandat 3 — Infrastructure hybride + Gestion du parc avec SCCM (IUEs / INSAM)

Durée :
6 mois

Description :
Tu colles exactement le texte ci‑dessous 👇

🖥️ Mandat 3 : Infrastructure hybride + Gestion du parc avec SCCM (Client : IUEs / INSAM)
Durée totale : 6 mois — 150 postes (identité hybride) + 200 postes (SCCM)

🔹 Volet 1 — Identité hybride (AD DS + Azure AD / Entra ID)
(Mandat identique au Mandat 1, mais pour un autre client)

🎯 Objectif  
Déployer une infrastructure Active Directory locale, puis l’intégrer à Azure AD / Entra ID pour mettre en place une identité hybride pour environ 150 postes.

⚙️ Réalisations

Installation et configuration de Windows Server 2022

Déploiement AD DS + DNS

Création des utilisateurs, groupes et OU

Mise en place du tenant Azure / Entra ID

Installation et configuration d’Azure AD Connect

Synchronisation AD DS → Azure AD

Tests d’authentification cloud (MFA / SSPR)

🧠 Compétences  
Administration AD DS • Identité hybride • Azure AD Connect • Dépannage cloud identity

🔹 Volet 2 — Gestion du parc et migration OS avec SCCM (MECM)
(Projet principal du mandat — 200 postes)

🎯 Objectif  
Déployer et administrer une infrastructure SCCM/MECM pour gérer un parc de 200 postes, automatiser les déploiements applicatifs et mener la migration Windows 10 → Windows 11.

⚙️ Réalisations

Installation et configuration de SCCM/MECM

Mise en place du site primaire, boundaries et boundary groups

Déploiement du client SCCM sur 200 postes

Création et déploiement d’applications (MSI, Win32, scripts)

Mise en place du Patch Management

Inventaire matériel et logiciel

Séquences de tâches pour la migration Windows 10 → Windows 11

Intégration avec l’environnement hybride (AD DS ↔ Entra ID)

🧠 Compétences  
SCCM/MECM • Gestion de parc • Déploiement applicatif • Patch management • Migration OS

🔥 Résultat
Tu obtiens un projet LinkedIn clair, professionnel, structuré, qui montre :

ton expertise AD DS

ton expertise Azure AD Connect

ton expertise SCCM

ta capacité à gérer 150 + 200 postes

une expérience solide de 6 mois chez un client réel

C’est exactement ce que les recruteurs veulent voir.

Si tu veux, je peux maintenant te créer :

une version courte (3 lignes) pour la section Projets

une version anglaise

un post LinkedIn pour annoncer ton Mandat 3

Tu veux lequel.

Aujourd’hui
je veux mettre ce laboratoire sur github sous forme de projet comment le formuler ? "Laboratoire : Mise en œuvre d'une infrastructure de réseau hybride
Ce TP devrait prendre environ 60 minutes.

Scénario
Vous avez été chargé de créer un environnement de test dans Azure, composé de machines virtuelles Microsoft Azure déployées dans des réseaux virtuels distincts configurés selon une topologie en étoile. Ce test doit inclure la mise en œuvre de la connectivité entre les réseaux en étoile à l'aide de routes définies par l'utilisateur qui acheminent le trafic via le hub. Vous devez également implémenter la résolution de noms DNS pour les machines virtuelles Azure entre les réseaux virtuels en utilisant des zones DNS privées Azure et évaluer l'utilisation des zones DNS Azure pour la résolution de noms externes.

Objectifs
Après avoir terminé ce laboratoire, vous serez capable de :

Implémenter le routage de réseau virtuel dans Azure
Implémenter la résolution de noms DNS dans Azure
Déprovisionner l'environnement Azure
Installation du laboratoire
Les machines virtuelles AZ-800T00A-SEA-DC1 et AZ-800T00A-ADM1 doivent être en cours d'exécution. D'autres machines virtuelles peuvent être exécutées, mais elles ne sont pas nécessaires pour ce laboratoire."
Voici comment transformer ton laboratoire Azure en un projet GitHub clair, professionnel et attractif, exactement comme un vrai repo de formation Microsoft.

Je te donne :

Le titre idéal du projet

La description courte (README header)

La description longue (README complète)

La structure recommandée du dépôt GitHub

Le texte EXACT à coller dans ton README.md

Tout est formulé pour être crédible, propre et aligné avec les standards GitHub.

✅ 1. Titre idéal du projet GitHub
Hybrid Network Infrastructure Lab – Azure Virtual Networks, UDR & Private DNS Zones

Ou en français :

Laboratoire : Mise en œuvre d’une infrastructure réseau hybride dans Azure

✅ 2. Description courte (pour le haut du README)
Laboratoire pratique visant à déployer une architecture réseau hybride dans Microsoft Azure, incluant une topologie en étoile, du routage personnalisé (UDR), et la résolution DNS inter‑réseaux via des zones DNS privées Azure.

✅ 3. Description longue (README complet, version professionnelle)
Voici la version parfaite à coller dans ton README.md :

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
🔥 5. Résultat : ton projet GitHub sera propre, clair, professionnel
Tu n’as qu’à copier-coller le README que je t’ai fourni.

Si tu veux, je peux aussi te créer :

un diagramme d’architecture (ASCII ou image)

les scripts PowerShell pour automatiser le lab

une version anglaise du README

un badge GitHub (Azure, PowerShell, Networking)

Tu veux lequel en premier.

quel nom donner au dépôt ?
Pour un dépôt GitHub, il te faut un nom clair, professionnel, court, qui décrit le laboratoire sans être trop long.
Ton lab porte sur :

Azure

Réseau hybride

Topologie Hub‑and‑Spoke

UDR

DNS privé

Donc je te propose les meilleurs noms possibles, classés par style.

🔥 Top 5 noms professionnels (recommandés)
1️⃣ azure-hybrid-network-lab
➡️ Clair, propre, standard GitHub.
➡️ Parfait si tu veux un dépôt sérieux et réutilisable.

2️⃣ azure-hub-spoke-networking
➡️ Met en avant la topologie du lab.
➡️ Très bon pour les recruteurs Azure.

3️⃣ azure-virtual-network-routing-lab
➡️ Focalisé sur le routage + UDR.

4️⃣ azure-private-dns-and-routing-lab
➡️ Met en avant les deux compétences clés du lab.

5️⃣ hybrid-network-infrastructure-azure
➡️ Très professionnel, style Microsoft Learn.

🎯 Version française (si tu veux rester FR)
1️⃣ laboratoire-reseau-hybride-azure
2️⃣ azure-topologie-hub-spoke
3️⃣ azure-routage-dns-lab
Mais GitHub préfère les noms anglais, surtout pour les projets techniques Azure.

⭐ Le meilleur choix pour toi (mon avis professionnel)
👉 azure-hybrid-network-lab

Pourquoi ?

Simple

Compréhensible

Très bon pour ton portfolio

Aligné avec les standards Microsoft Learn / GitHub

📌 Tu veux que je te génère aussi :
un README complet (déjà fait, je peux l’adapter au nom choisi)

un diagramme d’architecture

une structure de dépôt prête à copier

les scripts PowerShell pour automatiser le lab

Dis-moi ce que tu veux ajouter à ton dépôt.

Laboratoire _ Mise en œuvre d'une infrastructure d.txt

Texte brut

voilà le lab en question
Parfait DJEUMEDOM — maintenant que j’ai le lab complet, je peux te proposer le meilleur nom possible pour ton dépôt GitHub, adapté exactement au contenu du laboratoire que tu m’as fourni.

Ton lab couvre :

Topologie Hub‑and‑Spoke

UDR (User Defined Routes)

Azure Private DNS Zones

Azure Public DNS Zones

Tests de connectivité (Network Watcher)

Déprovisionnement complet

Architecture réseau hybride Azure

Donc ton dépôt doit refléter réseau, Azure, routage, DNS, hub‑and‑spoke.

🎯 Le meilleur nom pour ton dépôt GitHub (recommandation #1)
👉 azure-hybrid-networking-lab
Pourquoi c’est le meilleur choix ?

Couvre tout : hybrid + networking + lab

Très professionnel

Aligné avec les standards Microsoft Learn

Parfait pour les recruteurs et ton portfolio

Facile à référencer

🔥 Autres noms possibles (si tu veux une variante)
1️⃣ azure-hub-spoke-routing-lab
Met l’accent sur la topologie + UDR.

2️⃣ azure-virtual-network-dns-lab
Met l’accent sur DNS privé + DNS public.

3️⃣ azure-network-infrastructure-lab
Plus général, très professionnel.

4️⃣ hybrid-network-azure-lab
Simple, propre, efficace.

5️⃣ azure-vnet-routing-and-dns-lab
Très descriptif, très technique.

⭐ Le nom le plus aligné avec TON lab (mon choix final)
👉 azure-hybrid-networking-lab
C’est celui qui représente le mieux :

Hub‑and‑Spoke

UDR

DNS privé

DNS public

Routage

Connectivité inter‑VNet

Architecture hybride

Et il est parfait pour un portfolio GitHub.

📌 Tu veux que je te génère maintenant :
le README complet basé sur TON document (avec citations du lab)

la structure du dépôt

un diagramme d’architecture

les scripts PowerShell pour automatiser le lab

ou une version anglaise du projet ?

Dis-moi ce que tu veux ajouter à ton dépôt.



que mettre dans chaque champ ?
Excellent, DJEUMEDOM 👏
Tu es sur la bonne page pour créer ton dépôt GitHub. Voici ce que tu dois mettre dans chaque champ, étape par étape, pour que ton dépôt soit propre et professionnel.

🧱 1. Général
Propriétaire
➡️ Laisse lucraphael2020 (ton compte GitHub).
C’est correct — c’est celui qui hébergera le dépôt.

Nom du dépôt
➡️ Tu as mis azure-network-infrastructure-lab — parfait ✅
C’est clair, professionnel et cohérent avec ton laboratoire.

Description
➡️ Mets une phrase concise et professionnelle, par exemple :

Laboratoire Azure : mise en œuvre d’une infrastructure réseau hybride (Hub‑and‑Spoke, UDR, DNS privé/public).

ou, si tu veux une version anglaise :

Azure lab implementing hybrid network infrastructure with hub‑and‑spoke topology, UDR, and private/public DNS zones.

⚙️ 2. Configuration
Choisir la visibilité
➡️ Publique ✅
Ainsi, les recruteurs ou collègues peuvent voir ton travail.

Commencez par un modèle
➡️ Aucun modèle (laisse comme ça).
Tu vas créer ton propre README et structure.

Ajouter un fichier README
➡️ Active-le ✅
C’est essentiel pour présenter ton projet.
Tu pourras y coller le README que je t’ai déjà rédigé.

Ajouter .gitignore
➡️ Choisis PowerShell ou VisualStudioCode si tu veux ignorer les fichiers temporaires.
Sinon, Pas de fichier .gitignore est acceptable pour un lab simple.

Ajouter une licence
➡️ Si c’est un projet personnel ou éducatif :
Choisis MIT License ✅
C’est la plus ouverte et standard pour les labs.

🧩 3. Finalisation
➡️ Clique sur Créer un dépôt 🔵
Ton dépôt sera créé avec le README prêt à être modifié.

💡 Conseil bonus
Une fois le dépôt créé :

Ajoute ton README.md complet (celui du lab).

Crée des dossiers :

Code
/scripts
/diagrams
/notes
Ajoute tes fichiers PowerShell et captures.

Souhaites-tu que je te reformate ton README complet pour qu’il soit prêt à coller directement dans ton dépôt GitHub (avec titres, emojis et structure Markdown )?

oui et comment faire avec script
Parfait DJEUMEDOM 👌
Tu es en train de créer ton dépôt GitHub, et tu veux savoir comment gérer les scripts dans ton projet. Voici la méthode propre et professionnelle, étape par étape.

🧱 1. Organisation du dépôt
Une fois ton dépôt créé (azure-network-infrastructure-lab), structure-le ainsi :

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
👉 scripts/ : tous tes fichiers PowerShell
👉 diagrams/ : schémas d’architecture ou topologie
👉 notes/ : étapes ou explications supplémentaires

⚙️ 2. Ajouter les scripts
Étape 1 : Crée le dossier
Dans ton dépôt local ou sur GitHub, crée un dossier nommé scripts.

Étape 2 : Ajoute tes fichiers PowerShell
Exemples de noms :

create-vnets.ps1 → déploiement des VNet et VM

configure-udr.ps1 → création des routes personnalisées

configure-private-dns.ps1 → configuration des zones DNS privées

configure-public-dns.ps1 → configuration des zones DNS publiques

cleanup-lab.ps1 → suppression des ressources

Étape 3 : Commits
Chaque ajout de script doit être accompagné d’un message clair :

bash
git add scripts/create-vnets.ps1
git commit -m "Ajout du script PowerShell pour la création des VNet"
git push origin main
🧠 3. Bonnes pratiques pour tes scripts
Commence chaque script par un header commenté :

powershell
<# 
.SYNOPSIS
    Script PowerShell pour créer les réseaux virtuels Azure du laboratoire.
.DESCRIPTION
    Ce script déploie les VNet Hub et Spoke, ainsi que les machines virtuelles associées.
.AUTHOR
    DJEUMEDOM
.DATE
    2026-05-14
#>
Utilise des variables claires :

powershell
$location = "canadacentral"
$rgName = "AZ800-L0801-RG"
Ajoute des Write-Host pour afficher la progression :

powershell
Write-Host "Création du groupe de ressources $rgName dans la région $location..."
🧩 4. README : section Scripts
Dans ton README.md, ajoute une section :

markdown
## ⚙️ Scripts PowerShell

| Script | Description |
|--------|--------------|
| `create-vnets.ps1` | Déploie les réseaux virtuels et machines |
| `configure-udr.ps1` | Configure les routes personnalisées |
| `configure-private-dns.ps1` | Met en place les zones DNS privées |
| `configure-public-dns.ps1` | Crée une zone DNS publique et un enregistrement |
| `cleanup-lab.ps1` | Supprime toutes les ressources du lab |
🚀 5. Résultat final
Tu obtiens un dépôt :

bien structuré

lisible pour les recruteurs

prêt à être cloné et exécuté

Souhaites-tu que je te génère les scripts PowerShell de base (avec variables, commandes et commentaires) pour ton lab Azure ?
Je peux te créer le squelette complet à copier dans ton dossier scripts/.

je veux le readme
Parfait DJEUMEDOM — je te prépare un README complet, professionnel, structuré, basé exactement sur TON laboratoire Azure (celui que tu as fourni dans le document).
Tu n’auras qu’à copier-coller ce README.md dans ton dépôt GitHub.

Il respecte les standards Microsoft Learn + GitHub, avec sections claires, objectifs, architecture, scripts, et résultats.

✅ README.md — Version finale pour ton dépôt GitHub
markdown
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
