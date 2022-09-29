**Compte rendu TP 6 --Gestion du réseau**

Exercice 1. Adressage IP

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

Exercices 2. Préparation de l'environnement

1 -- J'ai commencé par rajouter les interfaces réseaux sur mes deux machines virtuelles.

![image](https://user-images.githubusercontent.com/104362418/193020186-2ff38e05-2418-4f5a-a247-b033b57f8365.png)

Dans les paramètres j'ai modifié l'adaptateur réseau pour le client.

![image](https://user-images.githubusercontent.com/104362418/193020402-b448c377-5128-43c2-96e0-c86ba6b75c90.png)

Et pour le serveur je lui ai rajouté un adaptateur réseau(Ils disposent de celui du Labos avec le DHCP et celui de notre LAN ICS_E38).

2 -- En démarrant puis en me connectant sur le serveur je constate que les deux interfaces réseau sont bien présentes. L'interface nommée "lo" correspond au localhost

3 -- 

