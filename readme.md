Turn Web Apps into (sort of) X11 Apps
===

If you have a lot of tabs and/or use a tiling Window manager, using WebApps like Gmail, Google Docs, and so on can be painful. Even if you open a new browser window for each, they all look the same
on your taskbar. With this setup, I can have an executable for a Web app, each will have its own icon and you can set them to only create a single instance, if you like. I use this with KDE/Plasma, but
it will probably work with other X11 setups, too. Not sure about Wayland.

What you need
==
1. Chrome or a Chrome-based browser (I use Vivaldi)
2. xdotool and xseticon (optional; https://github.com/xeyownt/xseticon)
3. Ability to create simple shell scripts

What's included here?
==
1. The weblaunch script. Needs to be on your path
2. bin/xseticon - x64 precompiled binary that may or may not work for you; put in /usr/local/bin
3. Many example scripts

Example Script
==
Here's how to launch YouTube Music
   #!/bin/bash
   APP=music.youtube.com
   ICON=-
   TITLE="YouTube Music"
   exec weblaunch -1 "$APP" "$TITLE" "$ICON"

Honestly, you could wrap it all on two lines:
   #!/bin/bash
   exec weblaunch -1 music.youtube.com "YouTube Music" -
   
Or, you could create an alias. Go wild.

Command Line Arguments
==
Here's how to run weblaunch
   weblaunch [-1] app title icon [alt_title]
   
If -1 is provided, the script will not create a second copy of the app, but will activate the first copy instead. If there is no first copy, this option has no effect and the app will launch.

* The app is the https URL (without the https) like music.youtube.com. You can put more on the URL like www.google.com/mail/u/?authuser=xxx@gmail.com
* The title is the title string to match when finding the app window. Wildcards are OK and it only has to match a substring. 
* The icon is a PNG file to use as the icon. You don't need this often, because most pages you want to set up like this will set their own icon. In that case, simply pass - as the icon name.
* Supply alt_title if you want to change the titlebar to have a different title. Note that if you change the title, this is what will be used for the -1 option, too, not the original title

Required Setup
==
You will want to edit weblaunch and set BROWSER to your choice of browser executables. It must support the --app command-line switch. In addition, you need xdotool (install via package manager) and
xseticon which you may have to build or download the Snap version (see earlier GitHub link). 

Your task manager will probably not show icons nicely if you allow it to group icons by program. All the icons will stack together since they are all really your web browser. You will want to experiment with 
settings, but generally disabling stacking will work.

You will probably want to create menu entries or Desktop files for your scripts. This is not done automatically.

Special Cases
==
* If the URL or title of a web site changes, you'll need to adapt your script
* If you need to log into a web site, this may cause problems with the titles so you may have to accept that you have to log in first
* Gmail will go to your primary account. If you are logged into a secondary account, you can use a URL like mail.google.com/mail/u/>authuser=myaddress@gmail.com
* Titles need to be specific enough not to match other windows. For example, Inbox isn't a good title for gmail unless you only have one account open since they will all say Inbox.

Other Notes
==
You can use Nativefier to convert a page to an Electron app. However, this is NOT your browser so you don't get saved passwords and extensions. So, for example, no Grammarly in Gmail or Docs, which is a big problem. Using this method,
you are still in your browser so everything works the same. See https://github.com/jiahaog/nativefier.
