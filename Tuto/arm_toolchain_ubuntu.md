# Install ARM toolchain in Ubuntu 20.04
16 Oct 2020

"_ARM decided to deprecate the use of PPA - their page at launchpad now has an anouncement: "... all new binary and source packages will not be released on Launchpad henceforth ..."._"

To make use of their latest arm-none-eabi-gdb you have to install gcc-arm-embedded manually.

Download latest version from their website and unpack it into some directory. I used /usr/share/ :

```sh
sudo tar xjf gcc-arm-none-eabi-your-version.bz2 -C /usr/share/
```

Create links so that binaries are accessible system-wide:

```sh
sudo ln -s /usr/share/gcc-arm-none-eabi-your-version/bin/arm-none-eabi-gcc /usr/bin/arm-none-eabi-gcc 
sudo ln -s /usr/share/gcc-arm-none-eabi-your-version/bin/arm-none-eabi-g++ /usr/bin/arm-none-eabi-g++
sudo ln -s /usr/share/gcc-arm-none-eabi-your-version/bin/arm-none-eabi-gdb /usr/bin/arm-none-eabi-gdb
sudo ln -s /usr/share/gcc-arm-none-eabi-your-version/bin/arm-none-eabi-size /usr/bin/arm-none-eabi-size
```

Install dependencies. ARM's "full installation instructions" listed in readme.txt won't tell you what dependencies are - you have to figure it out by trial and error. In my system I had to manually create symbolic links to force it to work:

```sh
sudo apt install libncurses-dev
sudo ln -s /usr/lib/x86_64-linux-gnu/libncurses.so.6 /usr/lib/x86_64-linux-gnu/libncurses.so.5
sudo ln -s /usr/lib/x86_64-linux-gnu/libtinfo.so.6 /usr/lib/x86_64-linux-gnu/libtinfo.so.5
```

Check if it works:

```sh
arm-none-eabi-gcc --version
arm-none-eabi-g++ --version
arm-none-eabi-gdb --version
arm-none-eabi-size --version
```