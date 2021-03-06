# Skyscraper by Lars Muldjord
A powerful and versatile yet easy to use game scraper written in C++ for use with multiple frontends running on a Linux system. It scrapes and caches various game resources from various web sources, including media such as screenshot, cover and video. It then gives you the option to combine all of these resources into the most complete results by using the provided '`localdb`' scraping module.

Any exported artwork can be customized completely. Check the documentation for that [here](ARTWORK.md).

#### Currently supports the following frontends (set with '-f'):
* EmulationStation
* AttractMode

#### Currently supports the following platforms (set with '-p'):
Check the full list of platforms [here](PLATFORMS.md).

#### Currently supports the following scraping sources (set with '-s')
* WEB: openretro.org
* WEB: thegamesdb.net
* WEB: worldofspectrum.org
* WEB: adb.arcadeitalia.net (Arcade Database by motoschifo, arcadedatabase@gmail.com, [youtube](https://www.youtube.com/c/ArcadeDatabase))
* WEB: screenscraper.fr
* LOCAL: localdb (scrapes exclusively from cached resources. Read more [here](#local-database-features))
* LOCAL: import (imports resources into the local cache. Read more about this [here](import/README.md))

... More scraping sources will be added in future releases!

## How to install Skyscraper
Follow the steps below to install the latest version of Skyscraper. Lines beginning with '`$`' signifies a command you need run in a terminal on the machine you wish to install it on.

### Install prerequisites
Skyscraper needs Qt5.3 or later to compile. For a Retropie, Ubuntu or other Debian derived distro, you can install it using the following commands:
```
$ sudo apt-get update
$ sudo apt-get install qt5-default
```
You might be asked for your sudo password. On RetroPie the default password is '`raspberry`'. To install Qt5 on other Linux distributions, please refer to their documentation.

### Download, compile and install
When you've installed the prerequisites as described above, you can install Skyscraper by typing in the following commands:
```
$ cd
$ mkdir skysource
$ cd skysource
$ curl https://raw.githubusercontent.com/muldjord/skyscraper/master/update_skyscraper.sh | bash
```
The last command will download and run the latest update script from Github. During the installation you might be asked for your sudo password. On RetroPie the default password is '`raspberry`'.

When the script has completed you are ready to run Skyscraper!

### Updating Skyscraper
From Skyscraper 2.3.2 and newer you can update to the latest version simply by running the following commands:
```
$ cd
$ cd skysource
$ ./update_skyscraper.sh
```
You might be asked for your sudo password during the update. On RetroPie the default password is '`raspberry`'. If your version is older than 2.3.2 (check with '`--help`') you need to follow the [installation instructions](#download-compile-and-install) instead.

## How to use Skyscraper
IMPORTANT!!! In order for Skyscraper to work properly, it is necessary to quit your frontend before running it! If you're running EmulationStation, you can quit it by pressing F4.

Remember, you can completely customize the artwork Skyscraper exports. Check out the documentation [here](ARTWORK.md). If you just want to use the default (pretty cool looking) artwork Skyscraper provides, read on.

### Simple mode
When you have completed the installation you can start Skyscraper in 'Simple mode' by running Skyscraper with no command line options by typing:
```
$ Skyscraper
```
Skyscraper will then ask you a bunch of questions, create an optimized script based on your answers, and finally run the script which scrapes the chosen platform in an optimal way. This is very useful for first time scrapings, as it will give you the best possible initial result for any given platform. If you're curious you can check out the generated script afterwards. It's located in '`[homedir]/.skyscraper/skyscript.sh`'.

### Manual mode (for advanced users)
Skyscraper is a command line tool, and has many, many options for you to fiddle around with. I recommend taking a look at all of them to familiarize yourself with the possibilites:
```
$ Skyscraper --help
```
This will give you a description of everything Skyscraper can do if you feel adventurous!

NOTE: If you have already scraped a platform using 'Simple mode', any subsequent scrapings of that particular platform should be done with:
```
$ Skyscraper -p [platform]
```
This will scrape the platform using the '`localdb`' scraping module, and will make use of all the resources you have gathered in the local database cache. A full description of the '`localdb`' scraping module can be found [here](#local-database-features). You can of course add further command line options if needed.

NOTE: To enable video scraping for the scraping modules that support it, you need to add the '`--videos`' command line option. This is disabled per default because of the significant space requirements needed to save them.

### config.ini
A lesser know, but extremely useful, feature of Skyscraper is to add your desired config variables to '`[homedir]/.skyscraper/config.ini`'. Any options set in this file will be used per default by Skyscraper. So if you always use, for example, '`-m 100`' on command line, you can set the matching option '`minMatch="100"`' in the config.

Many options can be set on two levels; either `[main]` or `[platform]`. Platform can be changed to any of the supported platforms (check list with '`--help`'), in which case the settings will only be applied while scraping that particular platform. Settings in the `[main]` section will always be used.

You can find an example config file at '`[homedir]/.skyscraper/config.ini.example`'. This file contains all available options. Just uncomment the ones you wish to use by removing the "`#`" in front of the variables.

### Local database features
Whenever you scrape any platform with any web scraping module, Skyscraper caches each resource locally. A resource can, for instance, be a game '`description`' or a game '`screenshot`'. Each game can have several versions of each resource cached locally. One of each type per web scraping module. This comes in handy when using the '`localdb`' scraping module.

#### LocalDb scraping module
After a while you'll have accumulated a decent amount of locally cached data for any given platform. To exclusively make use of this data Skyscraper provides the '`localdb`' scraping module (set with '`-s localdb`'). By using this source Skyscraper *only* scrapes from the locally cached data. Depending on how many sources you've scraped any given platform with, the 'localdb' module will give you almost perfect results, with almost no data missing. Per default any resource type is prioritized by timestamp. But it is also possible to prioritize them by scraping source. So if you prefer the '`description`' results from a certain scraping module, you can easily make sure that these will be prioritized above any other descriptions available. Read more about how to do this [here](dbs/README.md).

#### Update local data
If you wish to update / refresh the locally cached resources for a particular platform and scraping module, Skyscraper provides the '`--updatedb`' option. If this flag is set on the command line, any data in the local cache will be updated with the new incoming data.

If you wish to just refresh the data for a single rom simply scrape it with '`-p [platform] -s [scraping module] --updatedb [relative or full rom path and filename]`' and the locally cached data for that particular rom will be updated / refreshed. You can add more filenames one after the other if you like. If any filename or paths has spaces in it, remember to double-quote it like so `"relative path/to rom/rom filename.sfc"`.

When you've updated information in the local cache, always remember to rescrape the entire platform with 'Skyscraper -p [platform] -s localdb' afterwards to regenerate the gamelist for the frontend.

#### Default db folder
The default folder for all of Skyscrapers' locally cached data is in the '`[homefolder]/.skyscraper/dbs`' subfolder. In this folder you'll find the individual platform db subfolders. Any platform db folder is selfcontained and can be copied to a USB drive, or zipped up and uploaded to share with friends.

#### User-defined databases
Normally Skyscraper uses a default local db folder for each platform. But a friend might have send you a copy of his local database folder, and you wish to scrape from his data. In this case Skyscraper allows you to force the use of a local database with the '`-d [db folder]`' command line option. Keep in mind that if your friend has zipped the db folder for convenience, you need to unzip it before use. Skyscraper does *not* currently support zipped db folders.

#### Tiny words of warning
If you start copying your local databases to and from friends, or you accumulate some really big local databases that you sleep with at night because you love them so much - ALWAYS remember to back these up from time to time! Skyscraper is software. Software has bugs. And even though I do quite a bit of testing and feel confident in my code, bugs are inevitable from time to time.

Basically what I'm trying to say is that it is entirely your own fault if you've spent 6 months creating a bunch of local db's and suddenly you overwrite them unintentionally or Skyscraper corrupts the data for some i-have-no-idea-how reason. It could happen. So... PLAN YOUR BACKUPS! And don't come crying to me. :D

### Local data import
I addition to allowing scraping from local resources, Skyscraper also allows you to import your own data into the local cache, which in turn allows you to scrape your roms with it using the '`-s localdb`' scraping module. For a quick overview read on below. For a more detailed description with examples go [here](import/README.md).

#### Artwork import
Skyscraper allows you to import various artwork resources from the local '`[homedir]/.skyscraper/import`' folders. Simply place your data inside these folders with the EXACT filename of the roms you wish to connect them to. For instance, if you have a rom called '`Bubble Bobble.nes`' you would place your screenshot for this rom inside '`[homedir]/.skyscraper/import/screenshots`' called '`Bubble Bobble.png`'. Other image file formats are also supported.

#### Textual data import
For textual data, you need to first create a file called '`[homedir]/.skyscraper/import/definitions.dat`'. In this file, you must define the file content format you are providing for each rom. For instance, if your data comes in the form of 1 xml file per rom, and you wish to scrape '`publisher`' for this rom, perhaps your input file has a node like '`<publisher>This is the publisher</publisher>`'. In the '`definitions.dat`' file you'd then add a line looking like '`<publisher>###PUBLISHER###</publisher`'. The '`###PUBLISHER###`' tag is recognized by Skyscraper.

Now run the scraper with the '`-s import`' option:
```
$ Skyscraper -p [platform] -s import
```
If you've named the files correctly, the game will show with a green '`YES`' for cover, screenshot, wheel, marquee and/or video (if you've enabled them with the '`--videos`' option). If you've imported textual data, it will show the data at the relevant output line.

Now, to make use of the imported data, scrape with the '`-s localdb`' scraping module and your resources will be prioritized above all others as defined in '`[homedir]/.skyscraper/dbs/[platform]/priorities.xml`'.
```
$ Skyscraper -p [platform] -s localdb
```
Then start your frontend and enjoy your newly imported data. :)

### Artwork look and effects
Check the full artwork documentation [here](ARTWORK.md)

## Release notes

#### Version x.x.x (still unimplemented)
* Added 'sharpen' effect which sharpens the image
* Improved 'blur' and 'shadow' effect to be true gaussian
* Added option to --purgedb to purge all resources not related to your current romset
* Added option to --purgedb to purge everything completely
* Added 'mobygames' scraping module. Limited to 360 requests per hour so won't be included in 'Simple Mode'. Can be used to scrape a few games that other sources miss.
* Made 'import' base folder configurable
* Added 'Did you know' hints when running Skyscraper
* Added '--nohints' to disable hints. Can also be set in config.ini
* Add 'autocrop' to screenshots, just like I already do with wheels. Thank you to 'chipsnblip' for pointing this out

#### Version 2.5.3 (24th July 2018)
* Added '--allowext' option which will force allow a file extension for the given platform. Thank you to 'herbymachine' for suggesting this
* Added 'allowExtension' to the '[homedir]/.skyscraper/config.ini' variables for both 'main' and 'platform' specific sections. This is useful if you wish to permanently add a file extension to all or one platform when scraping
* Implemented 'developers' change in 'thegamesdb' API
* Implemented 'publishers' change in 'thegamesdb' API
* Fixed 'Tags' bug in 'screenscraper' module

#### Version 2.5.2 (13th July 2018)
* Fixed bug in 'thegamesdb' module that broke platform and genre detection

#### Version 2.5.1 (10th July 2018)
* Added '--relative' option which forces rom relative paths in gamelist for EmulationStation

#### Version 2.5.0 (8th July 2018)
* 'thegamesdb' removed from Simple Mode scraping scripts due to new api restrictions
* Implemented new 'thegamesdb' api, be wary of monthly request limit!
* Made sure 'remaining requests' is clearly stated when using 'thegamesdb'
* Implemented request limit test which makes Skyscraper stop if limit is reached
* Made sure cli header always has correct number of dashes

#### Older releases
Release notes for older releases can be found [here](OLDERRELEASES.md).