# SAE4.Cyber.01 - Sécuriser un système d’information 

Ce dépôt contient les livrables de la SAE 4.Cyber.01 du BUT R&T (Semestre 4). L'objectif est de concevoir, maquetter et sécuriser l'architecture réseau d'une entreprise répartie sur deux sites distants.

## Contexte du projet

L'entreprise dispose de deux sites géographiques. Chaque site possède une architecture LAN segmentée. L'interconnexion entre les sites est assurée par un tunnel sécurisé traversant un réseau public (simulation Internet).

**Outil utilisé :** Cisco Packet Tracer

##  Architecture Réseau

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
| **[Nom de l'étudiant 1]** | **Sécurisation DNS** | Mise en place de DNSSEC pour garantir l'authenticité des réponses DNS. |
| **[Nom de l'étudiant 2]** | **Sécurisation WEB** | Durcissement des serveurs Web (HTTPS, configurations sécurisées). |
| **[Nom de l'étudiant 3]** | **Tests de sécurité** | Scénarios d'attaques et vérification de la robustesse (Pentesting). |
| **[Nom de l'étudiant 4]** | **Recommandations ANSSI** | Audit de la maquette via la checklist officielle de l'ANSSI. |

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
* **Adressage IP :** [Préciser si vous utilisez un plan d'adressage spécifique, ex: 192.168.x.x]

---
*Projet réalisé dans le cadre du BUT Réseaux & Télécoms - IUT de Béthune.*