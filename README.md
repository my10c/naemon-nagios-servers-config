# Naemon servers scripts


### create\_server\_config.py
* this script create the server configuration to be used by thruk/neamon, the monitor system
* the fw list in the config.json look for the key __fw_hosts__

```
usage: <-s servername> [-f | -h | -v | -d]

create_server_config.py version: 1.0
Copyright 2019 - 2026 © Badassops LLC
License License 3-Clause BSD, https://opensource.org/licenses/BSD-3-Clause ♥
Written by Luc Suryo <luc@suryo.com>


Script to create server configuration file, use with naemon of the given server(s)

options:
  -h, --help            show this help message and exit
  -v, --version         show program's version number and exit
  --dry-run, -d         display the result on screen and delete the configuration file(s)
  --force, -f           force overwrite if the configuration file(s) already exist, default is false

Required arguments one of these: -s or -a. The arguments are mutual exclusion:
  -c CONFIG, --config CONFIG
                        the configuration file to use, default to servers.info.
  -s NAME, --server NAME
                        the server name, The given server name must exist in the file servers.info and has the
                        correct format. Multiple separate by comma's. Example: -s cyan,blue,red
  -a, --all             create all the servers configurations files
```

### config.json
* this file containts all the available checks
* the __ID__'s 80 - 89 is servers for check with handler (advice)
* the __ID__'s 90 - 97 is reserve for the Netgate checks (advice)
* always order the check by __ID__
* to add new, see the example below

```
 "X": {
    "cmd_id":  X ,                     required : unique ID must be a integer and must match the record: the 'X'
    "cmd_name": "",                    required : the command name, defined in the file objects/command.cfg
    "cmd_info": "",                    required : text word explain what the check does
    "cmd_stats": true|false,           required : create Perfdata
    "cmd_notification": true|false,    optional : send notification to teams
    "cmd_allowed_hosts": [],           optional : list of host allowed to us the service
    "cmd_max_check_attempts": x,       optional : attempt count of the check before the check is considered in error
    "cmd_check_interval": x,           optional : seconds between checks
    "cmd_event_handler": "",           optional : the checkk handler name, defined in the file objects/command.cfg
    "cmd_handler": true|false          optional : whatever a handler should be used
  }
```

### servers.info
* this file containts the check per server, infra and fw (Netgate)
* the infra names are hardcode and should only be changed with __extreme caution__
* the infra has no checks, only ping to the infra is checked
* each server contains ip of follow by check-#
* to add new, see the example below

```
#
#  == server name
#  == server ip
#       "cmd_id": 1, "check_disk_nrpe": "Disk check"
#       "cmd_id": 2, "check_fs_ro_nrpe": "RO Filesystems check"
#       "cmd_id": 3, "check_load_nrpe": "Load check"
#       "cmd_id": 4, "check_memory_nrpe": "Memory check"
#       "cmd_id": 5, "check_ntp_nrpe": "NTP offset check"
#       "cmd_id": 6, "check_ntp_time_nrpe": "NTP time check"
#       "cmd_id": 7, "check_pkt_lost_nrpe": "RTA and packet lost check"
#       "cmd_id": 8, "check_ssd_nrpe": "SSD Wear check"
#       "cmd_id": 9, "check_ssh": "SSH"
#       "cmd_id": 10, "check_swap_nrpe": "Swap check"
#       "cmd_id": 11, "check_swap_off_nrpe": "Swap off check"
#       "cmd_id": 12, "check_total_procs_nrpe": "Total procs check"
#       "cmd_id": 13, "check_uptime_nrpe": "Uptime check"
#       "cmd_id": 14, "check_users_nrpe": "Users check"
#       "cmd_id": 15, "check_named_nrpe": "Named check"
#       "cmd_id": 16, "check_dns_local_nrpe": "Local dns"
#       "cmd_id": 17, "check_humidity_nrpe": "Humidity check"
#       "cmd_id": 18, "check_temperature_nrpe": "Temperature check"
#       "cmd_id": 19, "check_ovpn_nrpe": "VPN check"
#       "cmd_id": 20, "check_dns_lilo_nrpe": "Lilo dns"
#       "cmd_id": 21, "check_dns_stitch_nrpe": "Stitch dns"
#       "cmd_id": 22, "check_cert_badassops": "badassops cert"
#       "cmd_id": 23, "check_cert_naemon": "naemon cert"
#       "cmd_id": 24, "check_cert_proxmox": "proxmox cert"
#       "cmd_id": 25, "check_https_maas": "HTTPS check port 5443"
#       "cmd_id": 26, "check_https": "HTTPS check"
#       "cmd_id": 27, "check_ntp_stratum": "NTP stratum check"
#       "cmd_id": 28, "check_ntp_stratum_2": "NTP stratum 2 check"
#       "cmd_id": 29, "check_memory_prxmox_nrpe: "Memory check Proxmox"
#

if a check is not use then use _ or ___, this will make it better readable and pad with _'s so each have the same length
see below:
ansible___:10.10.10.10:1:2:3:4:5:6:7:_:9:__:11:12:13:14:__:__:17:18:__:__:__:__:__:__:__:__:__:28

for fw see below as exmple:
fw name, followed by ip of the LAN interface the checks 90 - 97
burnwall__:10.10.10.1:90:91:91:92:94:95:96:97
```

### netgate
under the netgate directory there are  2 scipt
* check\_info
* check\_uptime

installation
- copy both files to /usr/local/libexec/nagios
- chown to root:wheel
- chmod to 0755
- install the nrpe package *via* the ui

```

add 2 entries as custom shell command (under - service -> nrpe)
 1ft field    4th field
 check_uptime /usr/local/libexec/nagios/check_info
 check_uptime /usr/local/libexec/nagios/check_uptime  -c 1200
```

- install the cron package *via* the ui
- add 1 entry (under - service -> cron)

```
Minute              0
Hours               0
Day of the Month    1
User                root
Command             /etc/rc.banner > /tmp/banner.out
```


### Suggestions welcome
enjoy my10c
