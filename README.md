Introduction
--------------------------------------------------------------------------------

This is an example buildout package for a project, please note that it is only
a guide and should be used at your own risk.  
In this generic README, please substitute ``<your-project-name>`` with a 
lower-case name without any spaces. For example, ``testing-project``.

Please read the notes at the bottom.


Preparation
--------------------------------------------------------------------------------

Install the following for using Plone/Pillow with a Virtual Environment:

    sudo apt-get install python-virtualenv python-dev libjpeg62-dev libxslt1-dev zlib1g-dev

>__Note:__ It is not good practice to use the system Python located in
``/usr/bin``. Instead, please use a Python Virtual Environment or install a
local copy of Python.  

Create a folder to house all of the project files and open it:

    mkdir <your-project-name>
    cd <your-project-name>

Install a Python Virtual Environment (this will use your system Python version)
and activate it:

    virtualenv <your-project-name>.python
    source <your-project-name>.python/bin/activate

>__Note:__ Activating the Virtual Environment will force your current terminal
to utilise the local instance of Python within the Virtual Environment. It
should show up in your terminal
like so ``(<your-project-name>.python)alex@desktop:~$``


Buildout Creation
--------------------------------------------------------------------------------

Follow the next two headings to either buildout using this example or
from scratch.

### Using this Example Buildout

Copy the contents of this example buildout into a new
directory:

    mkdir <your-project-name>.buildout
    cp -R path/to/example.buildout/* <your-project-name>.buildout

### Creating a Buildout from Scratch
Firstly, install ZopeSkel within your Virtual Environment:

    easy_install zopeskel

Once this is installed, you can use Templer to create a buildout template:

    templer basic_buildout <your-project-name>.buildout
    rm -f <your-project-name>.buildout/buildout.cfg
    mkdir <your-project-name>.buildout/profiles

You can define your own ``base.cfg`` in
``<your-project-name>.buildout/profiles/`` or you could copy the one from
this example.


Buildout Configuration
--------------------------------------------------------------------------------

Once you have prepared your OS for the project buildout, you can configure the
``buildout.cfg``. Firstly, create ``buildout.cfg`` in
``<your-project-name>.buildout/``:

    cd <your-project-name>.buildout/
    touch buildout.cfg

Open ``buildout.cfg`` and edit it to at least have an extends parameter:

    [buildout]
    extends =
        profiles/base.cfg
        http://dist.plone.org/release/4.3-latest/versions.cfg

>__Note:__ You can change the pinned Plone version in the above from
``4.3-latest/versions.cfg`` to say ``4.2.4/versions.cfg``, etc.

You can now set up and build your buildout (make sure you've activated your 
Virtual Environment):

    python bootstrap.py
    bin/buildout


Starting the Instance
--------------------------------------------------------------------------------

### Start in Development Mode

While developing, you will want to run your Plone instance in development mode.
Development mode sets all ``portal_javascripts``, ``portal_css``, etc, to
development mode, it will run in the foreground and also provides a verbose
output to terminal.

To run your instance in development mode, just run the following command:

    bin/instance fg

### Start in Production Mode

Otherwise, if you do not want to run in development mode, the "production" mode
will start the instance in the background.

To start, restart and stop your instance in "production" mode:

    bin/instance start
    bin/instance restart
    bin/instance stop

To see the log that the server outputs:

    tail -f var/log/instance.log


Advanced
--------------------------------------------------------------------------------

A more advanced ``buildout.cfg`` would have a style that looks similar to this:

    [buildout]
    extends =
        profiles/base.cfg
    
    develop +=
        ../<your-project-name>.types/
        ../<your-project-name>.theme/
    
    eggs +=
        <your-project-name>.theme
        <your-project-name>.types

    parts +=
        ldapdeploy
        initscript

    [versions]
    zope.tal = 3.6.1

    [ldapdeploy]
    recipe = zc.recipe.egg
    eggs = ${instance:eggs}
    initialization = configfile = "${buildout:directory}/parts/instance/etc/zope.conf"
    arguments = configfile

    [initscript]
    recipe = collective.recipe.template
    url = http://www.link-to-your-initscript.com/buildout/templates/zope_initscript.templ
    output = /etc/init.d/${initscript:name}
    mode = 755
    name = zope_${instance:http-address}


Notes
--------------------------------------------------------------------------------

__#1 - 30/01/2013__: Created by [Alex Stevens](http://github.com/AlexStevens/)
at Mooball IT - [www.mooball.com](http://www.mooball.com/)
