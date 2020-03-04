# Guide d'utilisation : Configuration du client de VPN pour Windows 10

Ces instructions permettent de configurer le VPN d'accès distant avec le client natif de Windows 10.

#### Ajouter le certificat à la liste des certificats autorisés

1. Executer `mmc.exe`
1. Cliquer sur *Files > Add/Remove Snap-in…*
1. Sélectionner *Certificates* dans la liste à gauche puis cliquer sur *Add >*.
1. Dans la fenêtre qui apparaît, sélectionner **Computer account**.

![Certificates snap-in](Win-1-Computer-Account.png)

5. Dans la prochaine fenêtre, sélectionner **Local Computer: (the computer this console is running on)**  puis cliquer sur *Finish*.
5. Dans la fenêtre de la console, sélectionner *Console Root > Certificates (Local Computer) > Trusted Root Certification Authorities > Certificates*.
5. Faire un clic droit sur *Certificates* puis cliquer sur *All Tasks > Import…*.

![All tasks menu, import option](Win-2-Import.png)

8. Dans la prochaine fenêtre, cliquer sur *Next*.
8. Cliquer sur *Browse…* et sélectionner le certificat que vous avez enregistré avec l'extension **.crt** (le certificat est disponible dans l'interface de cloud.ca, sur la page dédiée à la configuration du VPN d'accès distant) puis cliquer sur *Next*.

![Certificate import wizard](Win-3-Browse.png)

10. Garder **Place all certificates in the following store: Trusted Root Certification Authorities** puis cliquez sur *Next*
10. Cliquer sur *Finish*. Vous devriez voir le message *The import was successful*. Vous pouvez fermer les fenêtres ouvertes.
10. Si votre server est derrière un NAT, suivre cette procédure: [Microsoft link](https://support.microsoft.com/en-us/help/926179/how-to-configure-an-l2tp-ipsec-server-behind-a-nat-t-device-in-windows-vista-and-in-windows-server-2008)


#### Créer la connexion VPN:
[](Win-4-Settings.png)

![](Win-5-VPN.png)

![](Win-6-Add-Connection.png)

![](Win-7-Connection-Details.png)

![](Win-8-Select-Connection.png)


#### Initialiser la connexion VPN:
![](Win-9-Connect.png)

![](Win-10-Connected.png)