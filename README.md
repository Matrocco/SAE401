# Le DNS-SEC

# Problème :

**le Cache Poisoning**

Des personnes se faisant  passer pour des domaines connus, il permet donc de rediriger un client vers un site frauduleux

### exemple :

Un hacker a décelé une faille dans le serveur DNS. Il parvient à 
s'introduire dans le serveur et à modifier l'adresse correspondant à 
www.ovhcloud.com par l'IP d'un serveur secondaire lui appartenant : 
203.0.113.78.

![Un hacker_décele_une_faille_dans_le_serveur_DNS](https://www.ovhcloud.com/sites/default/files/styles/large_screens_1x/public/2021-07/Webcloud_1624_3.png)

Lorsque l'utilisateur entre l'adresse www.ovhcloud.com, son navigateur se 
dirige vers le serveur DNS pour récupérer l'adresse IP correspondante. 
Le DNS infecté renvoie l'adresse introduite par le hacker : 
203.0.113.78.

![DNS_infecté](https://www.ovhcloud.com/sites/default/files/styles/large_screens_1x/public/2021-07/Webcloud_1624_4.png)

Le navigateur utilise cette adresse IP pour obtenir le contenu du site. Le
 serveur pirate lui renvoie une page ressemblant à www.ovhcloud.com pour
 obtenir ses données personnelles (phishing).

![Page_piratée](https://www.ovhcloud.com/sites/default/files/styles/large_screens_1x/public/2021-07/Webcloud_1624_5.png)

Source : https://www.ovhcloud.com/fr/domains/dnssec/

# Solution :

ajoute une clé publique et une clé privée pour justifier l’authenticité du serveur DNS

Comme HTTPS, DNSSEC ajoute une couche de sécurité avec l'activation de réponses authentifiées au-dessus d'un protocole par ailleurs non sécurisé. Alors que HTTPS chiffre le trafic pour que personne en ligne ne puisse espionner vos activités sur Internet, DNSSEC se contente de signer les réponses pour que les falsifications soient détectables. DNSSEC apporte une solution à un problème réel sans qu'il soit nécessaire d'incorporer un chiffrement. 

Source :https://www.cloudflare.com/fr-fr/learning/dns/dnssec/how-dnssec-works/

# Explication :

DNSSEC crée un système de noms de domaine sécurisé en ajoutant des signatures cryptographiques aux enregistrements DNS existants.

Ces signatures numériques sont stockées dans des serveurs de noms DNS parallèlement à des types d'enregistrement courants comme A, AAAA, MX, CNAME, etc. En vérifiant la signature qui lui est associée, vous pouvez vérifier qu'un enregistrement DNS demandé provient de son serveur de noms faisant autorité et qu'il n'a pas été modifié en chemin, contrairement à un faux enregistrement injecté dans une attaque de l'homme du milieu( man in the middle).

Pour faciliter la validation des signatures, DNSSEC ajoute quelques nouveaux types d'enregistrements DNS :

- **RRSIG** : contient une signature cryptographique
- **DNSKEY** : contient une clé de signature publique
- **DS** : contient le hachage d'un enregistrement DNSKEY
- **NSEC** et **NSEC3** : pour le déni d'existence explicite d'un enregistrement DNS
- **CDNSKEY** et **CDS** : pour une zone fille demandant des mises à jour d'un ou de plusieurs enregistrements DS dans la zone parent.

Source https://www.cloudflare.com/fr-fr/learning/dns/dnssec/how-dnssec-works/

# Nouveaux problèmes :

DNSSEC introduit aussi ses propres problèmes, par exemple, le fait qu'un enregistrement spécial (NSEC, utilisé pour prouver la non-existence d'un enregistrement) indique le prochain domaine de la zone permettant d'énumérer le contenu complet d'une zone signée, même si le transfert de
 zone n'est pas permis. Ce problème fait que la plupart des [TLD](https://fr.wikipedia.org/wiki/Domaine_de_premier_niveau) utilisent l'enregistrement NSEC3, qui n'a pas ce défaut.
https://fr.wikipedia.org/wiki/Domain_Name_System_Security_Extensions

# Détaille :

Les ajouts au protocole DNS représentant DNSSEC ont été normalisés dans la RFC 2535[[2]](https://fr.wikipedia.org/wiki/Domain_Name_System_Security_Extensions#cite_note-RFC-2535-2) en mars 1999, puis mis à jour, rendant celle-ci obsolète, par les RFC 4033[[1]](https://fr.wikipedia.org/wiki/Domain_Name_System_Security_Extensions#cite_note-RFC-4033-1), RFC 4034[[3]](https://fr.wikipedia.org/wiki/Domain_Name_System_Security_Extensions#cite_note-RFC-4034-3), et RFC 4035[[4]](https://fr.wikipedia.org/wiki/Domain_Name_System_Security_Extensions#cite_note-RFC-4035-4).

```
  This document and its two companions obsolete [RFC2535], [RFC3008],
   [RFC3090], [RFC3445], [RFC3655], [RFC3658], [RFC3755], [RFC3757], and
   [RFC3845].  This document set also updates but does not obsolete
   [RFC1034], [RFC1035], [RFC2136], [RFC2181], [RFC2308], [RFC3225],
   [RFC3007], [RFC3597], and the portions of [RFC3226] that deal with
   DNSSEC.
```

https://datatracker.ietf.org/doc/html/rfc4033

# Inconveniant :

la connexion DNS n’est pas chiffrée pour le fait, il faut utiliser 

- **DoT (DNS over TLS)** : Les données DNS voyagent dans un tunnel sécurisé (le même genre que le cadenas vert HTTPS des sites web).
- **DoH (DNS over HTTPS)** : Le DNS est caché à l'intérieur du trafic web classique.

### Se renseignée a :

https://www.cloudflare.com/fr-fr/learning/dns/dns-over-tls/

information importante, mais sortant du cadre DNSSEC demander