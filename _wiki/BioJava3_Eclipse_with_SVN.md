---
title: BioJava3 Eclipse with SVN
---

Prerequisite
------------

-   Make sure you have a copy of the latest eclipse (Galileo)

<!-- -->

-   Make sure you have Java 1.6 installed. (if you are on OSX 10.4.x,
    install [soylatte](http://landonf.bikemonkey.org/static/soylatte/))

<!-- -->

-   Install the [m2eclipse](http://eclipse.org/m2e/) Maven eclipse
    plugin (previously hosted by
    [Sonatype](http://m2eclipse.sonatype.org/)). Be sure to include SCM
    integration, offered through the m2eclipse-extras package.

<!-- -->

-   Install [Subversive](http://www.eclipse.org/subversive/) through
    eclipse update site (it's the official eclipse plugin), or install
    [subclipse](http://subclipse.tigris.org/) plugin for subversion
    instead (latest versions: 1.8.16 and 1.6.18).

Installation
------------

-   In the SVN Repository Exploring view: Right click on the folder
    <i>/biojava/biojava-live/trunk</i> and select Check Out as Maven
    project

Details for specific Eclipse Versions
-------------------------------------

### Update for Eclipse Juno (I use Juno SR1)(October 2012)

There used to be some conflicts with SVN connectors, but thank God,
things are good now. Now, it's as simple as this:

-   From Help menu select "Install new software..."
-   select the Juno update site option (Juno -
    <http://download.eclipse.org/releases/juno>)
-   Check "m2e - Maven Integration for Eclipse", and "Subversive SVN
    Team Provider"
-   Go through the regular process (next, accept, & restart)
-   After installing both plugins, from File menu, select import...

![](Importing Maven Project.png "Importing Maven Project.png")

-   "Checkout Maven Projects from SCM" looks like this.

![](Checkout Maven Project through SCM.png "Checkout Maven Project through SCM.png")

-   beside the **SCM URL** label, there is a drop down box for SCM type,
    and a text field for URL. In the first time, the drop down box will
    be empty...
-   you will find a small line below containing "Find more SCM
    connectors in the m2e Marketplace". Click m2e Marketplace.
-   In the "m2e team providers" section, select "m2e-Subversive",

![](M2E Subversive Handler.png "M2E Subversive Handler.png")

then the next/accept/etc. process.

![](M2E Subversive Handler1.png "M2E Subversive Handler1.png")

-   When you start first call to SVN-based operation, eclipse will show
    you a window asking you to choose a connector. The safest way is to
    select SVNkit (pure java implementation). This choice will not
    install any conflicting binaries to your system, also, being
    Java-based, it is OS independent.. you will keep your head from all
    hassle related to win32/64 compatibility and such stuff.
-   congratulations! Everything is ready now. Go back to the "Checkout
    Maven Projects from SCM", you will see an SVN option in the dropdown
    box. Select it, key in the URl, and import (here I use the
    developer's access URL, which might be different from yours).

![](Checkout Maven Project through SCM (populated).png "Checkout Maven Project through SCM (populated).png")

------------------------------------------------------------------------

### Update for Eclipse Helios SR2 (May 2011)

The above plugins are still available and work fine, however, below are
the few important particulars.

-   Use update URLs from the plugins web site, do not use Eclipse market
    place, as in that case you will have to install all the components
    of the plugin manually and it will be very easy to forget to install
    something important, besides it does not always work.

<!-- -->

-   Make sure you have full JDK 1.6 installed, JRE will not be
    sufficient (some *Maven* plugins will not work)

<!-- -->

-   After JDK installation point Eclipse to the JDK location. For this
    edit *eclipse.ini* found in the Eclipse root directory. Insert *-vm*
    keyword with the location of your JDK and make sure that this
    keyword precedes *-vmargs* (!) for example

`  -vm`  
`  C:/Java/jdk1.6.23/bin`  
`  -vmargs`  
`  -Xms40m`  
`  -Xmx512m`

-   If you work on any other operating system but win32, you will have
    to install JavaHL library for the *subclipse* plugin manually. More
    information about it can be found here:
    [<http://subclipse.tigris.org/wiki/JavaHL>](http://subclipse.tigris.org/wiki/JavaHL)

<!-- -->

-   When adding the URL of BioJava development repository do not add the
    actual folder you want to check out, otherwise you may not be able
    to checkout it as maven project. For example if you want to checkout

`svn+ssh://dev.open-bio.org/home/svn-repositories/biojava/biojava-live/trunk/`

use

`svn+ssh://dev.open-bio.org/home/svn-repositories/biojava/biojava-live`

as the repository URL and then navigate to trunk in the Eclipse SVN
explorer.

### Eclipse Indigo (August 29th 2011)

I downloaded the Eclipse j2ee version (OSX Lion) and used the Eclipse
Marketplace to find and install the following plugins:

from Eclipse Marketplace:

`- Subclipse `  
`- Maven Integration for Eclipse`  
`- Maven Integration for Eclipse WTP (probably  not needed, but I do a lot of web stuff, so I added it)`  
`- m2e-subclipse (SCM connector, bring Maven and subclipse together)`

from Yoxos Marketplace

`- SvnKit Client Adapter (needs to be enabled in Preferences->Team->SVN, SVN Interface ->SVNKit, Pure Java )`  

To check out BioJava you can do: new -\> Maven -\>checkout project from
SCM, add biojava URL to .../biojava-live/trunk, press finish

A useful blog article providing more help for how to install Maven is
here:
[1](http://www.shareyourwork.org/roller/ralphsjavablog/entry/eclipse_indigo_maven_and_svn)

Here some instructions for where to find the various eclipse plugins:
[2](https://wiki.openmrs.org/display/docs/Step+by+Step+Installation+for+Developers) --[Andreas](User:Andreas "wikilink")
04:43, 30 August 2011 (UTC)

Anonymous access with Git
-------------------------

In the past, anonymous access to BioJava source code via SVN has been
problematic. Alternatively, you can retrieve the source code from the
read-only [BioJava github mirror](https://github.com/biojava) using
Eclipse.

*Requirements:*

-   Java 6 (1.6) JDK
-   Eclipse Indigo (3.7) or greater
-   m2eclipse (Eclipse [update
    site](http://download.eclipse.org/technology/m2e/releases))
-   EGit (pre-installed with Indigo)
-   Maven SCM Handler for EGit (m2e-egit) from Eclipse Marketplace

*Additional setup:*

-   Edit your eclipse.ini file to use the Java JDK as your VM
    (instructions above, required for Maven)
-   If necessary, [set the HOME environment
    variable](http://wiki.eclipse.org/EGit/User_Guide#Setting_up_the_Home_Directory_on_Windows)
    (required for EGit)

*Import the BioJava Maven project from Git ([StackOverflow
answer](http://stackoverflow.com/questions/4869815/importing-a-maven-project-into-eclipse-from-git)):*

-   Open the Git Repository perspective
-   Clone the BioJava git repository
    (http://github.com/biojava/biojava.git)
-   Expand the cloned repository, right-click "Working directory", and
    pick "Import Maven Projects..."
-   Open the Java perspective
-   Select all of the projects, right-click and choose "Team \> Share
    Project", select "Git", and check the "Use or create repository in
    parent folder of project" box

