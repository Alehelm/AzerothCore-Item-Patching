# AzerothCore-Item-Patching

This is a brief guide for editing an item to use a different displayID on an AzerothCore server, where a DBC patch is required.

Tools required:
DBCUtil - http://www.mediafire.com/file/bcc08c9a14viq9w/DBCUtil.rar/file

CSVed - https://www.google.com/search?client=firefox-b-d&q=CSVed

MPQEdit - http://www.zezula.net/en/mpq/download.html

WowModelViewer - https://wowmodelviewer.net/ (download the Cataclysm version)

Files required:
DBC Data Files - https://github.com/wowgaming/client-data/releases/

Intro:
If you wish to use a particular model for an item, perhaps an item which was created but never used on an obtainable item in the game, there are a number of steps you must follow to use this. The major point here is that the model, while existing in the data files, has not been assigned to a DisplayID. This guide will show you how to obtain the pathway to your desired model, create a DisplayID and then add this DisplayID to your item.

In order for the change to show and take effect, you must create a Patch for the client-side. This means that if you wish for other players to be able to see the changes, other than yourself, they must also have the patch file present in their client folder.

Step 1: Convert the DBC file to a CSV
In order to edit the DBC file (the client-side data) you need to first convert the .dbc file to a CSV file. This allows editing the table inside the file using a CSV editor. To do this:
  1. Download/Extract the DBCUtil linked above
  2. Find the .dbc file called ItemDisplayInfo.dbc in your AzerothCore data download
  3. Copy this file into the DBCUtil folder, then drag the file ONTO the DBCUtil.exe
  4. A command prompt will appear, before vanishing, and you will now have a new file called ItemDisplayInfo.dbc.csv
  5. Open CSVEd, then open this file in the program

Step 2: Find the name of the Model or Texture you wish to use
This can be a slightly tricky process which requires some investigation, from what I can see. The best way to do this is to use WoWModelViewer. You can browse all available models in the game, and find out their names in order to insert these into the DBC file. Remember that there are both Models, and Textures. A Model is the basically the shape of the item or model. The Texture dictates its color and appearance. For example, the Heroic Tier 10 DK Helm Model is 'helm_plate_raiddeathknight_h_01' and the texture is 'helm_plate_raiddeathknight_h_01red'. Once you know the name of the model, or texture, record it and move to the next step.

Step 3: Edit the DBC file
In the ItemDisplayInfo.dbc.csv file you have open in CSVEd, you need to locate the DisplayID you wish to edit. In the above example, the DisplayID for the Heroic Tier 10 DK Helm is 64587. To find the DisplayID of an item, look up that item in your database using HeidiSQL or Keira3.
  1. Using Ctrl + F to search, search for the DisplayID in Column 1.
  2. If you wish to change the Model, alter the entry in Column 2 to correspond to the Model name you located above using ModelViewer.
  3. If you wish to change the Texture, alter the texture column(s) required by the individual item you are editing (You can find complete info on which column corresponds to which Textures here: https://mangoszero-docs.readthedocs.io/en/latest/file-formats/dbc/itemdisplayinfo.html )
  4. In the above example, only Column 4 would need to be altered - 'helm_plate_raiddeathknight_h_01red'. This could be altered to 'helm_plate_raiddeathknight_h_01yellow' for example.
  5. Once you have made your changes, Save the CSV file using File -> Save

Step 4: Convert the .dbc.csv file back to a .dbc
This process is the same as Step 1, however this time you will drag the .dbc.csv file onto the executable. Make sure you remove the original .dbc file you converted before doing this or you will have a file-name conflict.

Step 5: Create a .mpq file to Patch the core
Changes to the World of Warcraft client can be achieved by 'Patching'. This involves editing the DBC files (as we have already done) and then placing them in a .mpq archive file. To do this:
  1. Download/extract MPQEdit linked above
  2. Create a folder in the extracted directory called 'Modified'
  3. Create a folder in this folder called DBFilesClient
  4. Open MPQEditor.exe
  5. Go MPQs -> New MPQ
  6. Enter the Name of the MPQ - this needs to be patch-# where # is a number greater than 3. Using numbers 1-3 will not work.
  7. Select Build the MPQ archive from a file or directory, and navigate the path to the 'Modified' folder you created.
  8. Select World of Warcraft - Wrath of the Lich King in the Game Compatibility section
  9. If done correctly, the next step will list however many files are in the MPQ to be made (1 if you are only doing this DBC file)
  10. Follow the wizard through until competion, you will now have a .mpq file in the MPQEdit directory named patch-# where # is the number selected.

Step 6: Add this Patch to your WoW install directory
Copy the patch-#.mpq file and put it in your WoW install under Data -> enUS or your locale

Step 7: Alter the itemdisplayinfo_dbc table in your AzerothCore database
In order for your change to work, the server-side database also needs to be updated. To do this:
  1. Using HeidiSQL, DBeaver or another database editor, open your acore_world database and find the itemdisplayinfo_dbc
  2. This table will likely be empty. You need to enter the information, as it appears in the .dbc file we just edited, into this table.
  3. Save the change, and restart your Worldserver

Step 8: Delete your WoW Cache
To ensure that you do not see an old, cached version of the item, delete the contents of the Cache folder in your WoW install directory

You should now be able to log in to your game, and see the altered model of the item edited.
  12. 
