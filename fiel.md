# Admin Windows - Maxime Nony

### GPO 1: 

#### Installer VisualStudioCode pour les utilisateurs dans le groupe `technique`

- Télécharger le fichier `.exe` d'installation de VScode. 

- Créer un script : 
``` ps1
# Exemple de script PowerShell pour installer Visual Studio Code à partir d'un fichier .exe partagé

# Spécifiez le chemin complet vers le fichier d'installation de Visual Studio Code (VSCodeSetup.exe)
$VSCodeInstallerPath = "\\serveur\partage\VSCodeSetup.exe"

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
--> This will auto install the `.exe` file.
