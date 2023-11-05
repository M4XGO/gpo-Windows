# Admin Windows - Maxime Nony

### GPO 1: 

#### Installer VisualStudioCode pour les utilisateurs dans le groupe `technique`

- Télécharger le fichier `.exe` d'installation de VScode. 

- Créer un script : 
``` ps1
# Spécifiez le chemin complet vers le fichier d'installation de Visual Studio Code (VSCodeSetup.exe)
$VSCodeInstallerPath = "\\w2022db\Partage\VSCodeSetup.exe"

# Vérifiez si le fichier d'installation existe sur le chemin spécifié
if (Test-Path $VSCodeInstallerPath) {
    # Exécutez l'installation silencieuse de Visual Studio Code
    Start-Process -FilePath $VSCodeInstallerPath -ArgumentList "/VERYSILENT /NORESTART" -Wait

    # Vérifiez si l'installation a réussi (facultatif)
    $vscodeInstalled = Test-Path "C:\Program Files\Microsoft VS Code\Code.exe"
    if ($vscodeInstalled) {
        Write-Host "Visual Studio Code a été installé avec succès."
    } else {
        Write-Host "L'installation de Visual Studio Code a échoué."
    }
} else {
    Write-Host "Le fichier d'installation de Visual Studio Code est introuvable. Veuillez vérifier le chemin."
}

```
-> ça va auto installer `.exe`.

- Placer ce script dans un dossier partagé avec les utilisateurs. 
- Placer le fichier `VSCodeSetup.exe` dans ce même fichier.
- Créer une GPO dans l'OU <b>technique/utilisateur</b> dans le `Gestion de stratégie de groupe`
- Modifier la GPO puis
- Ajouter un éxecution de scripts d'ouverture/fermeture dans la Configuration utilisateur/Paramètres Windows/Scripts(ouverture/fermeture). 
- Renseigner dans l'onglet <i>Script Powershell </i> le chemin complet du script ```\\w2022dc\Partage\installVscode.psi```
- Appliquer la GPO.

- Aller sur le post client.
- Lancer la commande `Gpupdate /force`
- Se déconnecter et se reconnecter du poste client. 

- #### Ça fonctionne ! 


### GPO 2: 

#### Bloquer l'accès à <b>Microsoft Edge</b> pour tout les utilisatuers.

- Dans l'application `Gestion de stratégie de groupe`
- Créer une GPO pour l'ensemble des utilisateurs du groupe `non.local`
- Modifier celle-ci en allant dans : `Paramètres Windows/Paramètres de sécurité/Stratégies de restriction logicielle`
- Clique droit sur cette onglet, selectionner `Nouvelles stratégies de restriction logicielle` puis `Règles supplémentaires`
- Clique droit sur `Nouvelle règle de chemin d'accès` puis rentrer le chemin d'accès au `.exe`de Microsoft Edge.

- Appliquer la GPO. 


- Aller sur le poste client.
- Faire la commande `Gpupdate /force`
- #### Ça fonctionne ! 


### GPO 3: 

#### Restreindre la connexion aux ordinateur sur le réseaux en fonction des groupes.

- Créer une nouvelle GPO puis la modifier. 
- Aller dans `Configuration ordinateur/Stratégies/Paramètres Windows/Paramètres de Sécurité/Stratégie locales/Attribution des droits utilisateurs`
- Clique droit dessus pour créer une nouvelle `Accéder à cet ordinateur à partir du réseaux`
- Ajouter les groupes (<b>TECH/DSI</b>)
- Appliquer les changement. 
- Appliquer la GPO.
- Se rendre sur un poste client <b>DIR</b> pour vérifier si la GPO à bien prix effet.
- #### Ça fonctionne !
