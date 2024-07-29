# Introduction to Linux Libraries

You can practice troubleshooting a Linux library issue in a real server with this SadServers Scenario

## Linux Libraries

When developing an application, we often leverage external code libraries rather than creating everything from scratch. Libraries consist of collections of items such as functions.

For instance, if we need encryption in our code, we have two options:

1. The first involves mastering encryption principles, including the underlying mathematics and formulas, then implementing them into code from scratch. Assuming everything goes perfectly, our code would work flawlessly and adhere to all necessary standards.

2. The second option, however, is to utilize someone else's work—libraries that have already undergone this process and proven their functionality and reliability (at least up to the point of our usage; future discoveries of bugs or security issues notwithstanding).

In most cases, the latter approach, leveraging existing libraries, is the preferred and advisable path to take.

We have two kinds of libraries:

* **Static**: These libraries are included in the compiled application. We add static libraries at compile time, and they become part of the application binary. Since they are already linked during compilation, the application does not have any dependency on these libraries at runtime.

* **Shared/Dynamic**: These libraries are used in our application but are not included in the binary. They must be provided at runtime to the application. Instead of being compiled into the application, they are dynamically linked when the application runs.

# How Linux Libraries Work

In Linux systems, a component known as the *dynamic linker/loader* (`ld.so`, `ld-linux*.so*`) is responsible for locating the required libraries and loading them before executing the application.

By default, Linux libraries reside in directories such as */lib*, */usr/lib*, and, in some distributions, */lib64* and */usr/lib64* for the x86_64 architecture. Often, these directories are symbolic links to */usr/lib* and */usr/lib64*. Libraries follow a naming convention such as `libLIBRARYNAME.so.VERSION`, for example, `liblzma.so.5`. It's important to note that while this naming convention is common, it's not mandatory or enforced, and libraries may appear in alternative formats.

When the dynamic linker attempts to start an application and load its dependencies, it searches through various locations in a specific order. Some well-known places include:

* **LD_PRELOAD**: The `LD_PRELOAD` environment variable instructs the dynamic linker to load specified libraries even if they are not listed in the application's dependencies.

* **/etc/ld.so.preload**: Similar to `LD_PRELOAD`, this file contains a list of libraries to be loaded by the dynamic linker, but the libraries are specified in the file rather than in an environment variable.

* **LD_LIBRARY_PATH**: The `LD_LIBRARY_PATH` variable sets directories for the dynamic linker to check before searching the default locations. It functions similarly to the PATH variable for executable files.

* **/etc/ld.so.cache**: This file serves as a cache of system libraries. It contains information about libraries available on the system. It's crucial to update this cache whenever libraries are modified. For example, when installing a package, the ldconfig command is executed to update this cache.

The `ld.so.cache` file is crucial because the system relies on it to quickly locate system libraries, avoiding the need to search through all mentioned directories such as /usr/lib. This optimization significantly reduces the time and potential delays in starting applications.

It's important to note that some of the behaviors mentioned above may depend on options provided to the application at compile time. For example, the `LD_LIBRARY_PATH` variable may not function if the application is executed in secure-execution mode.

# Diving Into Practice

Now, let’s move on from theory and dive into practice.

`ldd` command shows what libraries an executable file depends on. Here’s the output of the `ldd` command for the `sleep` command:

```bash
$ ldd `which sleep`
        linux-vdso.so.1 (0x00007fffd6d23000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f3ad2400000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f3ad2688000)
```

This output indicates the libraries that the `sleep` command depends on. It’s essential to note that `ldd` command may not always reveal dependencies, especially when dealing with interpreted languages like Python. For instance, consider the Python script provided below:

```bash
#!/usr/bin/env python3
import lzma
import time

print(lzma.__name__)

time.sleep(10)
```

If we attempt to run `ldd` directly on the source file, it won’t show anything, as demonstrated below:

```bash
ldd ./main.py
        not a dynamic executable
```

This result is expected because Python is an interpreted language, and the script itself is not a compiled executable file. Instead, it is executed by the Python interpreter at runtime. Therefore, `ldd` won’t detect any dependencies when applied directly to Python source files. Let’s run the code and check what `lsof` shows us:

```bash
COMMAND    PID  USER   FD   TYPE DEVICE SIZE/OFF     NODE NAME
python3 102408 myuser  cwd    DIR  252,1     4096  7046183 /home/myuser/main.py
python3 102408 myuser  rtd    DIR  252,1     4096        2 /
python3 102408 myuser  txt    REG  252,1  5904904 26745137 /usr/bin/python3.10
python3 102408 myuser  mem    REG  252,1 11477872 26747919 /usr/lib/locale/locale-archive
python3 102408 myuser  mem    REG  252,1   170456 26766181 /usr/lib/x86_64-linux-gnu/liblzma.so.5.2.5
python3 102408 myuser  mem    REG  252,1  2220400 26740839 /usr/lib/x86_64-linux-gnu/libc.so.6
python3 102408 myuser  mem    REG  252,1   108936 26766887 /usr/lib/x86_64-linux-gnu/libz.so.1.2.11
python3 102408 myuser  mem    REG  252,1   194872 26765755 /usr/lib/x86_64-linux-gnu/libexpat.so.1.8.7
python3 102408 myuser  mem    REG  252,1   940560 26740857 /usr/lib/x86_64-linux-gnu/libm.so.6
python3 102408 myuser  mem    REG  252,1    45240 26768959 /usr/lib/python3.10/lib-dynload/_lzma.cpython-310-x86_64-linux-gnu.so
python3 102408 myuser  mem    REG  252,1    27002 26748343 /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
python3 102408 myuser  mem    REG  252,1   240936 26740828 /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
python3 102408 myuser    0u   CHR  136,0      0t0        3 /dev/pts/0
python3 102408 myuser    1u   CHR  136,0      0t0        3 /dev/pts/0
python3 102408 myuser    2u   CHR  136,0      0t0        3 /dev/pts/0
```

When we run the provided Python script and check the running process with lsof, we can see that many libraries are indeed loaded, including LZMA, which we have used in our Python code. In this case, the LZMA library seems to be written in CPython, and when we check its dependencies using ldd, we find that it relies on other libraries such as liblzma:

```bash
$ ldd /usr/lib/python3.10/lib-dynload/_lzma.cpython-310-x86_64-linux-gnu.so
        linux-vdso.so.1 (0x00007ffef99fd000)
        liblzma.so.5 => /lib/x86_64-linux-gnu/liblzma.so.5 (0x00007fd768603000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fd768200000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fd768654000)
```

This demonstrates the intricate dependencies between system libraries, highlighting the importance of understanding and managing them properly. It’s important to be cautious with system libraries, as even small changes can have significant consequences.

Now let’s go back to our `sleep` command. Here’s the output of the strace command for executing the `sleep` command with a limited set of system calls to monitor file accesses when we executing `sleep`. We are doing it to check what files are accessed when we run `sleep` command:

```bash
$ strace -e openat,open,access -f sleep 5s
access("/etc/ld.so.preload", R_OK)      = 0
openat(AT_FDCWD, "/etc/ld.so.preload", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
```

This output confirms that the system is reading `ld.so.preload` and `ld.so.cache`, as mentioned earlier.

Now, Let’s set `LD_*` variables and check `strace` again:

```bash
$ mkdir -pv /tmp/mylibs/dir{1,2}
mkdir: created directory '/tmp/mylibs'
mkdir: created directory '/tmp/mylibs/dir1'
mkdir: created directory '/tmp/mylibs/dir2'

$ cd /tmp/mylibs/dir1

$ export LD_LIBRARY_PATH="${PWD}:/tmp/mylibs/dir2" LD_PRELOAD="/lib/x86_64-linux-gnu/liblzma.so.5"

$ strace -e openat,open,access -f sleep 5s
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/liblzma.so.5", O_RDONLY|O_CLOEXEC) = 3
access("/etc/ld.so.preload", R_OK)      = 0
openat(AT_FDCWD, "/etc/ld.so.preload", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/tmp/mylibs/dir1/glibc-hwcaps/x86-64-v3/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/glibc-hwcaps/x86-64-v2/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/tls/haswell/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/tls/haswell/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/tls/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/tls/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/haswell/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/haswell/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir1/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/glibc-hwcaps/x86-64-v3/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/glibc-hwcaps/x86-64-v2/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/tls/haswell/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/tls/haswell/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/tls/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/tls/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/haswell/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/haswell/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/x86_64/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp/mylibs/dir2/libc.so.6", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
```

As you can see, there’s considerably more activity when running `sleep` with the `LD_*` variables set. It checks for libraries in directories specified in `LD_LIBRARY_PATH` and also opens `/lib/x86_64-linux-gnu/liblzma.so.5`, which we set in `LD_PRELOAD`.

By using `lsof`, we can validate that liblzma is being loaded for the sleep process. Here’s the output of the `lsof` command:

```bash
$ sleep infinity &

$ lsof -p $(pgrep -f 'sleep infinity')
lsof: WARNING: can't stat() overlay file system /var/lib/docker/overlay2/ade36c155b2e1d19b09046cd6cbe4d9348b813bd2ec5791bed818820798d8189/merged
      Output information may be incomplete.
lsof: WARNING: can't stat() nsfs file system /run/docker/netns/c5e2679fc7fb
      Output information may be incomplete.
COMMAND    PID  USER   FD   TYPE DEVICE SIZE/OFF     NODE NAME
sleep   147063 myuser  cwd    DIR  252,1     4096 11313226 /tmp/mylibs/dir1
sleep   147063 myuser  rtd    DIR  252,1     4096        2 /
sleep   147063 myuser  txt    REG  252,1    35336 26788412 /usr/bin/sleep
sleep   147063 myuser  mem    REG  252,1 11477872 26747919 /usr/lib/locale/locale-archive
sleep   147063 myuser  mem    REG  252,1  2220400 26740839 /usr/lib/x86_64-linux-gnu/libc.so.6
sleep   147063 myuser  mem    REG  252,1   170456 26766181 /usr/lib/x86_64-linux-gnu/liblzma.so.5.2.5
sleep   147063 myuser  mem    REG  252,1   240936 26740828 /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
sleep   147063 myuser    0u   CHR  136,2      0t0        5 /dev/pts/2
sleep   147063 myuser    1u   CHR  136,2      0t0        5 /dev/pts/2
sleep   147063 myuser    2u   CHR  136,2      0t0        5 /dev/pts/2
```

As you can see, `/usr/lib/x86_64-linux-gnu/liblzma.so.5.2.5` is indeed loaded into the memory for the `sleep` process which we did set in `LD_PRELOAD` variable.

These tricks have various practical applications:

* **Developing**: During development, you can utilize temporary paths to test new or updated libraries before integrating them into applications that depend on them, ensuring they work correctly.
* **Management**: In cases where an application relies on a library version incompatible with the system’s default, you can override library paths for that specific application, allowing it to use alternative libraries.
* **Security**: Unfortunately, these techniques can also be exploited for malicious purposes. Cyber attackers may hijack libraries to manipulate application behavior, potentially introducing backdoors or other security vulnerabilities.

# Conclusion

In conclusion, libraries are essential building blocks in the toolkit of system administrators, SREs, and DevOps professionals. Understanding their nuances, dependencies, and loading mechanisms is key to optimizing system performance, ensuring reliability, and enhancing security.

Although library management is not as common nowadays due to the increasing use of containers, it’s still vital to understand these concepts. At the OS level, you might need to troubleshoot, patch, or investigate, making this understanding essential