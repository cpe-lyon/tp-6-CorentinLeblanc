**Compte rendu TP 6 --Gestion du réseau**

**<ins>Exercice 1. Adressage IP</ins>**

Nous avons utilisé du VLSM. Voici les sous-réseaux:

| Name  | Hosts Needed | Hosts Available | Unused Hosts | Network Address | Slash | Mask            | Usable Range                | Broadcast    |
|-------|--------------|-----------------|--------------|-----------------|-------|-----------------|-----------------------------|--------------|
| Host1 | 52           | 62              | 10           | 172.16.0.0      | /26   | 255.255.255.192 | 172.16.0.1 - 172.16.0.62    | 172.16.0.63  |
| Host2 | 38           | 62              | 24           | 172.16.0.64     | /26   | 255.255.255.192 | 172.16.0.65 - 172.16.0.126  | 172.16.0.127 |
| Host3 | 37           | 62              | 25           | 172.16.0.128    | /26   | 255.255.255.192 | 172.16.0.129 - 172.16.0.190 | 172.16.0.191 |
| Host4 | 35           | 62              | 27           | 172.16.0.192    | /26   | 255.255.255.192 | 172.16.0.193 - 172.16.0.254 | 172.16.0.255 |
| Host5 | 34           | 62              | 28           | 172.16.1.0      | /26   | 255.255.255.192 | 172.16.1.1 - 172.16.1.62    | 172.16.1.63  |
| Host6 | 33           | 62              | 29           | 172.16.1.64     | /26   | 255.255.255.192 | 172.16.1.65 - 172.16.1.126  | 172.16.1.127 |
| Host7 | 25           | 30              | 5            | 172.16.1.128    | /27   | 255.255.255.224 | 172.16.1.129 - 172.16.1.158 | 172.16.1.159 |

**<ins>Exercices 2. Préparation de l'environnement</ins>**

1 -- J'ai commencé par rajouter les interfaces réseaux sur mes deux machines virtuelles.

![image](https://user-images.githubusercontent.com/104362418/193020186-2ff38e05-2418-4f5a-a247-b033b57f8365.png)

Dans les paramètres j'ai modifié l'adaptateur réseau pour le client.

![image](https://user-images.githubusercontent.com/104362418/193020402-b448c377-5128-43c2-96e0-c86ba6b75c90.png)

Et pour le serveur je lui ai rajouté un adaptateur réseau(Ils disposent de celui du Labos avec le DHCP et celui de notre LAN ICS_E38).

2 -- En démarrant puis en me connectant sur le serveur je constate que les deux interfaces réseau sont bien présentes. L'interface nommée **"lo"** correspond au localhost ou de loopback qui correspond à la machine elle-même.

3 -- J'ai désinstallé complétement ce paquet grâce à la commande **"sudo apt autoremove cloud-init"**.

![image](https://user-images.githubusercontent.com/104362418/193023039-5aa6ff6b-e36e-44f9-a152-c3fbcc75d686.png)

Après redémarrage du serveur, le package est bien supprimé.

![image](https://user-images.githubusercontent.com/104362418/193023517-c8f61023-d01a-447e-8b7d-eb3d93e6a6b8.png)

J'aurais également pu utiliser la commande "**apt remove cloud-init && apt autoremove**", puis "**apt purge cloud-init**"
et enfin "**rm -rf /etc/cloud**" pour supprimer tout le dossier.

4 -- J'ai changé le nom du serveur comme demandé. J'ai pu l'affiché derrière avec la commande **"hostnamectl"**.

![image](https://user-images.githubusercontent.com/104362418/193024741-f89aa36d-91f4-4877-adcf-88bff329097f.png)

J'aurais aussi pu modifier le fichier "**/etc/hostname**" de la machine et indiquer dans le fichier "**/etc/hosts**" le nom
de la machine associé.

**<ins>Exercice 3. Installation du serveur DHCP</ins>**

1 -- L'installation du paquet isc-dhcp-server s'est faite avec la commande **"sudo apt install isc-dhcp-server"**.

![image](https://user-images.githubusercontent.com/104362418/193025455-8ed6803b-e520-4742-8008-4749b77214dd.png)

Le serveur n'a en effet pas réussi à démarrer car il n'est pas encore configuré.

2 -- J'ai changé de manière permanente l'adresse IP de l'interface réseau du réseau interne dans le fichier **"/etc/netplan/50-cloud-init.yaml"**. Pour mettre une interface up, on peux utiliser **"ip link"** ou **"sudo dhclient -r"**.

![image](https://user-images.githubusercontent.com/104362418/193031621-2ece2596-3f61-44ac-8b8e-ec16e7753426.png)

Il a fallu faire attention à bien respecté l'indentation.
Au redémarrage, les ips que l'on constate sont les bonnes.

![image](https://user-images.githubusercontent.com/104362418/193032096-af4a4f74-4d31-4b67-bc8f-3fd613992e87.png)

J'aurais également pu créer un nouveau fichier dans "**/etc/netplan/99-netcfg.yaml**" pour éviter de le 
surcharger par un autre fichier. Faire attention ne pas mettre de TAB uniquement des espaces. On fait 
"**sudo netplan try**" puis "**ip a**".

3 -- J'ai copié le fichier **"/etc/dhcp/dhcp.conf"** et créé une copie de ce fichier. 

![image](https://user-images.githubusercontent.com/104362418/193033555-4dd9897f-dc31-4a22-949d-c7c933a314ec.png)

J'ai ensuite modifié le fichier **"dhcpd.conf"** comme demandé.

![image](https://user-images.githubusercontent.com/104362418/193036271-9deb267c-fc09-4faa-b493-b4eafd4364e8.png)

Les deux premières lignes correspondent au temps par défaut du bail et du temps maximal du bail, tout est en seconde.

4 -- Voici les modifications que j'ai apporté au fichier **"/etc/default/isc-dhcp-server"**.

![image](https://user-images.githubusercontent.com/104362418/193037699-38f50db8-a7bd-4e8e-84f6-2b58dbdc3229.png)

5 -- Après avoir validé la configuration de mon fichier grâce à la commande "dhcpd -t" puis avoir redémarrez le serveur DHCP avec le commande **"systemctl restart isc-dhcp-server"**, je constate que mon serveur DHCP est actif.

![image](https://user-images.githubusercontent.com/104362418/193038165-93c41908-c621-4c88-b887-a64acbee3573.png)

6 -- J'ai fais comme à l'exercice précédent pour désinstaller cloud-init puis j'ai fais la même chose pour modifier le nom de la machine. Il a également fallu que je change l'ancien nom de la machine dans le fichier /etc/hosts pour que la commande **"sudo"** s'éxecute plus rapidement.

7 -- Voici une capture d'écran d'une partie du fichier **"/var/log/syslog"**:

![image](https://user-images.githubusercontent.com/104362418/193048080-8cd2fea2-15d9-4d15-8d67-b82dcf32841c.png)

DHCPREQUEST correspond à la demande faites par le client pour obtenir une adresse IP.

DHCPACK correspond au message de recéption du serveur vers le client.

DHCPDISCOVER correspond à la découverte des adresses disponibles sur le réseau.

DHCPOFFER correspond à l'étape ou le serveur donne une adresse IP disponible au client.

8 -- Le fichier **"/var/lib/dhcp/dhcpd.leases"** contient toutes les demandes de bails effectués par les clients.

![image](https://user-images.githubusercontent.com/104362418/193049826-4ddf59c7-608c-436b-b4d5-96a88fc2de91.png)

La commande **"dhcp-lease-list"** affiche la liste des bails et jusqu'a quand ils sont disponibles.

![image](https://user-images.githubusercontent.com/104362418/193050255-99576b00-3e51-442d-b479-86be3b3d3b0e.png)

9 -- La communication s'effectue bien dans les deux sens.

![image](https://user-images.githubusercontent.com/104362418/193050485-63cef397-c93d-4b5d-8602-2c8eadb5ca7b.png)

![image](https://user-images.githubusercontent.com/104362418/193050619-262ac29f-ad01-44e0-972b-0079039a47c7.png)

10 -- Après avoir effectué les modifications demandées sur le serveur, on constate que l'IP de l'interface à bien changé.

![image](https://user-images.githubusercontent.com/104362418/193056906-5162bc3f-f84d-4d29-aaf3-bc24d394b5c2.png)

Exercice 4. Donner un accès à Internet au client

1 -- J'ai décommenté la ligne **"net.ipv4.ip_forward=1"** dans le fichier **"/etc/sysctl.conf"**.

![image](https://user-images.githubusercontent.com/104362418/193058516-4bf2ef8f-b038-4f6a-8eb2-29a52d966b7e.png)

![image](https://user-images.githubusercontent.com/104362418/193058585-9cea9e3f-ba0c-40d5-b434-30f0c2be0a6d.png)

![image](https://user-images.githubusercontent.com/104362418/193058647-3f19ea6e-923f-4a5c-81dc-b6e6cff07557.png)

La valeur à donc bien été prise en compte.

2 -- J'ai donc autorisé la traduction d'adresse source en ajoutant la règle iptables

![image](https://user-images.githubusercontent.com/104362418/193526559-fa3e20dc-b9be-493f-a400-ab4caf8aef3b.png)

Les pings sur 8.8.8.8 et sur 1.1.1.1 fonctionnent donc bien sur le serveur.

![image](https://user-images.githubusercontent.com/104362418/193526733-be0fe349-7bd9-45c5-9e99-8dade325d30a.png)

Exercice 5. Installation du serveur DNS

1 -- J'ai installé le programmer **"bind9"** et je me suis assurée que le service est bien actif.

![image](https://user-images.githubusercontent.com/104362418/193530632-5076efe1-adf6-4cc8-acd9-ef2edeb6efb0.png)

2 -- J'ai donc fais un **"sudo nano /etc/bind/name.conf.options"**, j'ai décommenté la partie forwarders et remplacé les DNS.

![image](https://user-images.githubusercontent.com/104362418/193531339-b9760df5-64b4-41d1-ab70-bcf3f73f7954.png)

J'ai ensuite redémarré le serveur bind9.

![image](https://user-images.githubusercontent.com/104362418/193531910-c87a50d2-4e11-4487-b038-f4dd8c65e1c9.png)

3 -- Le ping sur "www.google.fr" fonctionne.

![image](https://user-images.githubusercontent.com/104362418/193533016-ef0d0f73-dc4b-449e-b344-89de6f80f4ee.png)

4 -- La commande pour surfer sur le site est **"lynx fr.wikipedia.fr"**.

![image](https://user-images.githubusercontent.com/104362418/193534215-e422e26b-7aa3-444b-ab3c-9129fca4f576.png)

Exercice 6. Configuration du serveur DNS pour la zone tpadmin.local

1 -- J'ai ajouté les lignes demandés au fichier suivant **"/etc/bin/db.tpadmin.local"**.

![image](https://user-images.githubusercontent.com/104362418/193535382-140dc08e-a2d2-4c75-9a02-daec844f95e5.png)

2 -- J'ai d'abord copié le fichier "db.local" et renommé la copie **"db.tpadmin.local"**, j'ai remplacé tout les localhost par tpadmin.local puis l'adresse 127.0.0.1 par l'adresse IP du serveur.

![image](https://user-images.githubusercontent.com/104362418/193537151-5272bbe7-bdf6-4196-af01-4f9f6c5220b1.png)

3 -- J'ai ajouté les lignes demandés au fichier **"named.conf.local"**.

![image](https://user-images.githubusercontent.com/104362418/193537626-4e9e68c5-51f4-4659-9f5e-559e11998572.png)

![image](https://user-images.githubusercontent.com/104362418/193539655-6309ee03-3604-4ead-ac09-4bcae089849e.png)

J'ai rentré l'adresse IP du serveur à l'envers car c'est un fichier de configuration de zone inverse.

4 -- Mes fichiers de configuration ont bien été validée

![image](https://user-images.githubusercontent.com/104362418/193540381-6b5713f1-9639-4120-b646-5f3fdf0d975c.png)

5 -- Après redémmarage du serveur bind9 les deux ping fonctionnent bien.

Le ping du client vers le serveur :

![image](https://user-images.githubusercontent.com/104362418/193544652-f3ea5ccb-62aa-4900-afa6-1642fa3562b4.png)

Le ping du serveur vers le client ne marche pas. Comme solution, j'ai trouvé une méthode pour forcer le fichier resolv.conf à prendre en compte une nouvelle ligne.

![image](https://user-images.githubusercontent.com/104362418/193547053-a623d0d4-6bdb-4deb-a182-aee096e88cdf.png)

![image](https://user-images.githubusercontent.com/104362418/193547150-f2b13c71-5f34-4ea9-8cab-17dcfb1e9561.png)

