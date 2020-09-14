# How to install It Lurks Below on Linux (GOG, Vanilla Wine, and DXVK)

## Testing Information

- Distro: [Pop OS 20.04 (Ubuntu 20.04 Based)](https://pop.system76.com/)
- Wine Version: [5.17 (Staging, Upstream)](https://www.winehq.org/)
- Requires: [DXVK (1.7.1+)](https://github.com/doitsujin/dxvk)
- Source: [GOG](https://www.gog.com/)
- Tester: Jonathan Vasquez (fearedbliss)

## Notes

- GOG is a DRM-free gaming platform which is an alternative to Steam. Because of
  this, we can easily download the DRM-free version of **`It Lurks Below`**
  and run it directly through a vanilla version of Wine without requiring
  Proton or Steam.
- Works in both 32 and 64 bit Wine Bottles with the default settings.
- The game will not run without DXVK installed (It will lock up with a black
  screen upon launching the game).
- These instructions assume you have already installed Wine on your system.
  If you don't have Wine installed yet, please follow the instructions on
  [Wine's download page](https://wiki.winehq.org/Download).

## Instructions

1. Download ILB from GOG's offline backups. You can reach this by logging into
   your GOG account on their website, going to your [games collection](https://www.gog.com/account)
   and clicking **`It Lurks Below`**. You should see a section in there that's
   called **`Download Offline Backup Game Installers`**. Expand that section and
   download the game.

1. Now we will create a Wine Prefix that will hold only **`It Lurks Below`**.
   We use prefixes in order to isolate applications from other wine
   environments and thus this allows us to install DXVK and make other changes
   that will only affect **`It Lurks Below`**. Each wine related command needs
   to be prefixed in order to make sure we are affecting the correct
   environment (You can export the variable if you know what you are doing).
   You can also pick the location of where you want the prefix to be in, just
   make sure you use that prefix for ILB consistently across the commands below.

    ```
    $ WINEPREFIX="${HOME}/prefix/ilb" winecfg
    ```

   - When the **`Wine configuration`** window appears, Press **`Ok`**.

1. Install and Setup DXVK 1.7.1

    The following commands will download and install DXVK 1.7.1 from their
    [Github page](https://github.com/doitsujin/dxvk/releases/tag/v1.7.1) into
    your prefix.

    ```
    $ wget https://github.com/doitsujin/dxvk/releases/download/v1.7.1/dxvk-1.7.1.tar.gz
    $ tar xf dxvk-1.7.1.tar.gz
    $ cd dxvk-1.7.1
    $ WINEPREFIX="${HOME}/prefix/ilb" ./setup-dxvk.sh install
    ```

1. Install ILB from the GOG installer

    The last step is to run the installer through wine. Find the installer
    executable you downloaded and execute it. Follow the instructions and
    install the game. For any errors that occur, just press **`Ok`** until the
    installer completes.

    ```
    $ WINEPREFIX="${HOME}/prefix/ilb" wine setup_it_lurks_below_1.01n_\(37717\).exe
    ```

1. That's it! Click **`Launch`** at the end of the installer and enjoy.


## Upgrading

When you want to upgrade ILB, you should be able to download the new installer
from your GOG's games collection (Step 1), and then run Step 4 again, targetting
the new installer. This should install the new ILB and overwrite the existing
files in place. Everything else is already set up properly so nothing else
needs to change.

## Latest Available Version (Offline Installer vs [GOG Galaxy](https://www.gog.com/galaxy))

For a variety of reasons ranging from testing, deployments, and other process
oriented tasks, there may be a delay downloading the latest version of the game
via the **`Download Offline Backup Game Installers`** vs using **`GOG Galaxy`**,
which will always allow you to download the latest version published. The delay
shouldn't be too long and most of the time the latest version will be the same
in both places, so this isn't much of an issue. 

### Getting the latest version on Windows using GOG Galaxy (Workaround)

If for whatever reason you need the latest version immediately, you could use
**`GOG Galaxy`** to get the latest version. Since **`GOG Galaxy`** doesn't run
on Linux (And doesn't work well even with Wine), if you have a spare physical or
virtual Windows machine, you could run **`GOG Galaxy`** there, and download the
latest version of **`It Lurks Below`**. Once that's done, simply swap the old
**`It Lurks Below`** folder on your Linux machine, with the new one. As a side
note, the actual location of where the **`It Lurks Below`** folder exists
doesn't matter, as long as when you launch the game, you are using the correct
prefix that we set up previously. So if there is a place on your drive (avoid
network drives) that you can easily get to to swap the folder during updates,
you could give this a shot.

### [Minigalaxy](https://github.com/sharkwouter/minigalaxy)

**`Minigalaxy`** is an open source alternative client to **`GOG Galaxy`** for
Linux. I attempted to use this client to see if I could download the latest
version of **`It Lurks Below`** that would have gotten pulled if we were to use
**`GOG Galaxy`**, however this wasn't the case. **`Minigalaxy`** pulled the same
version of the game available via the **`Offline Installer`**. This makes me
believe that **`Minigalaxy`** is pulling from the same source where the
**`Offline Installer`** is stored rather than wherever **`GOG Galaxy`** is
pulling from. Due to this, I would recommend sticking with downloading the game
manually through the website since this allows you to have less things installed
on your system, and allows you to have stricter control of your prefixes and
setup. If you do use **`Minigalaxy`** to do your installations, you'll still
need to set up DXVK and make any other modifications required to the automatic
prefixes that **`Minigalaxy`** creates.

## Launch Script

You can launch the game automatically through the desktop icon created by
the GOG installer. If you unchecked the **`Create desktop icon`** option in the
installer and no longer have an icon to launch the game through, the following
script will allow you to properly launch the game through the proper prefix.
Simply adjust the paths in the script for your system:

```
#/bin/sh

PREFIX="${HOME}/prefix/ilb"
ILB_DIR_PATH="${PREFIX}/drive_c/GOG Games/It Lurks Below"
ILB_EXECUTABLE="ILB.exe"

# Must switch into this directory before launching
# or the game will crash. Most likely related to
# wine needing to load the remaining dlls, etc.
cd "${ILB_DIR_PATH}"

# Switch DXVK_HUD below to 1 if you want to show the
# GPU and driver version alongside a FPS counter.
DXVK_HUD=0 WINEPREFIX="${PREFIX}" wine "${ILB_EXECUTABLE}" &
```

If you want to have a nice desktop icon, you can make a new file with the
**`.desktop`** extension and place the following contents inside (adjust the
**`Exec`** and **`Icon`** paths so that it points to your preferred paths):

```
[Desktop Entry]
Name=It Lurks Below
Exec=/path/to/your/launch/script.sh
Icon=/path/to/your/icon.ico
Type=Application
Terminal=false
```

Once created, place this script in **`~/.local/share/applications/`** and
wait a few seconds so that your desktop environment refreshes its list of
detected applications (**`GNOME`** and **`KDE`** should do this automatically).
Once you see it in your menu, clicking it should launch the game. You may need
to play around with the path you place the scripts and icons in since sometimes
the system is sensitive about spaces and locations.

## Character Save Location

Your character saves and other data for **`It Lurks Below`** are located at:

```
${WINEPREFIX}/drive_c/users/<your name>/Local Settings/Application Data/Graybeard Games/It Lurks Below
```

## Screenshots

![](https://i.imgur.com/oZO0FIv.png)
![](https://i.imgur.com/sgRdBak.png)
![](https://i.imgur.com/YhNL4j6.png)
![](https://i.imgur.com/PIUCJ43.png)
