lastcast
========

Scrobble music playing on Chromecast devices to last.fm and libre.fm.

By default, lastcast will scrobble music playing on Spotify,
Google Play Music, SoundCloud, and Plex, but can be configured to
scrobble any media player that supports Chromecast.

Getting started
---------------

``pip install lastcast``

Set up an initial configuration with the configuration
creation tool:

``lastcast --wizard``

If you'd prefer to set up the configuration manually, modify
``example.lastcast.toml`` from the repo and save it to
``~/.lastcast.toml``.

Once the configuration file is in place, just run ``lastcast`` to connect to
the Chromecast and start scrobbling!

Detailed macOS setup
--------------------

(for anyone not familiar with Python and pip)

Enter the following commands in your Terminal (Terminal.app, iTerm2, etc.):

1. ``sudo easy_install pip``
2. ``sudo pip install --upgrade lastcast --ignore-installed six``
3. ``lastcast --wizard``

This will prompt you to create a last.fm API application and then ask for your
login information, which will only be stored locally on your computer.

You may get an error on step 2 about ``cc`` missing. If this is the case,
install xcode by running ``xcode-select --install`` and retry step 2.

Now everything should be set up. When you want to start scrobbling, simply
run ``lastcast`` in the terminal.

Generally, I run lastcast like so: ``while true; do lastcast; sleep 5; done``.
This will help in case lastcast crashes for whatever reason (please open an issue
if you see something go wrong!)

Linux / systemd instructions
----------------------------

1. ``sudo pip install --upgrade lastcast``
2. ``lastcast --wizard``
3. Edit the code block below as needed (remember to fill in the config path!)
   and write to ``/usr/lib/systemd/system/lastcast.service``
   (or ``/etc/systemd/system/lastcast.service`` if the directory doesn't exist)
4. ``sudo systemctl daemon-reload``
5. ``sudo systemctl enable lastcast``

.. code-block:: ini

   [Unit]
   Description=lastcast
   Requires=networking.service

   [Service]
   ExecStart=/usr/local/bin/lastcast --config [PATH TO ~/.lastcast.toml]
   Restart=always
   RestartSec=5

   [Install]
   WantedBy=network-online.target


No Chromecast devices found?
----------------------------

It is possible that an incompatible version of ``netifaces`` will prevent lastcast
from finding any Chromecast devices on your network. This is known to affect
Windows 10 with ``netifaces==0.10.5`` installed.

The fix, as described in `this StackOverflow answer
<http://stackoverflow.com/a/41517483>`_ is simply to uninstall the wrong version
and manually install ``netifaces==0.10.4``.

.. code:: bash

   $ pip uninstall netifaces
   $ pip install netifaces==0.10.4

If you still can't discover any Chromecasts, please `open an issue
<https://github.com/erik/lastcast/issues/new>`_.
