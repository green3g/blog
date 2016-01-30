# How to install a mp495 Canon printer on Linux OpenSUSE 13.1

Sometimes installing printers on linux has been pretty troublesome, especially when native support isn't included out of the box, but this time around, things were really simple and I wanted to share that with anyone who may find it useful.

This guide is specifically how I installed the Canon PIXMA MP 495 on my OpenSUSE 13.1 64 bit edition. Also, I am using the Gnome spin of OpenSUSE. To begin with, I already had the mp495 connected to the network. I had previously used a Windows machine and the provided software to accomplish this. Alternatively, Canon Australia provides Ubuntu packages to configure this.

First, I needed the cups-bjnp library. This is available from a quick Google Search, or here at the time of this writing:

http://sourceforge.net/projects/cups-bjnp/

To install this library, I also needed to install cups-devel, the development cups library.

sudo zypper in cups-devel

Next we will extract, configure, and install the cups-bjnp library.

cd ~/Downloads #or wherever your downloaded tar is.
 tar -xfv cups-bjnp-1.2.2.tar
 cd cups-bjnp-1.2.2
 ./configure


if you didn't get any errors in the previous step, proceed, otherwise do some research to find out what went wrong, if all went well, type:

sudo make install

If everything went well, which it did for me, open YAST from the Activities. Choose Printers, then Add +.

Choose Connection Wizard, Special, Sepcify Arbitrary Device URI.

Type bjnp://your-printer-ip-address into the first URI field.

Choose Canon for the second field.

Search for Canon MP495 in the Find and Assign Driver, choose the first option. Click OK, and you should be able to print a test page.

Not so bad, right?
