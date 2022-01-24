ZombieBench, a benchmark test for NWNX-Leto and FPT files.
By dragonsong, dragon@weathersong.net
http://weathersong.infopop.cc

Setup
=====

1. You can either use the GATEWAY database and base objects provided, or create your own. To create your own, refer to the instructions in the module (read the comments on the lever).

2. Put your database into NWN\database. Three files are needed: a CDX file, a DBF file, and an FPT file. They should all have the same name. If you use a name other than "leto_fpt", you will need to configure it in the zb_generator script.

3. Create a directory for your base objects. This can be anywhere, but for simplicity's sake it ought to be in your NWN directory. For instance, nwn\leto or nwn\leto\fptbase.

4. Copy your base objects into this directory. You need three, a base zombie (BIC), a base sword (UTI), and a base ring (UTI). If you want to use your own, make sure to refer to module for instructions (comments on lever, and the zb_database script).

5. Now open the module in the Toolset, and look around. You'll need to edit the zb_generator script and configure a few variables. Recompile that script, save the module. Now you're ready to run the test!

6. Of course, you'll need NWNX to run the test.

7. And you'll also need NWNX-Leto. If you haven't already, download it:

http://www.sourceforge.net/projects/leto

At a bare minimum, you need the NWNX-Leto package (e.g., nwnxleto-build-02+16.rar). You may want to download the current Leto dev / build package as well, to use Moneo.exe for testing scripts or generating your own base objects.

8. The files you need for the NWNX-Leto module are nwnx_leto.dll and LetoScript.dll (found in the NWNX-Leto package on SourceForge). Copy BOTH of these into your NWN directory, where NWNX2.exe lives.

9. Start NWNX and start the ZombieBench module (you can do this from a command prompt, e.g. "nwnx2 -module zombiebench_v01"). Join the server, either as a DM or a player (player is recommended, so you can enjoy fighting the zombies).

10. Open the door to the north. The time it takes for the door to actually swing open is the result of your test. If it takes more than 5 seconds, your server isn't the greatest. If it only takes a second (or is unnoticeable), you're probably running a SMP 3GHz w/SCSI RAID. :}

Remember to examine the loot left by the zombies, after you've sent them all to their final resting.

Troubleshooting
===============
If you open the door and nothing happens (no delay, no zombies), something's wrong. Check these first:

* NWNX2 is running (and you're connected to the server, not single-player).

* The nwnx_leto module successfully loaded (check nwnx.txt) and LetoScript.dll loaded (nwnx_leto.txt).

* You have all the files where they need to be:

leto_fpt.cdx, leto_fpt.dbf, leto_fpt.fpt in your nwn\database directory (setup step 2)
basezombie.bic, basesword.uti, basering.uti in your fptbase directory (steps 3 and 4)

* The zb_generator script is configured with your file paths, db name, file names, etc

If you've checked all of these and it's still not working, try asking for help on the weathersong forums:

http://weathersong.infopop.cc

Do-it-yourself
==============
If you really want, you can rebuild the gateway database from scratch, and also build your own base objects (zombie, sword, ring), instead of using the files included with the module. If you want to start using this system in your own module / PW, you should run through this exercise to get a better understanding for how ZombieLand actually works.

The basic concept of the system is to have a database file act as a go-between for NWN and LetoScript. NWN uses FoxPro database files (CDX, DBF, and FPT) for its StoreCampaignObject (SCO) and RetrieveCampaignObject (RCO) functions. When you SCO an object (say, a monster), it is saved into the FPT file in raw GFF format, which means there's an actual GFF file (BIC or UTI) right inside the FPT. There's some amazing potential here: you can SCO just about anything, even localvault characters.

What really makes it work is RCO. This scripting function loads raw GFF data from an FPT file, makes it a live game object, and then creates it at the location (or in the inventory) of your choice.

So any object (monster, sword, armor, player-character) you store with SCO, you can later create, anywhere in any module, with RCO. And the server does not need to stay up - the files are stored on disk.

Here's where LetoScript comes in. Since RCO can create any object out of GFF data, using LetoScript to create, copy, or modify GFF data means you could potentially RCO *anything*, any file or any arbitrary GFF data, if only there was a way to get into the FPT file and modify what's there, or insert something new. LetoScript allows you to do this, with the <fpt> expression.

So, for a working system, you need the following:

* A database file (CDX, DBT, FPT file)

* NWNX-Leto (nwnx_leto.dll, LetoScript.dll)

The database is acting as a go-between, since LetoScript can push any data into it, and NWN can pull that data out, with RCO. For this reason, the db is referred to as a "gateway", or just "the gateway".

Besides those two items, you can optionally use template files. A template file is a BIC or a UTI you have as a file on disk, that you will manually insert into the gateway using LetoScript. For instance, basezombie.bic, one of the files included with the module, is a Zombie from the Toolset made into a BIC file. LetoScript can load this file, make some changes to it (change the name, the appearance, whatever), then send it to the gateway, before NWN RCOs the zombie, and spawns it. The spawned zombie will have the name and appearance you gave it using LetoScript.

So now your system has three parts to it:

* Database file (gateway)

* Template files (BICs for creatures/NPCs, and UTIs for items)

* NWNX-Leto

You can create the database simply by writing a small script that SCOs some default data (doesn't matter what). Such a script is on the lever in this module - read the comments on the lever and the comments in the zb_database script.

You can also create your own template files (a template animal, a template armor, whatever) using a similar script. This script would need to SCO the base objects you want. Then you would use Moneo / LetoScript to "extract" the objects, which pulls them out of the FPT file and makes them files of their own (just as basezombie.bic, basesword.uti, and basering.uti are). This functionality is also demonstrated on the lever; again, read the comments in zb_database.

Finally, there's the NWScript that does all of the actual generation and randomization. zb_generator is the best example there is for this. Go through it line by line, until you have a general understanding for everything it does. Then try tweaking a few things - the random names, the random properties. Try adding a new function to give the zombies some random armor instead of a random ring. Or make random kobolds, instead of random zombies. At some point you'll need to start learning some LetoScript. You can get help with LetoScript on the weathersong forums (URL at the top of this file), and the documentation for LetoScript is online as well:

http://www.aros.net/~sueko/leto/docs/LetoScript/index.html

Good luck, and have fun!
