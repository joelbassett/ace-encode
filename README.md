#####################################################################################################
#####################################################################################################
####                                                                                             ####
####                        ACE-Encode (DVD & BluRay Ripper/Encoder)                             ####
####                       ------------------------------------------                            ####
####                                                                                             ####
#####################################################################################################
#####################################################################################################

ACE-Encode was born from the need convert a whole CD/DVD/BluRay library (450+) into music & movie files readable on a Home Theater PC (HTPC).

ACE-Encode works by automatically ripping an inserted CD/DVD/BluRay to a folder (WARNING: Each BluRay can take up to 35+ GB of storage).

From there you can manually run (or as a CRON job) the encoder for DVD/BluRay which will convert the ripped folders to a usable (and smaller) movie file.

If you have a program such as CouchPotato, you can configure a "Black Hole" directory where CouchPotato will pick up the encoded movie, download all the appropriate METADATA and movie information and add the file to your media playing program (We use XBMC).

There are a few applications that do a similar thing, but many are limited to different Linux versions or even Windows, I'd like to think that this application can be adapted to all versions of Linux.

To make the most of all the features in this application, we recommend the installation of the following:
* Ubuntu (or some variation of Linux)
* BASH Shell
* DVDBackup
* MakeMKV
* ABCDE
* HandBrake
* XBMC (Local or Remote will work)
* CouchPotato

For help with the installation of each of these features please read the INSTALL section below.


#####################################################################################################
####                                                                                             ####
####                              Installation of ACE-Encode                                     ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

Step 1: Install GIT (if you don't already have it installed)
-------------------------------------------------------------

You may install ACE-Encode on Ubuntu by downloading the latest version from a .zip or .tar.gz and extracting it. But I recommend installing and running it from the GitHub source (to keep up with the latest features and bug fixes) using git as the updater. If you do not have git installed already, install it using the command below:

**************** BEGIN CODE: ****************
$ sudo apt-get install git-core
***************** END CODE: *****************


Step 2: Install ACE-Encode on Ubuntu
-------------------------------------

First, if you already have a _/usr/bin/ace-encode_ folder on your computer from a previous version you should remove or rename the previous folder. If your previous ACE-Encode is saved in the default installation location then from there do the following:

**************** BEGIN CODE: ****************
$ cd /usr/bin
$ sudo mv ace-encode ace-encode-old
***************** END CODE: *****************

After your new ACE-Encode installation is running and you have recovered any settings you need, you can delete the previous version’s folder (ace-encode-old).

Next, you will need to clone a copy of ACE-Encode from the master git repository:

**************** BEGIN CODE: ****************
$ cd /usr/bin
$ sudo git clone git://github.com/joelbassett/ACE-Encode.git ace-encode
***************** END CODE: *****************

This will download the current revision of ACE-Encode to the folder ace-encode. 


Step 3: Configure ACE-Encode
-----------------------------

If you have used GIT to install ACE-Encode to the above directory, start with:

**************** BEGIN CODE: ****************
$ cd /usr/bin/ace-encode
***************** END CODE: *****************

We will then need to ensure that ACE-Encode is executable by the user that you are logged in as. Replace {USERNAME} with the user you will be ripping files as:

**************** BEGIN CODE: ****************
$ sudo chmod +x ./*
$ sudo chown {USERNAME}:{USERNAME} ./*
***************** END CODE: *****************

Next we will need to change the settings for ACE-Encode:

**************** BEGIN CODE: ****************
$ nano ./settings.conf
***************** END CODE: *****************


The important settings to adjust are the following:
* GLOBAL_ENABLE_XBMC - Enable XBMC Connectivity (1 = on, 0 = off)
* GLOBAL_XBMC_HOST - XBMC Host that you wish to notify and update the library of (127.0.0.1 is the default)
* GLOBAL_ENABLE_EMAIL - Enable Email Connectivity (1 = on, 0 = off)
* GLOBAL_EMAIL - Email address to send notifications to
* GLOBAL_SOURCE_DRIVE** - The path for your DVD/BluRay drive (/dev/sr0 is default)
* GLOBAL_LOG_DIR - Directory to save the application logs (/data/Ripping/logs is default)
* ENABLE_DVD - Enable DVD Ripping & Encoding (1 = on, 0 = off)
* DVD_OUTPUT_DIR - Directory to save the Ripped DVDs in (/data/Ripping/DVD is default)
* ENABLE_BLURAY - Enable DVD Ripping & Encoding (1 = on, 0 = off)
* BLURAY_OUTPUT_DIR - Directory to save the Ripped BluRays in (/data/Ripping/BR is default)

All further settings are detailed in the CONFIGURATION section.


Step 4: Enable automatic CD/DVD/BluRay ripping
-----------------------------------------------

Step 4 takes into account that you have ABCDE, DVDBackup, MakeMKV & HandBrake installed and working correctly. For help installing any of the above, please see the INSTALLATION section for each extension.

To enable automatic ripping, first we need to create the definitions for each of the automatically started programs.

**************** BEGIN CODE: ****************
$ cd /usr/bin/ace-encode
$ sudo cp ./autorun/.abcde.conf ~
$ sudo cp ./autorun/ripcd.desktop /usr/share/applications/
$ sudo cp ./autorun/ripdvd.desktop /usr/share/applications/
$ sudo cp ./autorun/ripbluray.desktop /usr/share/applications/
$ sudo chmod +x ~/.abcde.conf
$ sudo chmod +x /usr/share/applications/ripcd.desktop
$ sudo chmod +x /usr/share/applications/ripdvd.desktop
$ sudo chmod +x /usr/share/applications/ripbluray.desktop
***************** END CODE: *****************

Next we need to open the file _mimeapps.list_:

**************** BEGIN CODE: ****************
$ sudo nano ~/.local/share/applications/mimeapps.list
***************** END CODE: *****************

And add the following at the bottom of the file:

**************** BEGIN CODE: ****************
[Default Applications]
x-content/audio-cdda=ripcd.desktop;
x-content/video-dvd=ripdvd.desktop;
x-content/video-bluray=ripbluray.desktop
[Added Associations]
x-content/audio-cdda=ripcd.desktop;
x-content/video-dvd=ripdvd.desktop;
x-content/video-bluray=ripbluray.desktop;
***************** END CODE: *****************

Save the file and then in Ubuntu go to System Settings > Details > Removable Media and then do the following:
* For CD - Click on the Audio CD drop down, you’ll notice our Rip CD program is listed. Choose this as your default. 
* For DVD - Click on the DVD Video Disc drop down, you’ll notice our Rip DVD program is listed. Choose this as your default. 
* For BluRay - Click on the Additional Settings button and find the option for Bluray Video Disc, you’ll notice our Rip BluRay program is listed. Choose this as your default. 

If all your programs have been installed and set up correctly, you should now be able to insert a disc of your choosing and it will automatically rip the music or movie to your computer.


Step 5: CD/DVD/BluRay encoding
-------------------------------

As DVD/BluRay encoding can be a very personal thing (quality vs size), I would prefer to run my encoding on demand. However the scripts are there and can be setup to run on a CRON script (If you have a large amount of encoding, I'd recommend only once a month).

When you change the VIDEO & AUDIO settings for each of the DVD & BluRay settings, I would recommend running the first pass manually to get a feel for how long each DVD/BR file will take to encode. Then based on the number of files, you can decide on which CRON job to create.

I will update this section in the future with a CRON script.

To automatically encode all ripped DVDs, use the following command:

**************** BEGIN CODE: ****************
$ sudo /usr/bin/ace-encode/dvd-encode
***************** END CODE: *****************

To automatically encode all ripped BluRays, use the following command:

**************** BEGIN CODE: ****************
$ sudo /usr/bin/ace-encode/bluray-encode
***************** END CODE: *****************


UPDATING TO LATEST VERSION
---------------------------

At a later stage, to update to a current revision of ACE-Encode, just go into _/usr/bin/ace-encode_ folder and type:

**************** BEGIN CODE: ****************
$ sudo git pull
***************** END CODE: *****************


#####################################################################################################
####                                                                                             ####
####                                 Installation of XBMC                                        ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

The following is a small excerpt from HOW-TO: Install XBMC for Linux (http://wiki.xbmc.org/index.php?title=HOW-TO:Install_XBMC_for_Linux)

In Ubuntu execute the following:

**************** BEGIN CODE: ****************
$ sudo apt-get install python-software-properties pkg-config
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:team-xbmc/ppa
$ sudo apt-get update
$ sudo apt-get install xbmc xbmc-eventclients-xbmc-send
***************** END CODE: *****************

And voila, XBMC is installed and can be started.


#####################################################################################################
####                                                                                             ####
####                              Installation of CouchPotato                                    ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

The following is a small excerpt from HOW-TO: Install CouchPotato for Ubuntu(http://www.htpcbeginner.com/install-couchpotato-on-ubuntu/)

You may install CouchPotato on Ubuntu by downloading the latest version from [here](https://github.com/RuudBurger/CouchPotatoServer) and extracting.
But I recommend running it from source (to keep up with the latest features and bug fixes) using Python with git as the updater. If you do not have python and git installed already, install them using the command below:

**************** BEGIN CODE: ****************
$ sudo apt-get install git-core python
***************** END CODE: *****************

Next, to install CouchPotato on Ubuntu, move to your home directory (or any other directory) and clone a copy of CouchPotato V2 from the git repository:

**************** BEGIN CODE: ****************
$ cd ~
$ git clone git://github.com/RuudBurger/CouchPotatoServer.git .couchpotato
***************** END CODE: *****************

This will download the contents to the folder .couchpotato. The “.” in the front keeps the .couchpotato folder hidden. That is all there is to setup CouchPotato on Ubuntu.

At a later stage, to update CouchPotato, just go into .couchpotato folder and type:

**************** BEGIN CODE: ****************
$ git pull
***************** END CODE: *****************

You may also update CouchPotato by clicking “Update to Latest” from the settings menu or click on the update notification that appears on top of the screen (shown below).

After you install CouchPotato, use the following command to run it:

**************** BEGIN CODE: ****************
$ python ~/.couchpotato/CouchPotato.py
***************** END CODE: *****************

CouchPotato V2 is now running on your system (unless you encounter any errors). Unlike CouchPotato V1 (uses port 5000), V2 runs on port 5050 by default. So you can access the CouchPotato web interface by going to:

**************** BEGIN CODE: ****************
http://localhost:5050
***************** END CODE: *****************


#####################################################################################################
####                                                                                             ####
####                                 Installation of ABCDE                                       ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

The following is a small excerpt from [The Ultimate Automated Ripping Machine](http://pathartl.me/blog/2013/12/01/the-ultimate-automated-ripping-machine/)

In Ubuntu execute the following:

**************** BEGIN CODE: ****************
$ sudo apt-get install abcde
***************** END CODE: *****************

And voila, ABCDE is installed and can be started.


#####################################################################################################
####                                                                                             ####
####                               Installation of DVDBackup                                     ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

The following is a small excerpt from The Ultimate Automated Ripping Machine (http://pathartl.me/blog/2013/12/01/the-ultimate-automated-ripping-machine/)

In Ubuntu execute the following:

**************** BEGIN CODE: ****************
$ sudo apt-get install dvdbackup
***************** END CODE: *****************

We also need to install libdvdcss to allow us to break DVD encryption. We’ll also need to install git (it’s usually the first thing I install on any machine along with vim) and clone the libdvdcss repo.

**************** BEGIN CODE: ****************
$ sudo apt-get install git
$ git clone git://git.videolan.org/libdvdcss
***************** END CODE: *****************

Now we compile and install libdvdcss

**************** BEGIN CODE: ****************
$ ./configure
$ make
$ sudo make install
$ echo '/usr/local/lib' > /etc/ld.so.conf.d/libdvdcss.conf
$ ldconfig -v
***************** END CODE: *****************

And voila, DVDBackup and libdvdcss is installed.


#####################################################################################################
####                                                                                             ####
####                               Installation of HandBrake                                     ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

The following is a small excerpt from The Ultimate Automated Ripping Machine (http://pathartl.me/blog/2013/12/01/the-ultimate-automated-ripping-machine/)

In Ubuntu execute the following:

**************** BEGIN CODE: ****************
$ sudo add-apt-repository ppa:stebbins/handbrake-snapshots
$ sudo apt-get update
$ sudo apt-get install handbrake-cli handbrake-gtk
***************** END CODE: *****************

And voila, HandBrake is installed and can be started.


#####################################################################################################
####                                                                                             ####
####                                Installation of MakeMKV                                      ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

The following is a small excerpt from [The Ultimate Automated Ripping Machine](http://pathartl.me/blog/2013/12/01/the-ultimate-automated-ripping-machine/)

In Ubuntu execute the following:

Just a little note, MakeMKV is not FOSS and does cost $50. The Beta version is currently free but can change at anytime.

We’ll need some tools to help us with compiling and installing MakeMKV

**************** BEGIN CODE: ****************
$ sudo apt-get install build-essential libc6-dev libssl-dev libexpat1-dev libavcodec-dev libgl1-mesa-dev libqt4-dev
***************** END CODE: *****************

Now download the binaries and source for MakeMKV

**************** BEGIN CODE: ****************
$ wget http://www.makemkv.com/download/makemkv-bin-1.8.6.tar.gz
$ wget http://www.makemkv.com/download/makemkv-oss-1.8.6.tar.gz
***************** END CODE: *****************

Extract

**************** BEGIN CODE: ****************
$ tar xvzf makemkv-oss-1.8.6.tar.gz
$ tar xvzf makemkv-bin-1.8.6.tar.gz
***************** END CODE: *****************

Now we’ll go and run make and make install in each directory

**************** BEGIN CODE: ****************
$ cd makemkv-oss-1.8.6
$ make -f makefile.linux
$ sudo make -f makefile.linux install
***************** END CODE: *****************

**************** BEGIN CODE: ****************
$ cd makemkv-bin-1.8.6
$ make -f makefile.linux
$ sudo make -f makefile.linux install
***************** END CODE: *****************

Cleanup time

**************** BEGIN CODE: ****************
$ rm -R makemkv*
***************** END CODE: *****************

And voila, MakeMKV and related utilities are installed and can be started.


#####################################################################################################
####                                                                                             ####
####                           Credits / Authors / Contributors                                  ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

Applications
-------------

* XBMC - http://xbmc.org/
* HandBrake - http://handbrake.fr/
* MakeMKV - http://www.makemkv.com/
* DVDBackup - http://dvdbackup.sourceforge.net/
* ABCDE - http://lly.org/~rcw/abcde/page/
* CouchPotato - https://couchpota.to/

Guides / Walkthroughs
----------------------

* The Ultimate Automated Ripping Machine - http://pathartl.me/blog/2013/12/01/the-ultimate-automated-ripping-machine/
* HOW-TO: Install XBMC for Linux - http://wiki.xbmc.org/index.php?title=HOW-TO:Install_XBMC_for_Linux


#####################################################################################################
####                                                                                             ####
####                                  Support or Contact                                         ####
####                       -----------------------------------------                             ####
####                                                                                             ####
#####################################################################################################

Having trouble with ACE-Encode? 

Please send any issues or questions through to my email - joel.bassett@gmail.com

All the best!
