# 4and6
small pieces of code dedicated to the digital investigation of Windows (mainly from Linux)


## TIME

> times are usually stored in UTC format.

- from UTC to locale time (time zone in which the following commands are executed) :

```console
$ # get locale time zone
$ timedatectl -p Timezone show
Timezone=Europe/Paris

$ # Paris is UTC+1 on december (HNEC)
$ date -d 2021-12-14T08:39:00.408109200Z
Tue Dec 14 09:39:00 CET 2021

$ # Z and UTC are equivalent
$ date -d "2021-12-14 08:39:00 UTC"
Tue Dec 14 09:39:00 CET 2021

$ # input format must have a valid format
$ date -d "Tue Dec 14 08:39:00 UTC"
Tue Dec 14 09:39:00 CET 2021

$ # output format can be specified
$ date -d "2021-12-14 08:39 UTC" +'%Y%m%d%H%M%S'
20211214093900
```


## EVTX

- used tools :
  - `evtxexport` and `evtxinfo` from https://github.com/libyal/libevtx

- scripts :
  - `evtx.toxml`

```console
$ # export in xml format
$ evtx.toxml /mnt/Windows/System32/winevt/Logs/System.evtx 2> /tmp/evtx.toxml.log
<Events File="/mnt/Windows/System32/winevt/Logs/System.evtx">
<Event>
  <System>
…
  </System>
  <EventData>
…
  </EventData>
</Event>
…
</Events>
```

  - `evtx.totsv`

> `evtx.totsv` is based on `evtx.toxml` export

```console
$ # export in tsv format
$ evtx.toxml /mnt/Windows/System32/winevt/Logs/Application.evtx 2> /tmp/evtx.toxml.log | evtx.totsv
/mnt/Windows/System32/winevt/Logs/Application.evtx	Microsoft-Windows-User Profiles Service	1531	2021-10-27T10:59:43.418663000Z	1	{…
…

$ # tsv lends itself easily to grep et cetera
$ evtx.toxml /mnt/Windows/System32/winevt/Logs/Microsoft-Windows-Kernel-PnP%4Configuration.evtx 2> /tmp/evtx.toxml.log | evtx.totsv |
  cut -f 4,6 | grep 'VID_....&PID_...' | sort -r
2021-12-14T08:39:00.408109200Z	{'DeviceInstanceId': 'USBSTOR\\Disk&…
2021-12-14T08:39:00.404377100Z	{'DeviceInstanceId': 'USB\\VID_…
2021-12-14T08:39:00.393581500Z	{'DeviceInstanceId': 'USB\\VID_…
…

$ # tsv can also be easily integrated into a sqlite database
$ sqlite3 /tmp/events.db 'CREATE TABLE IF NOT EXISTS "events" (file TEXT, provider TEXT, eventid INT, date TEXT, recordid INT, data TEXT);'
$ sqlite3 -cmd '.mode tabs' /tmp/events.db '.import /dev/stdin events' < <(
  for evtx in /mnt/Windows/System32/winevt/Logs/*.evtx
  do
    evtx.toxml "${evtx}" | evtx.totsv
  done 2> /tmp/evtx.toxml.log
)
$ sqlite3 /tmp/events.db 'SELECT … FROM events WHERE … ORDER BY date DESC;'
```

- other tools to look at :
  - `evtx_dump` at https://github.com/omerbenamram/evtx
  - python `python-evtx` module at https://github.com/williballenthin/python-evtx

> in the same way as for `evtxexport`, `evtx_dump` can be formatted with `sed '/^Record/d;/^<?xml/d;s/ xmls="[^"]*"//g'`

## REG


## USB
