# Making Skyve for C:2 II work on Linux properly - a guide
A bit of an intro to this one. There are some guides on the internet which explain how to do it, but I haven't found any of them really working without breaking the game in itself, so after hours of trying to fix my own Skyve install I decided to make this guide. It might be more complicated than other guides, but you have the guarantee that your game installation won't behave any differently than before.

## Step 1: Important things to point out
- This guide will focus on Cities: Skylines II installed via **Steam** on default file location, if you have different install, please adjust the steps to your case.
-  This guide will **NOT** help you with any issues you have with the game itself, sorry.
- You will have to install those packages:
	- ``wine`` - kinda obvious, but make sure you have it
	- ``winetricks`` - will be necessary to make prefix easily (``winetricks-git`` also works)

## Step 2: Install Skyve
Unfortunately, you will have to install Skyve from PDX Mods twice, once in-game and once as a standalone executable.

1. Subscribe to Skyve [BETA] on PDX Mods which you can find [here](https://mods.paradoxplaza.com/mods/75804/Windows). **Do not click the "Install Skyve" button in the main menu of the game, it will probably not launch anyway**.
2. Download Skyve on the same site as a standalone executable.

## Step 3: Creating a Wine prefix
Without doing this, you won't be able to launch Skyve at all.
1. Launch ``winetricks``, given that you already have it installed. If not, install it.
	- Ignore any warnings that might pop-out while using the app, they won't cause any issues and you can safely click "OK" on any of them.
2. Select the option to **create a new wine prefix**. You can name it whatever you like, however for the rest of this guide I will assume you named it "Skyve".
3. While in the menu, click on **install a Windows component or DLL**. Another menu will pop out, in which you want to check the package ``dotnet48`` and click OK, it will install now.
	- Winetricks will now install dotNET 4.8 in your wine prefix. Be reminded that it will also install all of its dependencies, so you will have to wait and install every dependency that pops out until winetricks shows you the menu you were before again.
	- If you encounter any errors or warnings, ignore them.
4. Close ``winetricks``.

The default location of the prefix will now be in your home folder, more specifically under ``~/.local/share/wineprefixes/``.

## Step 4: Create Symlinks
Probably the most tedious yet the part which drove me insane the first time. You will have to create a lot of symlinks. If you aren't aware, symlinks are basically an advanced version of Windows' links or shortcuts.

1. Create a symlink to the **steamuser** folder of your C:S II prefix. By default it would be:
	-  ```$ ln -s ~/.local/share/Steam/steamapps/compatdata/949230/pfx/drive_c/users/steamuser ~/~/.local/share/wineprefixes/Skyve/drive_c/users/```
2. Create a symlink to the **AppData** folder of your C:S II prefix, this time for your user "account" under Skyve prefix.
	- Run ```$ mkdir -p ~/.local/share/wineprefixes/Skyve/drive_c/users/yourusername/AppData/LocalLow/Colossal\ Order/``` to make directories necessary. 
	- ```$ ln -s ~/.local/share/Steam/steamapps/compatdata/949230/pfx/drive_c/users/yourusername/AppData/LocalLow/Colossal\ Order/Cities\ Skylines\ II/```
	- In both commands, please make sure to change ``yourusername`` to your actual username.
3. Create a symlink to the game installation folder.
	- ```$ ln -s ~/.local/share/wineprefixes/Skyve/drive_c/Program\ Files\ (x86)/Steam/steamapps/ ~/.local/share/Steam/steamapps/common/Cities\ Skylines\ II/```

## Step 5: Run "Skyve Setup.exe"
1. If your game is open, please close it now.
2. Unzip the downloaded zipfile from Paradox Mods via any method you like to any location you want.
3. Run ``Skyve Setup.exe`` in the folder that you unarchived it to.
	- You can run it with this command: ```$ WINEPREFIX=/home/yourusername/.local/share/wineprefixes/Skyve wine "Skyve Setup.exe"```.
	- You can install Skyve into any destination you want to. However I must say that I am not sure if this method works for the Skyve Service. So unless you really need it, I would disable it.

Now you should be able to run Skyve without any issues! Do so with the command ```$ WINEPREFIX=/home/yourusername/.local/share/wineprefixes/Skyve wine Skyve.exe``` inside the folder you have ``Skyve.exe`` in.
