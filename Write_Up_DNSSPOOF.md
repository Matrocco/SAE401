# Write Up— Tentative d’attaque DNS Spoofing avec dsniff

## Objectif

L’objectif de cette manipulation était de réaliser une attaque de **DNS spoofing** (empoisonnement DNS) sur une machine victime présente dans le réseau local, en utilisant les outils du paquet **dsniff** :

* `arpspoof` : mise en place d’une attaque de type Man-in-the-Middle (MITM)
* `dnsspoof` : injection de réponses DNS falsifiées

Le but était de faire résoudre le domaine **entreprise.lan** vers l’adresse IP **172.16.40.10**, correspondant à la machine attaquante.



## Topologie du réseau

| Équipement                  | Adresse IP   |
| --------------------------- | ------------ |
| Machine attaquante (Kali)   | 172.16.40.10 |
| Serveur DNS légitime (BIND) | 172.16.40.1  |
| Machine victime             | 172.16.40.12 |



## Méthode mise en œuvre

### 1. Configuration du fichier de spoof DNS

Un fichier de correspondance a été créé afin de forcer `dnsspoof` à répondre avec l’adresse IP de l’attaquant :

```bash id="a1021"
echo "172.16.40.10 entreprise.lan" | sudo tee /etc/hosts_spoof
```



### 2. Activation du routage IP

Le routage IP a été activé sur la machine attaquante afin de permettre le relais des paquets après la mise en place du MITM :

```bash id="a1022"
sudo sysctl -w net.ipv4.ip_forward=1
```



### 3. Mise en place de l’attaque MITM par ARP spoofing

L’attaquant s’est positionné entre la victime et le serveur DNS légitime.

Dans un premier terminal :

```bash id="a1023"
sudo arpspoof -i eth0 -t 172.16.40.12 172.16.40.1
```

Dans un second terminal :

```bash id="a1024"
sudo arpspoof -i eth0 -t 172.16.40.1 172.16.40.12
```

Cette étape permet de rediriger le trafic entre la victime et le serveur DNS vers la machine attaquante.

---

### 4. Lancement de dnsspoof

L’outil `dnsspoof` a ensuite été démarré pour injecter de fausses réponses DNS :

```bash id="a1025"
sudo dnsspoof -i eth0 -f /etc/hosts_spoof
```



### 5. Vérification sur la machine victime

La vérification a été effectuée à l’aide de la commande suivante :

```bash id="a1026"
dig entreprise.lan
```

Résultats observés :

* la victime reçoit toujours la réponse du DNS légitime ;
* ou bien la requête retourne `SERVFAIL` ;
* l’adresse IP falsifiée n’est jamais obtenue.



## Analyse de l’échec de l’attaque

### Présence de DNSSEC sur le serveur DNS

Le serveur DNS légitime utilise **DNSSEC**, mécanisme de sécurisation du DNS permettant de garantir l’authenticité et l’intégrité des réponses.

DNSSEC repose notamment sur :

* des signatures cryptographiques (`RRSIG`) ;
* des clés publiques (`DNSKEY`) ;
* une chaîne de confiance entre les zones DNS.

Dans ce contexte, les réponses falsifiées générées par `dnsspoof` ne contiennent aucune signature valide.

Par conséquent :

* elles sont rejetées par le résolveur de la victime ;
* la réponse légitime est conservée ;
* ou une erreur `SERVFAIL` est renvoyée en cas d’échec de validation.

Les outils tels que `dnsspoof`, `ettercap` ou `bettercap` ne sont pas capables de générer des signatures DNSSEC valides, car celles-ci nécessitent l’accès aux clés privées du serveur DNS légitime.



## Conclusion

La tentative de DNS spoofing échoue de manière logique en raison de la présence de **DNSSEC** sur le serveur DNS cible.

Les raisons principales sont les suivantes :

* le serveur DNS signe ses réponses ;
* l’attaquant ne possède pas les clés privées nécessaires ;
* les réponses falsifiées ne peuvent pas être validées ;
* le client rejette toute réponse non signée ou invalide.

En conséquence, l’attaque est techniquement impossible tant que DNSSEC est activé et correctement vérifié par la machine victime.



## Contremesures et pistes théoriques

Dans un cadre d’étude, plusieurs pistes théoriques peuvent être évoquées :

* usurpation DHCP afin d’imposer un autre serveur DNS ;
* tentative de désactivation de la validation DNSSEC côté client ;
* compromission du poste client ;
* redirection du trafic via un autre type de MITM.

Ces scénarios restent toutefois dépendants du niveau de sécurisation de l’infrastructure.
