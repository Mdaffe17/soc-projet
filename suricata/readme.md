# Suricata - Configuration et Usage (Linux, Windows, macOS)

Ce dossier contient la configuration Suricata utilisée dans votre SOC.  
Suricata est déployé via Docker **uniquement pour Linux** (sniffing réseau réel possible).  

Pour **Windows et macOS**, Suricata doit être installé **directement sur la machine**, car Docker Desktop ne peut pas accéder aux interfaces réseau physiques.

---

# I. Déploiement Suricata (Linux uniquement)

Sur Linux, vous pouvez utiliser le fichier `docker-compose.yml` fourni pour capturer le trafic en temps réel :

### Lancer Suricata :

```bash
docker compose up -d
```

Les logs et alertes seront dans :

```
suricata/logs/eve.json
suricata/logs/
```

---

# II. Suricata sur macOS et Windows

  **Docker ne peut PAS sniffer le trafic réel sur macOS / Windows**  
  Il utilise une VM invisible → impossible de voir le trafic de votre machine.

  **Solution recommandée** : Installer Suricata directement sur l’OS.

---

## Windows

1. Télécharger Suricata :
https://www.openinfosecfoundation.org/download/
2. Installer avec les options par défaut.
3. Trouver l’ID Suricata correspondant à cette interface

```Powershell
"C:\Program Files\Suricata\suricata.exe" -i -v
```

4. Remplacer le fichier de configuration par celui fourni ici (suricata.yaml) et dans ce même fichier suricata.yaml remplacé interface : eth0 par l'ID trouvé avec la commande précédente. Ex si ID trouvé égal 5, alors on fait interface : 5

Emplacement

```Powershell
C:\ProgramData\Suricata\suricata.yaml
```

5. Démarrer Suricata sur l’interface visible de Windows (ex. Ethernet, Wi-Fi)

```Powershell  
 suricata.exe -c suricata.yaml
```
---

## MacOS

1. Installer suricata via Homebrew :

```zsh
brew install suricata
```

2. Identifier l'interface réseau avec la commande :

```zsh
 ifconfig        #généralement en0  
```    

3. Remplacer le fichier de configuration par celui fourni ici (suricata.yaml) et dans ce même fihcier remplacé l'interface réseau trouvé avec la commande ifconfig. Ex : interface : en0

-Emplacement pour les mac ARM

```zsh
/opt/homebrew/etc/suricata/suricata.yaml
```

-Emplacement pour les macs Intel

```zsh
/usr/local/etc/suricata/suricata.yaml
```

4. Démarrer suricata
```zsh
sudo suricata -c suricata.yaml
```
---

# III. Configuration utilisée dans ce projet (`suricata.yaml`)

Cette version de suricata.yaml disponible dans notre dépôt est **universelle**, 
compatible Linux/Windows/macOS :

# V. Vérifier que Suricata fonctionne

Sur Linux :

1. Lancer un ping :
   ```bash
   ping google.com
   ```

2. Voir les logs :
   ```
   logs/eve.json
   ```
---

#  Notes importantes

- Sur Windows/macOS → installation native obligatoire pour surveiller le trafic réel.
- Cette configuration du fichier suricata.yaml fonctionnera parfaitement sur tous les OS.
---



