### find the so/bin/man etc.
    
    `which`: finds the `binary executable` of the program (if it is in your PATH)

    `whereis`: finds the binary(.so also included), the source, and the man page files for a program

    `locate`: finds all the files that have the pattern specified anywhere in their paths

### library

    - `readelf`
      - `-d`: see dynamic library
      - param `soname`: soname is used to indicate what binary api compatibility your library support.
    - `ldd`: view the dynamic library


### apt
    
    - e.g. upgrade gcc from 7.x to the latest version, `-y` means `--yes` or `--assume-yes`, so no need to wait for the user's input
      - apt-get upgrade -y gcc 

### netstat

lsof maybe invalid when the port is occupied by other user

so use the following cmd

    sudo netstat -tulpn
