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
--> ça va auto installer `.exe` fichier.

Créez un partage réseau : Si vous n'en avez pas déjà un, créez un partage réseau sur un serveur ou un emplacement de stockage réseau approprié. Assurez-vous que les utilisateurs du groupe technique ont les autorisations de lecture pour accéder à ce partage.

Placez le script dans le partage réseau : Copiez le script PowerShell d'installation de Visual Studio Code (par exemple, Install-VSCode.ps1) dans le dossier partagé sur le réseau que vous avez créé à l'étape précédente.

Définissez le chemin complet du script dans la GPO : Lors de la création de la GPO et de l'ajout du script, spécifiez le chemin complet du script dans le champ "Script de connexion". Par exemple, si le script se trouve dans le dossier partagé \\serveur\partage, vous utiliserez le chemin complet \\serveur\partage\Install-VSCode.ps1.


Préparation du script :

Assurez-vous d'avoir préparé le script PowerShell d'installation de Visual Studio Code, comme décrit précédemment. Le script doit être accessible depuis le serveur ou le partage réseau auquel vos utilisateurs ont accès.
Ouvrir l'Éditeur de Stratégie de Groupe :

Sur un contrôleur de domaine ou un ordinateur administratif, ouvrez l'éditeur de gestion de stratégie de groupe (GPMC) en utilisant la commande gpmc.msc.
Créer une nouvelle GPO :

Dans l'arborescence de l'éditeur GPMC, cliquez avec le bouton droit sur l'OU (Unité d'Organisation) à laquelle vous souhaitez lier la GPO, puis sélectionnez "Créer un GPO dans cet espace et le lier ici". Donnez un nom approprié à la GPO, par exemple, "Installation de Visual Studio Code".
Éditer la GPO :

Cliquez avec le bouton droit sur la nouvelle GPO et sélectionnez "Modifier".
Ajouter le script de connexion :

Dans l'éditeur de GPO, accédez à "Configuration de l'utilisateur" > "Stratégies Windows" > "Scripts (ouverture/fermeture de session)".
Dans le volet de droite, double-cliquez sur "Ouverture de session" :

Cliquez sur l'onglet "Scripts de connexion".
Cliquez sur "Ajouter" pour sélectionner le script PowerShell que vous avez préparé pour l'installation de Visual Studio Code.
Spécifiez le chemin complet du script :

Assurez-vous de spécifier le chemin complet du script PowerShell dans le champ "Script de connexion". Par exemple, \\serveur\partage\Install-VSCode.ps1.
Enregistrez la GPO :

Fermez l'éditeur de GPO et enregistrez les modifications.
Liage de la GPO :

La GPO est maintenant créée, mais elle doit être liée à l'OU appropriée. Cliquez avec le bouton droit sur l'OU où vous souhaitez appliquer la GPO, puis sélectionnez "Lier un objet GPO existant" et choisissez la GPO que vous avez créée.
Testez la GPO :

Effectuez des tests en vous connectant avec un utilisateur du groupe technique pour vous assurer que le script d'installation de Visual Studio Code est exécuté lors de la connexion.
Vérification :

Après le test, vérifiez que Visual Studio Code a été installé avec succès sur les postes des utilisateurs.
