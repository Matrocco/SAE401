# SAE4.Cyber.01 - Sécuriser un système d’information 

Ce dépôt contient les livrables de la SAE 4.Cyber.01 du BUT R&T (Semestre 4). L'objectif est de concevoir, maquetter et sécuriser l'architecture réseau d'une entreprise répartie sur deux sites distants.

## Contexte du projet

L'entreprise dispose de deux sites géographiques. Chaque site possède une architecture LAN segmentée. L'interconnexion entre les sites est assurée par un tunnel sécurisé traversant un réseau public (simulation Internet).

**Outil utilisé :** Cisco Packet Tracer

##  Architecture Réseau
<img width="582" height="485" alt="image" src="https://github.com/user-attachments/assets/af80f12d-6bd7-4059-8085-325bf3351288" />

### Topologie
Le réseau est composé de deux sites connectés via un **Tunnel IPSEC GRE**.

Chaque site est divisé en 3 zones (VLANs) :
1.  **Service**
2.  **Production**
3.  **Admin**

### Politique de Sécurité (ACL)
Les règles de filtrage suivantes ont été implémentées :
*  **Réseau Admin :** Accès total à tous les réseaux (locaux et distants).
*  **Réseaux Service & Production :** Isolés. Ils ne peuvent accéder à aucun autre réseau (ni localement, ni sur le site distant).

## Équipe et Spécialisations

Ce projet a été réalisé par un quadrinôme. Chaque membre s'est spécialisé sur un aspect critique de la sécurité :

| Membre de l'équipe | Spécialisation | Description succincte |
| :--- | :--- | :--- |
| **Mathéo Crépieux** | **Sécurisation DNS** | Mise en place de DNSSEC pour garantir l'authenticité des réponses DNS. |
| **Baptiste Allart** | **Sécurisation WEB** | Durcissement des serveurs Web (HTTPS, configurations sécurisées). |
| **Thomas Dubourdieu, Arthur Parmentier** | **Tests de sécurité** | Scénarios d'attaques et vérification de la robustesse (Pentesting). |
| **Alexis Stingre** | **Recommandations ANSSI** | Audit de la maquette via la checklist officielle de l'ANSSI. |

##  Contenu du dépôt

* `/Maquette` : Fichier `.pkt` (Packet Tracer) final.
* `/WriteUp` : Documentation technique détaillée.
    * *Note : Conformément aux consignes, le Write-Up contient les configurations en mode texte (pas de captures d'écran).*
* `/ANSSI` : Fichier Excel de suivi des recommandations ANSSI (Retenues / Exclues / Non applicables).

##  Installation et Utilisation

1.  Cloner ce dépôt :
    ```bash
    git clone [https://github.com/Matrocco/SAE401.git](https://github.com/Matrocco/SAE401.git)
    ```
2.  Ouvrir le fichier `.pkt` situé dans le dossier `/Maquette` avec **Cisco Packet Tracer**.
3.  Pour tester la connectivité et les restrictions ACL, référez-vous à la section "Tests" du Write-Up.

##  Détails Techniques (Extrait)

* **Protocole de Tunneling :** GRE (Generic Routing Encapsulation)
* **Sécurisation du Tunnel :** IPSEC (Détails des algos de chiffrement/hachage dans le WU)
* **Adressage IP :**

| **Réseau** | **Plage IP / Masque** | **Passerelle R1** | **Passerelle R2** |
| --- | --- | --- | --- |
| **VLAN 10** | `172.16.10.0/24` | 172.16.10.254 | 172.16.10.253 |
| **VLAN 20** | `172.16.20.0/24` | 172.16.20.254 | 172.16.20.253 |
| **VLAN 30** | `172.16.30.0/24` | 172.16.30.254 | 172.16.30.253 |
| **Réseau Inter-R1** | `172.16.0.0/16` | 172.16.255.1 | — |
| **Réseau Inter-R2** | `172.16.0.0/24` | 172.16.255.2 | — |
| **Tunnel GRE** | `192.168.1.0/30` | 192.168.1.1 | 192.168.1.2 |
| **Internet** | `0.0.0.0/0` | *DHCP/Public* | *DHCP/Public* |

---
*Projet réalisé dans le cadre du BUT Réseaux & Télécoms - IUT de Béthune.*
