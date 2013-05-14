UnknownElementBot
=================
UnknownElementBot_ is a Python_ robot that edits Wikipedia_ and interacts with people
over IRC_. This file provides a basic overview of how to install and setup the
bot; more detailed information is located in the ``docs/`` directory (available
online at PyPI_).


Installation
------------

This package contains the core ``UnknownElementBot``, abstracted enough that it should
be usable and customizable by anyone running a bot on a MediaWiki site. Since
it is component-based, the IRC components can be disabled if desired. IRC
commands and bot tasks specific to `my instance of UnknownElementBot`_ that I don't
feel the average user will need are available from the repository
`UnknownElementBot-plugins`_.

It's recommended to run the bot's unit tests before installing. Run ``python
setup.py test`` from the project's root directory. Note that some
tests require an internet connection, and others may take a while to run.
Coverage is currently rather incomplete.

Latest release (v0.1)
~~~~~~~~~~~~~~~~~~~~~

UnknownElementBot is available from the `Python Package Index`_, so you can install the
latest release with ``pip install earwigbot`` (`get pip`_).

You can also install it from source [1]_ directly::

    curl -Lo UnknownElementBot.tgz https://github.com/UnknownElement/UnknownElementBot/tarball/v0.1
    tar -xf UnknownElementBot.tgz
    cd UnknownElement-UnknownElementBot-*
    python setup.py install
    cd ..
    rm -r UnknownElementBot.tgz earwig-UnknownElementBot-*
    
    
Setup
-----

The bot stores its data in a "working directory", including its config file and
databases. This is also the location where you will place custom IRC commands
and bot tasks, which will be explained later. It doesn't matter where this
directory is, as long as the bot can write to it.

Start the bot with ``UnknownElementBot path/to/working/dir``, or just ``UnknownElementBot`` if
the working directory is the current directory. It will notice that no
``config.yml`` file exists and take you through the setup process.

There is currently no way to edit the ``config.yml`` file from within the bot
after it has been created, but YAML is a very straightforward format, so you
should be able to make any necessary changes yourself. Check out the
`explanation of YAML`_ on Wikipedia for help.

After setup, the bot will start. This means it will connect to the IRC servers
it has been configured for, schedule bot tasks to run at specific times, and
then wait for instructions (as commands on IRC). For a list of commands, say
"``!help``" (commands are messages prefixed with an exclamation mark).

You can stop the bot at any time with Control+C, same as you stop a normal
Python program, and it will try to exit safely. You can also use the
"``!quit``" command on IRC.

Customizing
-----------

The bot's working directory contains a ``commands`` subdirectory and a
``tasks`` subdirectory. Custom IRC commands can be placed in the former,
whereas custom wiki bot tasks go into the latter. Developing custom modules is
explained below, and in more detail through the bot's documentation on PyPI_
(or in the ``docs/`` dir).

Note that custom commands will override built-in commands and tasks with the
same name.

``Bot`` and ``BotConfig``
~~~~~~~~~~~~~~~~~~~~~~~~~

`UnknownElementBot.bot.Bot`_ is UnknownElementBot's main class. You don't have to instantiate
this yourself, but it's good to be familiar with its attributes and methods,
because it is the main way to communicate with other parts of the bot. A
``Bot`` object is accessible as an attribute of commands and tasks (i.e.,
``self.bot``).

`UnknownElementBot.config.BotConfig`_ stores configuration information for the bot. Its
docstring explains what each attribute is used for, but essentially each "node"
(one of ``config.components``, ``wiki``, ``irc``, ``commands``, ``tasks``, and
``metadata``) maps to a section of the bot's ``config.yml`` file. For example,
if ``config.yml`` includes something like::

    irc:
        frontend:
            nick: MyChatBot
            channels:
                - "##UnknownElementBot"
                - "#channel"
                - "#other-channel"

...then ``config.irc["frontend"]["nick"]`` will be ``"MyChatBot"`` and
``config.irc["frontend"]["channels"]`` will be ``["##UnknownElementBot", "#channel",
"#other-channel"]``.

Custom IRC commands
~~~~~~~~~~~~~~~~~~~

Custom commands are subclasses of `UnknownElementBot.commands.Command`_ that override
``Command``'s ``process()`` (and optionally ``check()`` or ``setup()``)
methods.

The bot has a wide selection of built-in commands and plugins to act as sample
code and/or to give ideas. Start with test_, and then check out chanops_ and
afc_status_ for some more complicated scripts.

Custom bot tasks
~~~~~~~~~~~~~~~~

Custom tasks are subclasses of `UnknownElementBot.tasks.Task`_ that override ``Task``'s
``run()`` (and optionally ``setup()``) methods.

See the built-in wikiproject_tagger_ task for a relatively straightforward
task, or the afc_statistics_ plugin for a more complicated one.

Footnotes
---------

- Questions, comments, or suggestions about the documentation? `Let me know`_
  so I can improve it for other people. Let me known: techgeekcasey@hotmail.com
