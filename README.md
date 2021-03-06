# Skyscraper by Lars Muldjord
A powerful and versatile yet easy to use game scraper written in C++ for use with multiple frontends running on a RetroPie system. It scrapes various game resources from various web sources, including media such as screenshot, cover and video.

Currently supports the following frontends (set with '-f'):
* EmulationStation
* AttractMode

Currently supports the following platforms (set with '-p'):
* Amiga (OCS, ECS, AGA, CD32, CDTV)
* Apple 2
* Arcade
* Atari2600
* Atari5200
* Atari7800
* Atari Jaguar
* Atari ST
* ColecoVision
* Commodore 64
* Game Boy
* Game Boy Advance
* Game Boy Color
* Megadrive / Genesis
* MSX (MSX, MSX 2, MSX 2+, MSX Laserdisc)
* NeoGeo
* Nintendo 64
* Nintendo DS
* Nintendo Entertainment System
* PC-Engine / TurboGrafx-16
* Playstation
* Playstation Portable
* Sega CD
* Sega Game Gear
* Sega Master System
* Super Nintendo
* ZX Spectrum

... More platforms will be added in future releases!

Currently supports the following scraping sources (set with '-s')
* WEB: openretro.org
* WEB: thegamesdb.net
* WEB: worldofspectrum.org
* WEB: adb.arcadeitalia.net (Arcade Database by motoschifo, arcadedatabase@gmail.com, https://www.youtube.com/c/ArcadeDatabase)
* WEB: screenscraper.fr
* LOCAL: localdb (scrapes exclusively from cached resources, read more under "Local database features")
* LOCAL: import (imports resources located in '[homedir]/.skyscraper/import' into the local database. Read more under "Local data import")

... More scraping sources will be added in future releases!

## Note for Amiga users
For Amiga I STRONGLY recommend you to set up your RetroPie to use WHDLoad. Follow this guide:
* http://www.ultimateamiga.co.uk/HostedProjects/RetroPieAmiga/guide.html

## Prerequisites
Install this package:
* $ sudo apt-get install qt5-default
* [enter your 'pi' user password, default is 'raspberry']

## Building and running
Create a folder for the Skyscraper source, download the latest release, compile it and install it:
* $ cd /home/pi
* $ mkdir sources
* $ cd sources
* $ wget https://github.com/muldjord/skyscraper/archive/2.0.2.tar.gz
* $ tar xvzf 2.0.2.tar.gz
* $ cd skyscraper-2.0.2
* $ qmake
* $ make
* $ sudo make install
* [enter your 'pi' user password, default is 'raspberry']

If everything went well you are now ready to run Skyscraper!

## Usage
IMPORTANT!!! In order for Skyscraper to work properly, it is necessary to quit your frontend before running it! If you're running EmulationStation, you can quit it by pressing F4.

Now, I recommend taking a look at the command line options first:
* $ Skyscraper --help

This will give you a description of everything Skyscraper can do if you feel adventurous!

However, Skyscraper was designed to work with default options. If you're using the EmulationStation frontend, basically all you need to do is type:
* $ Skyscraper -p [platform]

Where [platform] must be one of the supported platforms (check '--help' for a list). This will scrape using the default scraping module for that platform.

NOTE: To enable video scraping for the scraping modules that support it, you need to add the '--videos' command line option. This is disabled per default because of the significant space requirements needed to save them.

I recommend scraping with different scraping modules one after the other with the '-s' option:
* $ Skyscraper -p [platform] -s [scraper]

This will scrape using the specified scraping module instead of the default one. Every time you scrape with a new scraping module, all resources from that module will be cached locally. After you've scraped with different scraping modules, always rescrape with:
* $ Skyscraper -p [platform] -s localdb

Scraping with localdb will combine all of your locally cached resources into the most complete results. Remember to always overwrite your gamelist and NOT skip existing entries, unless you have a specific reason to do so. Read on for a more thorough description of localdb and how to prioritize the results.

### Local database features (1.6.0 and onwards)
Whenever you scrape any platform with any web scraping module, Skyscraper caches each resource locally. A resource can, for instance, be a game 'title' or a game 'screenshot'. Each game can have several versions of each resource cached locally. One of each type per web scraping module. This comes in handy when using the 'localdb' scraping module.

#### LocalDb scraping module
After a while you'll have accumulated a decent amount of locally cached data for any given platform. To exclusively make use of this data Skyscraper provides the 'localdb' scraping module (set with '-s localdb'). By using this source Skyscraper *only* scrapes from the locally cached data. Depending on how many sources you've scraped any given platform with, the 'localdb' module will give you almost perfect results, with almost no data missing. Per default any resource type is prioritized by timestamp. But it is also possible to prioritize them by scraping source. So if you prefer the 'description' results from a certain scraper, you can easily make sure that these will be prioritized above any other descriptions available. Read more about how to do this [here](dbs/README.md).

#### Update local data
Normally the locally cached data is persistent. This means that it will only allow one instance of any type of resource for any rom per scraping source. If you later wish to update the resources for a certain source, Skyscraper provides the '--updatedb' option. If this flag is set on the command line, any data in the local cache will be updated with the new incoming data. So if rom X has a description that you feel is lacking, and you've noticed that the data from a specific scraping module is more to your liking, simply rescrape the platform with '-s [scraping module] --updatedb' and the locally cached data will be updated. Then prioritize it to make use of it with '-s localdb'. Read more about how to do this [here](dbs/README.md).

#### Default db folder
The default folder for all of Skyscrapers' locally cached data is in the '[homefolder]/.skyscraper/dbs' subfolder. In this folder you'll find the individual platform db subfolders. Any platform db folder is selfcontained and can be copied to a USB drive, or zipped up and uploaded to share with friends.

#### User-defined databases
Normally Skyscraper uses a default local db folder for each platform. But a friend might have send you a copy of his local database folder, and you wish to scrape from his data. In this case Skyscraper allows you to force the use of a local database with the '-d [db folder]' command line option. Keep in mind that if your friend has zipped the db folder for convenience, you need to unzip it before use. Skyscraper does *not* currently support zipped db folders.

#### Tiny words of warning
If you start copying your local databases to and from friends, or you accumulate some really big local databases that you sleep with at night because you love them so much - ALWAYS remember to back these up from time to time! Skyscraper is software. Software has bugs. And even though I do quite a bit of testing and feel confident in my code, bugs are inevitable from time to time.

Basically what I'm trying to say is that it is entirely your own fault if you've spent 6 months creating a bunch of local db's and suddenly you overwrite them unintentionally or Skyscraper corrupts the data for some i-have-no-idea-how reason. It could happen. So... PLAN YOUR BACKUPS! And don't come crying to me. :D

### Local data import
I addition to allowing scraping from local resources, Skyscraper also allows you to import your own data into the local cache, which in turn allows you to scrape your roms with it using the '-s localdb' scraping module.

#### How it works
Skyscraper allows you to import various rom resources from the local '[homedir]/.skyscraper/import' folders. Simply place your data inside these folders with the EXACT filename of the roms you wish to connect them to. For instance, if you have a rom called 'Bubble Bobble.nes' you would place your snap for this rom inside '[homedir]/.skyscraper/import/snaps' called 'Bubble Bobble.png'. Other image file formats are also supported.

Now run the scraper with the '-s import' option:
* $ Skyscraper -p [platform] -s import

If you've named the files correctly, the game will show with a green 'YES' for cover/boxart, screenshot/snap and video. If you've imported textual data, it will show the data at the relevant output line.
Now, to make use of the imported data, scrape with the '-s localdb' scraper and your resources will be prioritized above all others, as defined in '[homedir]/.skyscraper/dbs/[platform]/priorities.xml'.
* $ Skyscraper -p [platform] -s localdb

Then start your frontend and enjoy your newly imported rom data. :)

#### Textual data import
For textual data, you need to first create a file called '[homedir]/.skyscraper/import/definitions.dat'. In this file, you must define the file content format you are providing for each rom. For instance, if your data comes in the form of 1 xml file per rom, and you wish to scrape 'publisher' for this rom, perhaps your input file has a node like '```<publisher>This is the publisher</publisher>```'. In the 'definitions.dat' file you'd then add a line looking like '```<publisher>###PUBLISHER###</publisher```'. The '```###PUBLISHER###```' tag is recognized by Skyscraper. Read a more detailed description with examples [here](import/README.md).

### Artwork look and effects for EmulationStation
Skyscraper allows you to fully customize how you want the final artwork to appear and what effects should be applied. Check the 'artwork' section in 'config.ini.example' for a full list of available options. I've also created some artwork examples to get you started. Try appending one of the following lines to your Skyscraper command line options.

#### Default: Small cover with drop shadow, larger screenshot
![Small cover with drop shadow, larger screenshot](https://raw.githubusercontent.com/muldjord/skyscraper/master/artwork_examples/Bubble%20Bobble.png)

#### Big cover with drop shadow, small screenshot
Append command line option '-c config_artwork01.ini'
![Big cover with drop shadow, small screenshot](https://raw.githubusercontent.com/muldjord/skyscraper/master/artwork_examples/P.P.%20Hammer.png)

#### Big centered cover with drop shadow, no screenshot
Append command line option '-c config_artwork02.ini'
![Big centered cover with drop shadow, no screenshot](https://raw.githubusercontent.com/muldjord/skyscraper/master/artwork_examples/Alfred%20Chicken.png)

#### Big centered screenshot with drop shadow, no cover
Append command line option '-c config_artwork03.ini'
![Big centered screenshot with drop shadow, no cover](https://raw.githubusercontent.com/muldjord/skyscraper/master/artwork_examples/North%20%26%20South.png)

## Release notes

#### Version 2.0.2 (4th October 2017)
* Updated 'arcadedb' result parsing to fit new format
* Now scrapes 'msx' platform families correctly with the 'screenscraper' module
* Optimized file checksumming
* Fixed a bug in 'screenscraper' module not allowing platforms with multiple platform name to be recognized. (thanks to 'qqplayer' for pointing this out!)

#### Version 2.0.1 (15th September 2017)
* Removed 'mamedb' source files as they were no longer in use
* Slightly changed help text for scraping modules
* 'thegamesdb' now properly uses Qt's XML parser
* 'screenscraper' now properly uses Qt's XML parser
* Started implementing region and lang support for 'screenscraper', but still not enabled

#### Version 2.0.0 (7th September 2017)
* Back to basics: Removed several web sources. Now only allows the ones I have explicit permission to use.
* Properly implemented official API for 'arcadedb' module
* Added scraping module info to output per result but only when using '--verbose'
* Added check for unreasonably bad scraping runs, making Skyscraper exit if 30 of 30 files miss from the get-go

### Older unsupported releases (no longer available and don't ask for them)

#### Version 1.8.2 (27th August 2017)
* Added support for 'coleco' platform
* Added support for 'pcengine' platform
* Added zip support to all platforms per request from users. :)

#### Version 1.8.1 (22nd August 2017)
* Added 'rating' scraping to 'thegamesdb'

#### Version 1.8.0 (14th August 2017)
* Added 'arcadedb' scraper module *with* video support
* Vastly improved scraping of 'neogeo' and 'arcade' platforms in general by mapping the filenames to real names from mameMap.csv
* Improved 'neogeo' and 'arcade' search platform matching

#### Version 1.7.4 (10th August 2017)
* Added textual import with 'import' scraper using '[homedir]/.skyscraper/important/definitions.dat' file
* Added video import with 'import' scraper
* Improved 'uvlist' description scraping
* Now properly handles empty nodes in EmulationStation gamelist.xml export

#### Version 1.7.3 (8th August 2017)
* Added 'developer' support for 'uvlist' scraper
* Improved html unescaping a lot
* Cleaned up xml escaping
* Added 'import' scraper, scraping from resources located in '[homedir]/.skyscraper/import' folder

#### Version 1.7.2 (7th August 2017)
* Added 'uvlist' scraping module
* Added rating resource and support
* Added rating support to lemonamiga
* Added rating support to lemon64
* Added rating support to mobygames

#### Version 1.7.1c (7th August 2017)
* Fixed make install

#### Version 1.7.1 (7th August 2017)
* Moved all source files to 'src' folder
* '[homedir]/.skyscraper' is now default folder for all files used by Skyscraper
* '/usr/local/bin/Skyscraper' is now default location for Skyscraper executable
* Refined '--help' output a bit
* Fixed lemon64 scraping
* Added 'lemonamiga' scraping module
* Added '--skipped' command line option
* Added 'make install' for correct installation of files

#### Version 1.7.0 (5th August 2017)
* MAJOR: Fixed and refined 'attractmode' frontend implementation, now works in a basic manner
* 'attractmode' can now skip existing entries
* 'emulationstation' now properly add brackets to 'name' on skipped entries
* Added check for 'db.xml' when doing '--cleandb'
* Refactored GameEntry variables
* Changed GameEntry from struct to class
* Added 'Overall title similarity' to final output
* Added 'Overall completeness' to final output
* Code refactoring here, there and everywhere
* Now accepts results where we have low editDistance, but high similarity (For instance "Disney's Darkwing Duck" with fileName "Darkwing Duck").
* Added '--nobrackets' option that disables and [] and () tags in the frontend game titles. (Thanks for the feedback 'incunabula')
* Fixed bracket parsing
* Now always uses completeBaseName since some filenames have more than one '.'
* Completely rewrote sorting algorithm. 30 lines became one with a nifty C++11 lambda :D
* Added zip format to GameGear and MSX platforms
* Now uses filenames for output image files again

#### Version 1.6.0 (1st August 2017)
* Now allows more resources of same type, as long as 'source' differs
* Now allows user to set priorities for local resource sources
* Fixed a bug that would nullify timestamp of local resources
* Optimized LocalDb communication to improve scraping speed
* Added README.md to dbs subfolder
* Added priorities.xml.example file to dbs subfolder. Automatically copies this to new databases when they are created if none already exists.
* Implemented '--cleandb' command line option that removes files with no resource entry
* Implemented '--mergedb' command line option that merges two local databases together
* Now no longer does sha1 for roms bigger than 50 MBs (Pi runs out of ram when reading them). Instead does sha1 on filename for those special cases.
* Removed default platform when scraping. You are now forced to put in a valid platform with '-p [platform]'
* Added more initial info when running Skyscraper
* Added '--unattend' command line option
* Added 'source' attribute to local database resources
* Removed 'mobygames' descriptions from 'openretro' scraper. Now uses native descriptions.
* Improved cover and screenshot scraping for 'openretro' module
* Disabled filling in missing data when scraping from web sources. User is meant to use 'localdb' scraping module for this.
* Implemented date formats to standardize output and better support EmulationStation requirements

#### Version 1.5.0 (27th July 2017)
* MAJOR: Added support for local database resources
* MAJOR: Added support for video scraping (currently supported in the 'screenscraper' scraping module)
* MAJOR: Added 'localdb' scraping module
* Added video tag in EmulationStation gamelist.xml output. Beware though, the Pi's are having a difficult time showing the videos properly.
* Added several new command line options relevant to the new video and localdb features
* Added cover, screenshot and video as part of the result output with "YES" or "NO" depending on whether they were found or not
* Fixed a bug where image tag in gamelist.xml had wrong path when using non-default path
* Now uses rom or filename (for .uae) sha1 for image filename, in case people have several roms with the same name under subdirs
* Added 'players' scraping for 'mobygames' module and improved screenshot getter even more

#### Version 1.1.8b (24th July 2017)
* Implemented a slightly hacky fix that removes some (but not all) warnings caused by a known Qt bug
* Improved final sorting algorithm
* Code refactoring / polishing

#### Version 1.1.8a (23rd July 2017)
* Hardcoded screenscraper devid and password, since this seems to be the right way of doing it

#### Version 1.1.8 (23rd July 2017)
* Refactored internal config handling to be MUCH cleaner
* Fixed bug in xml escaping
* Fixed the final sorting algorithm so it no longer outputs double entries

#### Version 1.1.7 (21st July 2017)
* Added '--nosubdirs' command line argument to exclude subdirs when scraping
* Slightly changed screen output colors and text
* Implemented generic AbstractFrontend class
* Added support for multiple frontends set with '-f'
* Changed default scraper for certain platforms based on new stats

#### Version 1.1.6 (20th July 2017)
* Corrected max threads from 4 to 8 in command line help text
* Fixed default config value bug

#### Version 1.1.5 (20th July 2017)
* Added 'msx' platform support
* Added 'psp' platform support
* Added possibility for one input platform to match many different scraper source platforms

#### Version 1.1.4 (20th July 2017)
* Added 'atarist' support
* Improved mobygames getScreenshot function

#### Version 1.1.3 (19th July 2017)
* Improved sorting by always moving "The" to end of game title

#### Version 1.1.2 (19th July 2017)
* Upped allowed number of threads to 8 (Wrooooooom!!!)
* Fixed a nasty race condition related to config file access that caused regular crashes
* Refactored config default loading to be easier to understand
* Fixed a silly memory hog that caused memory to be eaten up (not a leak, just really silly)

#### Version 1.1.1 (18th July 2017)
* Now allows user to skip existing entries
* Now allows user to set max description length with '-l'
* Refactored thread result communication to be much cleaner

#### Version 1.1.0a (16th July 2017)
* Fixed a bug where Skyscraper would get caught in an endless loop when scraping 'amiga' using the 'openretro' scraping module.

#### Version 1.1.0 (14th July 2017)
* Now quits if GamesDatabase has blocked you.
* Added support for user ID and KEY when using scrapers that require it
* Added 'screenscraper' scraping module
* Mobygames scraping module no longer truncates descriptions
* OpenRetro scraping module no longer truncates descriptions

#### Version 1.0.1 (4th July 2017)
* OpenRetro now always removes '[AGA]' from search results, since it will be appended later
* Now supports multitags of both the '()' and '[]' variety
* Now properly removes html tags from game descriptions in various scraper modules
* Fixed situations where using OpenRetro would result in a few blank covers

#### Version 1.0.0 (3rd July 2017)
* Added 'worldofspectrum' scraping module (Have fun, Dom! :D)
* Now also handles filename parenthesis comments (eg. '(disc 1 of 2)')
* Now properly handles number of threads if number of files are less than allowed threads
* Changed default scraping module for a number of platforms based on stats

#### Version 0.9.9 (2nd July 2017)
* Added 'gamesdatabase' scraping module
* Now detects if a game is a sequel and pays more attention to it when looking for matches
* Now properly appends tag such as [AGA] back into the title name when writing the xml
* Added '--stats' command line option for exporting platform scraping stats
* Changed default scraper for a bunch of platforms based on stats
* Added support for the 'apple2' platform
* Added support for the 'atari5200' platform
* Added support for the 'atarijaguar' platform
* Added support for the 'gb' platform
* Added support for the 'gbc' platform
* Added support for the 'n64' platform
* Added support for the 'nds' platform
* Added support for the 'segacd' platform

#### Version 0.9.8 (29th June 2017)
* Added MameDB scraper module
* Added support for the 'neogeo' platform
* Added support for the 'arcade' platform
* Added support for the 'atari2600' platform
* Added support for the 'atari7800' platform
* Added support for the 'gamegear' platform
* Added support for the 'mastersystem' platform
* Added 'Estimated time' to output
* Redesigned thread initialization
* FINALLY found and fixed memory leak :) Verified with 'ps'

#### Version 0.9.7a (27th June 2017)
* Probably fixed a memory leak... I hope. :S

#### Version 0.9.7 (26th June 2017):
* Artwork is now fully customizable (check 'config.ini.example')
* Added artwork dropshadow effect
* Added artwork config examples
* Added support for megadrive / genesis

#### Version 0.9.6 (25th June 2017):
* HOL scraping module added

#### Version 0.9.5 (23rd June 2017):
* Now properly supports config file setup using '-c' (see 'config.ini.example')
* Now allows user to force a certain scraper module using '-s'
* MobyGames scraping support added
* Added support for psx, nes, snes and zxspectrum
* Now provide 'config.ini.example'

#### Version 0.9.4 (21st June 2017):
* Lemon64 scraping support added (now default for 'c64')

#### Version 0.9.3 (19th June 2017):
* Fixed major bug in Amiga scraping which caused it to skip more than half :S

#### Version 0.9.2 (19th June 2017):
* Fixed minor error in command line descriptions
* Now prints chosen platform when starting

#### Version 0.9.1 (19th June 2017):
* Removed 'unpublished' command line option, as it was too specific

#### Version 0.9.0 (19th June 2017):
* Now supports both Amiga and C64 scraping. Automatically chooses best scraper for each platform
* Modularized scraping definitions
* TheGamesDb scraping support added (default for 'c64')
* OpenRetro + MobyGames (for descriptions) scraping support added (default for Amiga)

#### Version 0.8.0 (15th June 2017):
* Added threaded scraping! Set number of threads with "-t" command line option. Check "--help" for more info

#### Version 0.7.2 (12th June 2017):
* Added options for gamelist and images folder

#### Version 0.7.0 (11th June 2017):
* First public release
