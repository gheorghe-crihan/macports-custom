# Custom macports files

To enable use from the macports _port(1)_ shell, do the following:

1. Edit (uncomment) the last line of _/opt/local/etc/macports/sources.conf_ to
   read
```
   ...
   file:///Users/${USERNAME}/ports
```

   see _sources.conf(1)_.   

2. Issue a _portindex(1)_ command.


**NOTE:** The repository contains submodules, so please checkout with ```git recursive```.
