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
- Créer une GPO dans l'OU <b>technique/utilisateur</b>. 
- Ajouter un éxecution de scripts d'ouverture/fermeture dans la Configuration utilisateur/Paramètres Windows/Scripts(ouverture/fermeture). 
- Renseigner dans l'onglet <i>Script Powershell </i> le chemin complet du script ```\\w2022dc\Partage\installVscode.psi```

- Aller sur le post client.
- Lancer la commande `Gpupdate /force`
- Se déconnecter et se reconnecter du poste client. 

- #### Ça fonctionne ! 


### GPO 2: 

#### Bloquer l'accès à <b>Microsoft Edge</b> pour tout les utilisatuers.