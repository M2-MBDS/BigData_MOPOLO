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




# Chapitre 5
* JKV.1 : conception d’un record
* JKV.2 : Test de la classe existante HelloBigDataWorld.java
* JKV.3 : Coder votre propre classe MobileClients.java

## Exercice JKV.1 : conception d’un record

<!-- -- Un exemple de Structure des enregistrements (majorKey-MinorKeys)
/Nom/Prenom/Email/-minorKeys
Exemple :
/Nom/Prenom/Email/-
/Nom/Prenom/Email/-/info/
/Nom/Prenom/Email/-/info/adresse
/Nom/Prenom/Email/-/appels
/Nom/Prenom/Email/-/appels/derniersAppelsRecus
/Nom/Prenom/Email/-/appels/derniersAppelsEmis
/Nom/Prenom/Email/-/images
/Nom/Prenom/Email/-/images/photoProfile
/Nom/Prenom/Email/-/images/photoProfileAncienne
/Nom/Prenom/Email/-/T�lMobile
/Nom/Prenom/Email/-/DateAbonnement -->

```java
List<String> majorPath = new ArrayList<String>();
majorPath.add("Nom");
majorPath.add("Prenom");
majorPath.add("Email");

String minorComponent;
minorComponent = 'info';

Key myKey = Key.createKey(majorPath, minorComponent);
```

## Exercice JKV.2 : Test de la classe existante HelloBigDataWorld.java

Compilation
```
javac -g -cp $KVHOME/lib/kvclient.jar:$KVHOME/tpnosql $KVHOME/tpnosql/hello/HelloBigDataWorld.java
```
Execution
```
java -Xmx256m -Xms256m  -cp $KVHOME/lib/kvclient.jar:$KVHOME/tpnosql hello.HelloBigDataWorld
```

## Exercice JKV.3 : Coder votre propre classe MobileClients.java
Compilation
```
javac -g -cp $KVHOME/lib/kvclient.jar:$KVHOME/tpnosql $KVHOME/tpnosql/pkMobileClients/MobileClients.java
```
Execution
```
java -Xmx256m -Xms256m  -cp $KVHOME/lib/kvclient.jar:$KVHOME/tpnosql pkMobileClients.MobileClients
```




# Chapitre 6
## Utilisation d’ l’interface CLI
```sql
execute 'drop table CRITERES';
execute 'drop table CLIENT';
execute 'drop table APPRECIATION';
```

```sql
execute 'CREATE TABLE CRITERES (
	CRITEREID INTEGER, 
	TITRE STRING,
	DESCRIPTION STRING,
	PRIMARY KEY(CRITEREID)
)';
```
```sql
execute 'create table CLIENT (
    ClIENTID INTEGER, 
	NOM STRING, 
	PRENOM STRING, 
	CODEPOSTAL STRING,
	VILLE STRING,
	ADRESSE  STRING,
	TELEPHONE STRING,
	ANNEENAISS STRING,
	primary key(shard(CLIENTID)))';
```
```sql
execute 'create table APPRECIATION(
	VOLID INTEGER, 
	CRITEREID INTEGER, 
	CLIENTID INTEGER, 
	DATEVOL  STRING, 
	NOTE ENUM (EXCELLENT,TRES_BIEN, BIEN, 
	MOYEN, PASSABLE, MEDIOCRE
	),
	primary key (shard(VOLID, CRITEREID, CLIENTID, DATEVOL))
	)';
```
Insertion des lignes
```sql
put table -name criteres -json
'{"CRITEREID":1, 
"TITRE":"Qualit� du site reservation",
"DESCRIPTION": "Evaluer la qualit� du site de r�servation"
}';

put table -name criteres -json
'{"CRITEREID":2, 
"TITRE":"Prix",
"DESCRIPTION": "Evaluer le prix de la r�servation"
}';

put table -name criteres -json
'{"CRITEREID":3, 
"TITRE":"Nourriture � bord",
"DESCRIPTION": "Evaluer la qualit� de la nourriture � bord"
}';

put table -name criteres -json
'{"CRITEREID":4, 
"TITRE":"Qualit� du si�ge",
"DESCRIPTION": "Evaluer la qualit� du si�ge"
}';

put table -name criteres -json
'{"CRITEREID":5, 
"TITRE":"Qualit� du si�ge",
"DESCRIPTION": "Evaluer la qualit� du si�ge"
}';

put table -name criteres -json
'{"CRITEREID":6, 
"TITRE":"Accueil au guichet",
"DESCRIPTION": "Evaluer la qualit� accueil au guichet"
}';

put table -name criteres -json
'{"CRITEREID":7, 
"TITRE":"Accueil � bord",
"DESCRIPTION": "Evaluer la qualit� accueil � bord"
}';
```
Recherche
```sql

```
Compilation
```sql
javac -g -cp $KVHOME/lib/kvclient.jar:$KVHOME/tpnosql $KVHOME/tpnosql/airbase/Airbase.java  
```
Execution
```sql
java -Xmx256m -Xms256m  -cp $KVHOME/lib/kvclient.jar:$KVHOME/tpnosql airbase.Airbase
```







# Chapitre 7
## Exercice 7.1 : Création des tables externes HIVE pointant vers les tables physiques Oracle Nosql

### étape 1 : creation de tables dans la base nosql
```
Fait de l'exercice precedent
```
### étape 2 : creation de tables externes dans la base Hive
```sql
-- table externe Hive CRITERES_ONS_EXT
drop table CRITERES_ONS_EXT;

CREATE EXTERNAL TABLE  CRITERES_ONS_EXT  (CRITEREID int, TITRE string, DESCRIPTION string)
STORED BY 'oracle.kv.hadoop.hive.table.TableStorageHandler'
TBLPROPERTIES (
"oracle.kv.kvstore" = "kvstore",
"oracle.kv.hosts" = "bigdatalite.localdomain:5000", 
"oracle.kv.hadoop.hosts" = "bigdatalite.localdomain/127.0.0.1", 
"oracle.kv.tableName" = "criteres");


-- table externe Hive APPRECIATION_ONS_EXT
drop table APPRECIATION_ONS_EXT;

CREATE EXTERNAL TABLE APPRECIATION_ONS_EXT(VOLID int, CRITEREID int, CLIENTID INT,  DATEVOL  string, NOTE string)
STORED BY 'oracle.kv.hadoop.hive.table.TableStorageHandler'
TBLPROPERTIES (
"oracle.kv.kvstore" = "kvstore",
"oracle.kv.hosts" = "bigdatalite.localdomain:5000", 
"oracle.kv.hadoop.hosts" = "bigdatalite.localdomain/127.0.0.1", 
"oracle.kv.tableName" = "appreciation");


-- table externe Hive CLIENT_ONS_EXT
drop  table CLIENT_ONS_EXT;

create external table CLIENT_ONS_EXT (ClIENTID INT, NOM STRING, PRENOM STRING, CODEPOSTAL STRING,
VILLE STRING, ADRESSE  STRING, TELEPHONE STRING, ANNEENAISS STRING)
STORED BY 'oracle.kv.hadoop.hive.table.TableStorageHandler'
TBLPROPERTIES (
"oracle.kv.kvstore" = "kvstore",
"oracle.kv.hosts" = "bigdatalite.localdomain:5000", 
"oracle.kv.hadoop.hosts" = "bigdatalite.localdomain/127.0.0.1", 
"oracle.kv.tableName" = "client");
```


## Exercice 7.3 : Création des tables externes HIVE pointant vers des fichiers physiques Hadoop Hdfs



## Exercice 7.3 : Executer le script airbase.sql contenant les tables PILOTE, AVION et VOL



# HISTORY CMD
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