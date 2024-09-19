<h1 align="center">Limo <img src="resources/logo.png" alt="logo" width="40"/></h1>

General purpose mod manager primarily developed for Linux with support for the [NexusMods](https://www.nexusmods.com/) API and [LOOT](https://loot.github.io/).

<p align="center">
<img src="resources/showcase.png" alt="logo" width="800"/>
</p>

## Features

- Multiple target directories per application
- Automatic adaptation of mod file names to prevent issues with case mismatches
- Auto-Tagging system for filtering
- FOMOD support
- Sort load order according to conflicts
- Import installed games from Steam
- Simple backup system
- LOOT integration:
    - Manage installed plugins
    - Automatically sort the load order
    - Check for issues with installed plugins
- NexusMods API support:
    - Check for mod updates
    - View description, changelogs and available files
    - Download mods through Limo
    
## How Limo works

There are two basic concepts you should know in order to understand how Limo works:
The *Staging Directory* and *Deployers*.

### Staging Directory
Whenever you install a mod, Limo does not change any file of the game you wish to mod immediately.
It instead stores all of the mod's files inside of the so called *Staging Directory*. This has the advantage of allowing
you to quickly change which mods should be enabled or win conflicts, should there be any.

### Deployers
In order to actually change the game you are trying to mod, there needs to be a mechanism which takes mods from the
*Staging Directory* and puts them into the game's directory. This is what *Deployers* do. Each *Deployer* manages
however many mods you assign to it and then links them into its *Target Directory*, which is the game's directory.
If there are any conflicts between mods, the mod lower in the *Deployer's* load order will win. Should any files in 
the *Target Directory* need to be overwritten, a backup is automatically created and restored when the mod is no
longer active.
    
***For a guide on how to use Limo, refer to the [wiki](https://github.com/limo-app/limo/wiki). This will help you even if you are not modding Skyrim.***

## Installation

### Flatpak

<a href='https://flathub.org/apps/io.github.limo_app.limo'>
    <img width='240' alt='Download on Flathub' src='https://flathub.org/api/badge?locale=en'/>
</a>

### Build from source

####  Install the dependencies

 - [Qt5](https://doc.qt.io/qt-5/index.html)
 - [JsonCpp](https://github.com/open-source-parsers/jsoncpp)
 - [libarchive](https://github.com/libarchive/libarchive)
 - [pugixml](https://github.com/zeux/pugixml)
 - [OpenSSL](https://github.com/openssl/openssl)
 - [cpr](https://github.com/libcpr/cpr)
 - [libloot](https://github.com/loot/libloot)
 - (Optional, for tests) [Catch2](https://github.com/catchorg/Catch2)
 - (Optional, for docs) [doxygen](https://github.com/doxygen/doxygen)

On Debian based systems most dependencies, with the exception of cpr and libloot, can be installed with the following command:

```
sudo apt install \
		build-essential \
		cmake \
		git \
		libpugixml-dev \
		libjsoncpp-dev \
		libarchive-dev \
		pkg-config \
		libssl-dev \
		qtbase5-dev \
		qtchooser \
		qt5-qmake \
		qtbase5-dev-tools \
		libqt5svg5-dev \
		libboost-all-dev \
		libtbb-dev \
		cargo \
		cbindgen \
		catch2 \
		doxygen		
```

#### Clone this repository:

```
git clone https://github.com/limo-app/limo.git
cd limo
```

#### Build libunrar:

```
git clone https://github.com/aawc/unrar.git
cd unrar
make lib
cd ..
```

#### Build Limo:

```
mkdir build
cmake -DCMAKE_BUILD_TYPE=Release -S . -B build
cmake --build build
```
 
#### (Optional) Run the tests:

```
mkdir tests/build
cmake -DCMAKE_BUILD_TYPE=Release -S tests -B tests/build
cmake --build tests/build
tests/build/tests
```

#### (Optional) Build the documentation:

```
doxygen src/lmm_Doxyfile
```

## Contributing configurations

From version 1.0.7 onwards, Limo supports specialized deployer and auto tag imports for Steam games. Each configuration is stored in
a file named *<STEAM_APP_ID>.json* in the *steam_app_configs* directory of this repository. If you are using Limo to mod a Steam
game for which for no configuration file exists or if you want to improve an existing configuration, please consider open a Pull Request.

You can export your existing configuration by clicking on the *Export* button in the *App* tab. This will generate the file *exported_config.json*
in the app's staging directory. Before creating a Pull Request, rename this file to *<STEAM_APP_ID>.json*, e.g. *489830.json* for
Skyrim SE, and move it to the *steam_app_configs* directory.

**Note**: Deploy modes in this file will default to *hard link*, even if you are using sym links. When your configuration is imported by others,
sym links will automatically used instead if hard links do not work.