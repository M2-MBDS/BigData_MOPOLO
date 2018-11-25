# Chapitre 4
## Exercice 2 : CLI-A & D

``` $ java -Xmx256m -Xms256m -jar $KVHOME/lib/kvstore.jar kvlite ```

### 1) Accéder à CLI-A & D

```
$ java -jar $KVHOME/lib/kvstore.jar runadmin -port 5000 -host bigdatalite.localdomain
```
Connection a KVSTORE
```
kv-> connect store -name kvstore
```

### 2) Identifier la liste de toutes les commandes

```
kv-> help
```
ou
```
kv-> ?
```

### 3) Identifier les différents composants avec les commandes

```
kv-> ? show
```

### 4) Créer un Fichier de nom schema_UserInfo.avsc avec le contenant suivant et le charger dans KVSTORE

```json
{
    "type": "record",
    "name": "ContactInfo",
    "namespace": "example",
    "fields": [
        {
            "name": "phone",
            "type": "string",
            "default": ""
        },
        {
            "name": "email",
            "type": "string",
            "default": ""
        },
        {
            "name": "city",
            "type": "string",
            "default": ""
        }
    ]
}
```
```
kv-> ddl add-schema -file /home/oracle/Bureau/tpMOPOLO/schema_UserInfo.avsc
```
Vérifier la présence de ce schema.
```
kv-> show schemas;
```
Afficher sa structure du schema
```
kv-> show schemas -name example.ContactInfo;
```
### 5) Ajouter les données suivantes sans schéma
```
-----> Clé : /Smith/Bob/-/phonenumber
-----> Valeur : "408 555 5555"

-----> Clé : /contact/Adam/Eve
-----> Valeur : "{\"phone\":\"888-431-9361\", \"email\":\« eadam@jas.com\",\"city\":\"N ice\"}"
```
```
kv-> put kv -key /Smith/Bob/-/phonenumber -value "408 555 5555";

kv-> put kv -key /contact/Adam/Eve -value "{\"phone\":\"888-431-9361\", \"email\":\« eadam@jas.com\",\"city\":\"N ice\"}";
```

### 6) Ajouter l’objet avec schéma (example.ContactInfo)
```
-----> Clé : /contact/Evan/Houston
-----> Valeur : "{\"phone\":\"028-781- 1457\",\"email\":\"evan@texfoundation.or g\",\"city\":\"Geest-G\"}"
```
```
kv-> put kv -key /contact/Evan/Houston -value "{\"phone\":\"028-781- 1457\",\"email\":\"evan@texfoundation.or g\",\"city\":\"Geest-G\"}" -json example.ContactInfo;
```

### 7) Créer un fichier contact.txt avec les données suivantes et les charger dans KVSTORE
```sql
### Begin Script ###
put kv -key /contact/Bob/Walker -value "{\"phone\":\"857-431-9361\", \"email\":\"Nunc@Quisque.com\",\"city\":\"Turriff\"}" -json example.ContactInfo
put kv -key /contact/Craig/Cohen -value "{\"phone\":\"657-486-0535\", \"email\":\"sagittis@metalcorp.net\",\"city\":\"Hamoir\"}" -json example.ContactInfo
put kv -key /contact/Lacey/Benjamin -value "{\"phone\":\"556-975-3364\", \"email\":\"Duis@laceyassociates.ca\",\"city\":\"Wasseiges\"}" -json example.ContactInfo
put kv -key /contact/Preston/Church -value "{\"phone\":\"436-396-9213\", \"email\":\"preston@mauris.ca\",\"city\":\"Helmsdale\"}" -json example.ContactInfo
exit
### End Script ###
```

Utilisez la commande load pour cela.
```
kv-> load -file /home/oracle/Bureau/tpMOPOLO/contact.txt
```

Charger un objet connaissance sa clé
```
kv-> get kv -key /contact/Bob/Walker
```

Afficher l’ensemble des objets contenus dans le KVSTORE

/!\ Attentien le resultat est bizarre et different de celui du cours 
```
kv-> get kv -all
```
Afficher des objets connaissant une partie de la clé, par exemple /contact
```

```
Supprimer un objet de votre choix. Vérifier qu’il est bien supprimé
```
kv-> delete kv -key /Smith/Bob/-/phonenumber
```





<!-- 
1 connect store -name kvstore
2 ddl add-schema -file /home/oracle/Developper/schema_UserInfo.avsc
3 help
4 show
5 ;
6 show schemas;
7 show schema_UserInfo.avsc
8 show -schemas schema_UserInfo.avsc
9 show
10 ;
11 help
12 show admin
13 show admin;
14 show schemas
15 show verify
16 execute 'drop table CRITERES';
17 execute 'drop table CLIENT';
18 execute 'drop table APPRECIATION';
19 execute  'CREATE TABLE CRITERES (
20 CRITEREID INTEGER, 
21 TITRE STRING,
22 DESCRIPTION STRING,
23 PRIMARY KEY(CRITEREID)
24 )';
25 drop table CRITERES;
26 drop table CRITERES
27 connect store -name kvstore
28 execute 'drop table CRITERES';
29 execute 'drop table CLIENT';
30 execute 'drop table APPRECIATION';
31 execute 'CREATE TABLE CRITERES (
32 CRITEREID INTEGER,
33 TITRE STRING,
34 DESCRIPTION STRING,
35 PRIMARY KEY(CRITEREID)
36 )';
37 execute 'create table CLIENT (
38 ClIENTID INTEGER, 
39 NOM STRING, 
40 PRENOM STRING, 
41 CODEPOSTAL STRING,
42 VILLE STRING,
43 ADRESSE  STRING,
44 TELEPHONE STRING,
45 ANNEENAISS STRING,
46 primary key(shard(CLIENTID)))';
47 execute 'create table APPRECIATION(
48 VOLID INTEGER, 
49 CRITEREID INTEGER, 
50 CLIENTID INTEGER, 
51 DATEVOL  STRING, 
52 NOTE ENUM (EXCELLENT,TRES_BIEN, BIEN, 
53 MOYEN, PASSABLE, MEDIOCRE
54 ),
55 primary key (shard(VOLID, CRITEREID, CLIENTID, DATEVOL))
56 )';
57 show table CRITERES;
58 show tables -name CRITERES;
59 put table -name criteres -json
60 '{"CRITEREID":1, 
61 "TITRE":"Qualité du site reservation",
62 "DESCRIPTION": "Evaluer la qualité du site de réservation"
63 }'
64 ;
65 show tables -name CRITERES;
66 put table -name criteres -json
67 '{"CRITEREID":2, 
68 "TITRE":"Prix",
69 "DESCRIPTION": "Evaluer le prix de la réservation"
70 }';
71 put table -name criteres -json
72 '{"CRITEREID":4, 
73 "TITRE":"Qualité du siége",
74 "DESCRIPTION": "Evaluer la qualité du siége"
75 }';
76 put table -name criteres -json
77 '{"CRITEREID":5, 
78 "TITRE":"Qualité du siége",
79 "DESCRIPTION": "Evaluer la qualité du siége"
80 }';
81 put table -name criteres -json
82 '{"CRITEREID":6, 
83 "TITRE":"Accueil au guichet",
84 "DESCRIPTION": "Evaluer la qualité accueil au guichet"
85 }';
86 put table -name criteres -json
87 '{"CRITEREID":7, 
88 "TITRE":"Accueil é bord",
89 "DESCRIPTION": "Evaluer la qualité accueil é bord"
90 }';
91 exit
92 connect store -name kvstore
93 connect store -name kvstore
94 help
95 ?
96 ? show
97 ? topology
 -->