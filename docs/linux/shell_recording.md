---
tags:
  - Shells
---

# Shell recording

???+ Note "Reference(s)"
    * <https://asciinema.org/>
    * <https://github.com/asciinema>
    * <https://github.com/asciinema/asciinema>
    * <https://github.com/asciinema/asciicast2gif>
    * <https://www.youtube.com/watch?v=nAUohhEJ56M>


---
## Table of contents

<!-- vim-markdown-toc GitLab -->
- [Shell recording](#shell-recording)
  - [Table of contents](#table-of-contents)
  - [`script` + `scriptreplay` + `byzanz` or `congif`](#script--scriptreplay--byzanz-or-congif)
  - [`ttyrec` + `ttygif`](#ttyrec--ttygif)
    - [install `ttyrec`](#install-ttyrec)
    - [install `ttygif`](#install-ttygif)
    - [use](#use)
  - [`asciinema` + `asciicast2gif`](#asciinema--asciicast2gif)
    - [install](#install)
    - [use](#use-1)
<!-- vim-markdown-toc -->

---
## `script` + `scriptreplay` + `byzanz` or `congif`

* `byzanz` is a small and efficient screencast creator. It records your desktop session or parts of
  it to an animated GIF, Ogg Theora or Flash.


!!! Note ""

    === "pacman"
        ```console
        # pacman -S byzanz
        ```

    === "apt"
        ```console
        # apt install byzanz
        ```

    === "yum"
        ```console
        # yum install byzanz
        ```

    === "dnf"
        ```console
        # dnf install byzanz
        ```

* config generates GIF animations of console sessions. Like `scriptreplay`, it reads the output of
  script, including timing information. Unlike `scriptreplay`, `congif` parses the session dialogue
  and encodes it as a GIF animation that can be viewed on graphical programs. See
  <https://github.com/lecram/congif>.


`byzanz` will record

The convert program is a member of the ImageMagick
```console
$ convert -size 1920x1080 canvas:black /tmp/black.png
$ feh --bg-fill --no-fehbg /tmp/black.png
```

```console
$ script --log-timing file.tm --log-out script.out
    > Script started, file is script.out
$ ttyrec -f record.ttyrec
$ ls
    > ...
$ echo "test"
    > ...
$ exit
$ exit
    > Script done, file is script.out

$ rm record.ttyrec

$ scriptreplay --maxdelay 0.1 --log-timing file.tm --log-out script.out

$ byzanz-record --exec 'scriptreplay --maxdelay 0.1 --log-timing file.tm --log-out script.out' byzanz-record.gif
```

!!! Note
    You can get the current mouse position with `xev`.


* recording the top right corner of a 1920x1080 screen:
```console
$ byzanz-record -x 960 -w 960 -y 0 -h 540 --exec 'scriptreplay --maxdelay 0.1 --log-timing file.tm --log-out script.out' byzanz-record.gif
```

* recording the left side of a 1920x1080 screen:
```console
$ byzanz-record -x 0-w 960 -y 0 -h 1080 --exec 'scriptreplay --maxdelay 0.1 --log-timing file.tm --log-out script.out' byzanz-record.gif
```

---
## `ttyrec` + `ttygif`

* `ttyrec` is a terminal recorder (and `ttyplay` is a terminal player).
* `ttygif` converts a `ttyrec` file into GIF files. It's a stripped down version of `ttyplay` that
  screenshots every frame.

???+ Note "Reference(s)"
    * <https://en.wikipedia.org/wiki/Ttyrec>
    * <https://github.com/ovh/ovh-ttyrec>
    * <https://aur.archlinux.org/packages/ovh-ttyrec-git/>
    * <https://github.com/mjording/ttyrec>
    * <https://github.com/icholy/ttygif>
    * <https://aur.archlinux.org/packages/ttygif/>

### install `ttyrec`

!!! Note ""

    === "aur"
        ```console
        $ git clone https://aur.archlinux.org/ovh-ttyrec-git.git
        $ cd ovh-ttyrec-git
        $ makepkg -si
        ```

    === "build manually"
        ```console
        $ git clone https://github.com/ovh/ovh-ttyrec
        $ git checkout v1.1.6.7 # checkout to the latest release (v1.1.6.7 at the time of writing)
        $ ./configure && make
        ```
        Optionally you can also run `$ make install` if you want, or add `ttrec`, `ttyplay` and
        `ttytime` to your `$PATH` variable. But note that is not needed to use the binaries: you
        can just run `./ttyrec` (or `./ttyplay`, `ttytime`) from the build folder.

### install `ttygif`

!!! Note ""

    === "aur"
        ```console
        $ git clone https://aur.archlinux.org/ovh-ttyrec-git.git
        $ cd ovh-ttyrec-git
        $ makepkg -si
        $ cd ..
        $ git clone https://aur.archlinux.org/ttygif.git
        $ cd ttygif
        $ makepkg -si
        ```

    === "build manually"
        TODO

### use

* record:
```console
$ ttyrec -f record.ttyrec
$ ls
    > ...
$ echo "test"
    > ...
$ exit
```

* replay in shell:
```console
$ ttyplay record.ttyrec
```

* create GIF from record:
```console
$ ttygif record.ttyrec
```

---
## `asciinema` + `asciicast2gif`

`asciinema` is terminal session recorder.

???+ Note "Reference(s)"
    * <https://github.com/asciinema/asciinema>

### install

!!! Note ""

    === "pacman"
        ```console
        $ sudo pacman -S asciinema
        ```

    === "build manually"
        TODO

### use

```console
$ asciinema rec -i 0.1 record.cast
```