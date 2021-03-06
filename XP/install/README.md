## Installation helpers for Sitecore Experience Platform (XP)

> These steps assume you have all of the prerequisites installed or have followed the instructions at [README.md](../../Prerequisites/README.md)

### Solr

Solr can be installed with a script if you do not already have it.

Still in an elevated PowerShell session:

- Browse to the solr folder

  ```powershell
  Set-Location Sitecore.Habitathome.Utilities\XP\Install\solr
  ```

- Review the `install-solr.ps1` script to ensure the Java and Solr versions are correct

- Install Solr

  ```powershell
  .\install-solr.ps1
  ```

> Once setup is complete, the installer should load the solr 'home' page

#### To Uninstall Solr

- Review the `remove-solr.ps1` script to ensure the Solr versions and its data folder are correct.
- Uninstall Solr

  ```powershell
  .\remove-solr.ps1
  ```

### Prepare to install XP

```
Set-Location ..
```

#### A bit about the process

The `install-xp0.ps1` script takes in a JSON configuration file (defaults to `.\configuration-xp0.json`) and runs through the entire platform installation.

The `install-modules.ps1` script uses the same JSON configuration file as the install-xp0.ps1 script and installs optional modules.

The `assets.json` file is used to manage which modules you would like to install. Make sure this file is configured correctly before you run the set-installation-defaults.ps1 script.

The `configuration-xp0.json` is generated by running two powershell scripts. These powershell scripts use a template settings file (`install-settings.json`) and populates some values.

The `set-installation-defaults.ps1` does what it sounds like - it creates the `configuration-xp0.json` file and populates it with the default values based on the version of the platform it was meant to support.

The `set-installation-overrides.ps1.example` script needs to be renamed and modified. Executing it once it has been renamed will fill in the rest of the information in the `configuration-xp0.json` file as well as override some of the default values.

----------

#### Installation

- Review the `assets.json` file to ensure everything is configured correctly. 
	- By default, only the SPE and SXA modules get installed automatically. If you would like to install the Data Exchange Framework-related modules or the Salesforce Marketing Cloud connect modules you will need to set the appropriate flags in this file.


- Set some predefined defaults
  ```
	.\set-installation-defaults.ps1
	```

- Create a copy of the overrides file
  ```
	copy set-installation-overrides.ps1.example set-installation-overrides.ps1
	```

- Edit **`set-installation-overrides.ps1`** to set the SQL instance name and sa password

- Apply overrides
  ```
	.\set-installation-overrides.ps1
	```

- Copy your license file to the `.\assets` folder
  ```
	copy ~\Downloads\license.xml .\assets
	```

- Let's GO!
  ```
  .\install-singledeveloper.ps1
  ```
- Install Modules
```.\install-modules.ps1```

> You will be prompted for your dev.sitecore.com credentials - the required assets will be downloaded automatically.

> **KNOWN ISSUE:** Downloading large files take time and are causing connection issues.
> After the download of larger files, you may get an error `Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host.` The download was NOT completed successfully.
> 
> **WORKAROUND:** Manually download the larger Sitecore WDP Package required and place it into the `XP/install/assets` folder.
