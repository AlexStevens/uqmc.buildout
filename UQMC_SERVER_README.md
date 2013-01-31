Starting the Instance
--------------------------------------------------------------------------------

For the most part of the issues that will arise, a simple restart should work.

To start, restart and stop your instance:

    /etc/init.d/zope_8080 start
    /etc/init.d/zope_8080 restart
    /etc/init.d/zope_8080 stop

To see the log that the server outputs:

    tail -f <location of 'uqmc.buildout'>/var/log/instance.log


Problems with Server? Contact
--------------------------------------------------------------------------------

If there's any troubles, feel free to contact
[Alex Stevens](mailto:alexander.stevens@uqconnect.edu.au) (UQMC President 2013)


Notes
--------------------------------------------------------------------------------

__#1 - 31/01/2013__: Created by [Alex Stevens](http://github.com/AlexStevens/)
