Sample init scripts and service configuration for gdcd
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/gdcd.service:    systemd service unit configuration
    contrib/init/gdcd.openrc:     OpenRC compatible SysV style init script
    contrib/init/gdcd.openrcconf: OpenRC conf.d file
    contrib/init/gdcd.conf:       Upstart service configuration file
    contrib/init/gdcd.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "gdc" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, gdcd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, gdcd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that gdcd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If gdcd is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/gdc/gdc.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/gdc.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/gdcd
Configuration file:  /etc/gdc/gdc.conf
Data directory:      /var/lib/gdcd
PID file:            /var/run/gdcd/gdcd.pid (OpenRC and Upstart)
                     /var/lib/gdcd/gdcd.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the gdc user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
gdc user and group.  Access to gdc-cli and other gdcd rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start gdcd" and to enable for system startup run
"systemctl enable gdcd"

4b) OpenRC

Rename gdcd.openrc to gdcd and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/gdcd start" and configure it to run on startup with
"rc-update add gdcd"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop gdcd.conf in /etc/init.  Test by running "service gdcd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy gdcd.init to /etc/init.d/gdcd. Test by running "service gdcd start".

Using this script, you can adjust the path and flags to the gdcd program by
setting the GDCD and FLAGS environment variables in the file
/etc/sysconfig/gdcd. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
