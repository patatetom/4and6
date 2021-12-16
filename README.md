# 4and6
small pieces of code dedicated to the digital investigation of Windows (mainly from Linux)

## EVTX

- used tools :
  - `evtxexport` and `evtxinfo` from https://github.com/libyal/libevtx

- scripts :
  - `evtx.toxml`

```console
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
$ evtx.toxml /mnt/Windows/System32/winevt/Logs/Application.evtx 2> /tmp/evtx.toxml.log | evtx.totsv
/mnt/Windows/System32/winevt/Logs/Application.evtx	Microsoft-Windows-User Profiles Service	1531	2021-10-27T10:59:43.418663000Z	1	{…
…

$ evtx.toxml /mnt/Windows/System32/winevt/Logs/Microsoft-Windows-Kernel-PnP%4Configuration.evtx 2> /tmp/evtx.toxml.log | evtx.totsv |
  cut -f 4,6 | grep 'VID_....&PID_...' | sort -r
2021-12-14T08:39:00.408109200Z	{'DeviceInstanceId': 'USBSTOR\\Disk&…
2021-12-14T08:39:00.404377100Z	{'DeviceInstanceId': 'USB\\VID_…
2021-12-14T08:39:00.393581500Z	{'DeviceInstanceId': 'USB\\VID_…
…
```

- other tools to look at :
  - `evtx_dump` at https://github.com/omerbenamram/evtx
  - python `python-evtx` module at https://github.com/williballenthin/python-evtx

> in the same way as for `evtxexport`, `evtx_dump` can be formatted with `sed '/^Record/d;/^<?xml/d;s/ xmls="[^"]*"//g'`

## REG


## USB
