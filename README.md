# i3pystatus

i3pystatus is a (hopefully growing) collection of python scripts for 
status output compatible to i3status / i3bar of the i3 window manager.

## Installation

To install it, follow these steps:

    cd ~/.config/i3status/
    git clone git@github.com:janoliver/i3pystatus contrib
    cd contrib/i3pystatus
    cp __main__.py.dist __main__.py

Add the following to `~/.config/i3status/config`:

    general {
        output_format = "i3bar"
        colors = true
        interval = 5
    }

Change your i3wm config to the following:

    # i3bar
    bar {
        status_command    cd ~/.config/i3status/contrib ; i3status | python -m i3pystatus
        position          top
        workspace_buttons yes
    }

And finally adjust the settings in `~/.config/i3status/contrib/i3pystatus/__main__.py`
as you like. 

## Modules


### backlight


Shows backlight brightness


* format — format string used for output. {brightness}, {max_brightness}, {percentage} are available (default: {brightness}/{max_brightness})
* backlight — backlight. See /sys/class/backlight/ (default: acpi_video0)
* color —  (default: #FFFFFF)



### battery


This class uses the /proc/acpi/battery interface to check for the
battery status


* battery_ident —  (default: BAT0)
* format —  (default: {status} {remaining})



### clock


This class shows a clock


* format — stftime format string



### load


Shows system load


* format — format string used for output. {avg1}, {avg5} and {avg15} are the load average of the last one, five and fifteen minutes, respectively. {tasks} is the number of tasks (i.e. 1/285, which indiciates that one out of 285 total tasks is runnable). (default: {avg1} {avg5})



### mail


Generic mail checker

The `backends` setting determines the backends to use. Currently available are:


* backends — List of backends (instances of i3pystatus.mail.xxx)
* color —  (default: #ffffff)
* color_unread —  (default: #ff0000)
* format —  (default: {unread} new email)
* format_plural —  (default: {unread} new emails)
* hide_if_null — Don't output anything if there are no new mails (default: True)


    Currently available backends are:


#### imap


This class handles IMAP mailservers. The mail server
functionality is implemented in the subclass IMAP.MailServer

The servers parameter should be a list of dicts containing the following
items:
* host
* port (optional, defaults to 143)
* username
* password
* ssl (optional, defaults to False)


* servers —  (required)



#### notmuchmail


This class uses the notmuch python bindings to check for the
number of messages in the notmuch database with the tags "inbox"
and "unread"


* db_path —  (required)



#### thunderbird


This class listens for dbus signals emitted by
the dbus-sender extension for thunderbird.

Requires
* python-dbus
* python-gobject2







### modsde


This class returns i3status parsable output of the number of
unread posts in any bookmark in the mods.de forums.


* format — Use {unread} as the formatter for number of unread posts (default: {unread} new posts in bookmarks)
* offset — subtract number of posts before output
* color —  (default: #7181fe)
* username —  (required)
* password —  (required)



### regex


Simple regex file watcher


* format — format string used for output (default: {0})
* regex —  (required)
* file — file to search for regex matches
* flags — Python.re flags



### temp


Shows CPU temperature


* format — format string used for output. {temp} is the temperature in degrees celsius, {critical} and {high} are the trip point temps. (default: {temp} °C)
* color —  (default: #FFFFFF)
* color_critical —  (default: #FF0000)
* high_factor —  (default: 0.7)




## Contribute

To contribute a module, make sure it uses one of the Module classes. Most modules
use IntervalModule, which just calls a function repeatedly in a specified interval.

The output attribute should be set to a dictionary which represents your modules output,
the protocol is documented [here](http://i3wm.org/docs/i3bar-protocol.html).

Please add an example for how to configure it to `__main__.py.dist`. It should be
a python class that can be registered with the `I3statusHandler` class. Also don't
forget to add yourself to the LICENSE file.

**Patches and pull requests are very welcome :-)**

