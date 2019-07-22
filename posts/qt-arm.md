---
layout: default
---

# Cross-compile Qt Applications for ARM Devices

I have had to compile programs on a desktop machine so that they run on ARM devices- [Gumstix](https://www.gumstix.com/) for work a few years ago, and most recently a [Beaglebone](http://beagleboard.org/bone) for a home project.
I find setting up a build environment frustrating every single time, so I am recording this as an outline for next time I have to do it.

**The goal:** Set up a build environment on my desktop (Windows 10) machine so I can compile Qt- based programs that run on an ARM device (Beaglebone Green Wireless).

### Steps:
1. Set up a Linux host build environment.
2. Install and test a cross compiler.
3. Configure and build an ARM-friendly version of Qt.
4. Copy the Qt libraries to the Beaglebone.
5. Install Qt Creator and test a Qt-based program.

## 1. Install a Linux host environment
Most of us non-guru Linux tinkerers use either [Ubuntu](https://www.ubuntu.com/) or [Mint](https://linuxmint.com/). I set up a 32-bit Linux Mint set up in a 30GB [VirtualBox](https://www.virtualbox.org/) virtual machine.  
On my first boot, I install the VirtualBox Guest Additions. These will make sure the guest OS (Mint) uses all the features provided by VirtualBox.  
Then I open a terminal window (*Ctrl+T* is a fast way to get one) and install a base compiler set:

```
$ sudo apt-get install build-essential
```

This gets me the GNU C and C++ compilers.
Let's test them with a hello world program. Using a text editor, I create a file called hello.cpp with the following contents:

```
#include <iostream>
using namespace std;
int main()
{
  cout << "Hello World!" << endl; 
  return 0;
}
```

To test the g++ compiler I use the following command:

```
$ g++ hello.cpp -o hello
```

This specifies the output file as _hello_. Running it with `./hello` will print Hello World! on the screen.


## 2. Install and test a cross compiler.
The Beaglebone uses the arm-linuxgnueabi*hf*- compiler. The *hf* here stands for Hard Float, meaning the CPU is able to handle floating point operations instead of having to do it in software.  
To install the compiler in Mint:
```
$ sudo apt-get install linux-armgnueabihf-g++
```
Let's test it using the same hello world program from before:

```
$ arm-linuxhnueabihf-g++ hello.cpp -o HelloARM
$ ./helloARM
```

At this point I get an error message because the x86-architecture processor cannot run this binary (built for a different architecture). I copy it to the Beaglebone and try again:
```
$ scp HelloARM [user]@[beaglebone_ip_address]:[remote_path]/
$ ssh [user]@[beaglebone_ip_address]
[remote_path]$ ./HelloARM
```

Now that I am greeted with a *Hello World!*, I know I can use this compiler for the Qt build.

![Hello ARM](/assets/img/04/01_hello.png)

## 3. Configure and build an ARM-friendly version of Qt
As of right now, the latest release of Qt is 5.5. I gave up trying to compile that for an arm device, and the newest version I have working is 4.8.6.
I download the source package from here:

[Qt Download Archive > Qt Everywhere 4.8.6](https://download.qt.io/archive/qt/4.8/4.8.6/qt-everywhere-opensource-src-4.8.6.tar.gz)

I uncompress the package and modify the following *qmake.conf* files to make sure they list the *arm-linuxgnueabihf-* prefix across the board. I am not entirely sure all three are necessary, but they work with the `configure` command I will use shortly.

### [qt_source]/mkspecs/linux-arm-gnueabi-g++/qmake.conf  

![First qmake.conf](/assets/img/04/02_qmake1.png) 

### [qt_source]/mkspecs/qws/linux-arm-g++/qmake.conf

![Second qmake.conf](/assets/img/04/03_qmake2.png) 

### [qt_source]/mkspecs/qws/linux-arm-gnueabi-g++/qmake.conf

![Second qmake.conf](/assets/img/04/04_qmake3.png) 

I go back to the shell and run the configure commmand. I only want Qt for network, timers, and file operations for the time being. I use the `configure` command to request that no multimedia modules are built.  
I also specify the install directory (*/usr/local/qt4*). All applications compiled with this Qt version will look for the libraries in this path.

```
[qt_source_path]$ ./configure -embedded arm -xplatform qws/linux-arm-gnueabi-g++ -prefix /usr/local/qt4 -debug-and-release -opensource -no-phonon -no-audio-backend -no-phonon-backend -no-multimedia -no-script -no-gif -qt-zlib -no-cups -no-accessibility -no-opengl -no-glib -little-endian -v
```

Once everything is configured, I use `make` and `make install` to build Qt and install it in the specified path:

```
[qt_source_path]$ make
[qt_source_path]$ sudo make install
```

## 4. Copy the Qt libraries to the Beaglebone.
The `make install` command has placed a fresh build of Qt (including qmake and libraries) in */usr/local/qt4/*. I copy it to the Beaglebone as follows:

```
$ scp -r /usr/local/qt4 [user]@[ip address]:/usr/local/
```

## 5. Install Qt Creator and run a test program

I head back to [qt.io](https://www.qt.io/) and download the Qt online installer for 32 bit Linux.  
Once installed, I run Qt Creator and configure an appropriate kit for cross compiling (*Tools->Options->Build & Run*):

### Compiler

Add an *arm-linux-g++* compiler. The binary is found under */usr/bin:*

![Compiler config](/assets/img/04/05_compiler.png)

### Qt version
Add a Qt Version- this means the *qmake* binary. In this case, it is found in */usr/local/qt4/bin*:

![qmake config](/assets/img/04/06_qt_version.png)

### Kit
Add a kit to target the ARM device in question. I call this kit *Arm Qt 4.8.6*. I set the compiler and Qt versions to the ones I just created and hit OK.

![Kit config](/assets/img/04/07_kit.png)

Create a new Console project and call it *HelloArm*. Qt Creator will auto-generate a *main.cpp* file. Add debug lines so it  prints *Hello ARM!*:


```
#include <QCoreApplication>
#include <QDebug>		// Added to template

int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);
    qDebug() << "Hello ARM!";	// Added to template

    return a.exec();
}
```

I build the project using the Arm Qt 4.8.6 kit.

I go back to the shell and copy the resulting binary to the Beaglebone:

```
[path_to_binary]$ scp HelloArm [user]@[ip_address]:[remote_path]
[path_to_binary]$ ssh [user]@[ip_address]
[remote_path]$ ./HelloArm

```

![Hello ARM!](/assets/img/04/08_helloarm.png)


Success! I can now access tons of extra functionality provided by Qt on the Beaglebone.

Questions? Comments? [Give me a shout!](/about)
