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

- other tools to look at :
  - `evtx_dump` at https://github.com/omerbenamram/evtx
  - python `python-evtx` module at https://github.com/williballenthin/python-evtx

> in the same way as for `evtxexport`, `evtx_dump` can be formatted with `sed '/^Record/d;/^<?xml/d;s/ xmls="[^"]*"//g'`

## REG


## USB
