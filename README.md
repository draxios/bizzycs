# Step 1: Create a New User
First, create a new user named 'bizzycs' for running the CS2 server:

Open an ssh [terminal](https://www.putty.org/) as a sudo user and enter:
```
sudo adduser bizzycs
sudo adduser bizzycs sudo
```
Follow the prompts to set up the new user. You can leave some fields blank if you want.

# Step 2: Install SteamCMD and CS2
Next, we will install SteamCMD, which is a command-line version of the Steam client.
We will next install CS2. First we set the location of where we want it, then we install via [id 730](https://developer.valvesoftware.com/wiki/Steam_Application_IDs).

Update your package lists:
```
sudo apt update
```

Install prereqs (_deprecated_):
```
dpkg --add-architecture i386
apt-get upgrade
apt-get install libc6:i386
apt-get install lib32z1
apt-get install screen
```

Switch New User:
```
login
```
>Or just close your terminal and reconnect using new bizzycs user!

Install SteamCMD:
```
wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
tar -xvzf steamcmd_linux.tar.gz
./steamcmd.sh
force_install_dir ./Steam/steamapps/common/cs2
login anonymous
app_update 730 validate
quit
```

# Step 3: Configure CS2 Server
Make sure your on the bizzycs user and at the right location (can use pwd) at: /home/bizzycs 

If still on another user or root, switch to the bizzycs user:
```
su - bizzycs
```

# Step 4: Create and Configure the server.cfg
Navigate to the CS2 server configuration directory:
```
cd /home/bizzycs/Steam/steamapps/common/cs2/game/csgo/cfg
```
Copy the server.cfg from this repo or create your own using a text editor like nano:
```
nano server.cfg
```
Add your server configurations (hostname, rcon password, etc.) to this file.

# Step 5: Start the Server
Run the CS2 server with the necessary command-line parameters.

Navigate to the server's main runtime directory for Ubuntu:
```
cd /home/bizzycs/Steam/steamapps/common/cs2/game/bin/linuxsteamrt64
```

Start the server with a command like:
```
./cs2 -dedicated +map cs_office +sv_setsteamaccount [GSLT_TOKEN] +maxplayers 20 +sv_lan 0 +sv_cheats 0
```
>NOTE: You will need a GSLT token, get one [here](https://steamcommunity.com/dev/managegameservers)

Adjust any parameters as needed for your server setup (e.g., game type, mode, map group).
[Only people worse than me at documenting is Valve](https://developer.valvesoftware.com/wiki/Command_line_options)

# Step 6: Maintain and Update the Server
Regularly update the server using SteamCMD.

Run SteamCMD with the update command as before:
```
cd /home/bizzycs
./steamcmd.sh +login anonymous +force_install_dir /home/bizzycs/Steam/steamapps/common/cs2 +app_update 730 validate +quit
```

# Additional Notes
Remember to configure any firewalls or network settings to allow traffic on the necessary ports (default 27015 for CS2).

### Ubuntu 22.04:
```
sudo ufw allow from any to any port 27015 proto tcp
sudo ufw allow from any to any port 27015 proto udp
```

# Additonal Resources
[Valve CS2 Dedicated Server Wiki](https://developer.valvesoftware.com/wiki/Counter-Strike_2/Dedicated_Servers)
[Valve Command line Options](https://developer.valvesoftware.com/wiki/Command_line_options)
[CS2 RCON Tool](https://github.com/fpaezf/CS2-RCON-Tool-V2)
[AlliedModders](https://forums.alliedmods.net/)
[AlliedModders CS2 Server Linux Thread](https://forums.alliedmods.net/showthread.php?t=344056)
[Reddit Guide](https://www.reddit.com/r/cs2/comments/16vto3o/guide_to_running_a_dedicated_server_on_linux/)
[Docker Image](https://hub.docker.com/r/joedwards32/cs2)

>And yo! Please regularly back up your server configuration and other important files.
