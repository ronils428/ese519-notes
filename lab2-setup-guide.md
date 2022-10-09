## Computer Specs

|   System      |Spec                                                      |
|--------------:|----------------------------------------------------------|
|   macOS       | Big Sur                                                  |
|   Computer    | MacBook Pro (13-inch, 2018, Four Thunderbolt 3 Ports)    |
|   Processor   | 2.3 GHz Quad-Core Intel Core i5                          |
|   Memory      | 16 GB 2133 MHz LPDDR3                                    |
|   StatUp Disk | Macintosh HD                                             |
|   Graphics    | Intel Iris Plus Graphics 655 1536 MB                     |
|   Board       | Adafruit QT Py                                           |



## Setting Up Terminal

While most Macs come with a built in terminal app, I prefer to use ITerm2 simply because of the ease of access. Following this blog post (https://blog.mestwin.net/drop-down-terminal-in-macos-with-iterm2/), you can create a dropdown terminal that is opaque and appears and disappears with a simple keystroke. I have personally found that this form of the terminal greatly improves my workflow as this new ITerm2 doesn't appear in Mac's application switcher. I can freely switch between my code and StackOverFlow and access the terminal only when I need to.

## Setting Up a Text Editor

Once you have a terminal setup (whether you are using ITerm2 or the built in Terminal app), the two first commands you are going to want to do are "ls" and "cd". "ls" will allow you to see which folders you can navigate to and "cd ****" where **** is the destination will allow you to navigate to that destination. Some other commands that I found useful are "mkdir" and "touch" but we will get to those in a second. 

First navigate to a location of your choosing (I chose my Desktop - I used the command cd /Desktop). Once in the desktop, I used the command "mkdir lab2" to create a folder to host all of the contents of the lab. I then used the "cd" command to navigate into this newly created folder (cd lab2). I then made two different .txt files using the command "touch" (touch test.txt setup.txt). I can then use the "ls" command to see if the files have been created. Since they are I can go ahead and use the command "nano test.txt" which will go ahead and open up a text editor. If you don't have nano, I suggest following this guide to install nano (https://phoenixnap.com/kb/use-nano-text-editor-commands-linux). I inputted some text and followed the instructions in the bottom left of my screen to save and exit out of this text editor. After editing the .txt file, I can use the command "cat test.txt" to view the contents of the file directly in the terminal without having to open the file. A flow of all the commands I used are down below.

```
mkdir lab2
cd lab2
touch test.txt setup.txt
nano test.txt
```
Input whatever text and then exit out using <kbd>Ctrl</kbd> + <kbd>X</kbd>. Make sure to save your work by following the commands!
```
cat test.txt
```
This will output the content of your test.txt directly in your terminal.

## Connecting to Serial Console

In order to connect to RP2040’s REPL, we will follow this guide (https://learn.adafruit.com/welcome-to-circuitpython/advanced-serial-console-on-mac-and-linux). I have provided a high level overview of the steps I took to get it working. Firstly, with the board unplugged I inputted the command "ls /dev/tty.*" and took note of the output. Then after connecting the board to my computer, I inputted the same command and saw an additional item in the output. From there I can simply input "screen /dev/tty.usbmodem141441" where the second part is the additional item from the previous step. And from there you should see your RP2040’s REPL. In order to exit this screen simply hit "CTRL a + k" and then it will ask you to confirm and just like that you are back to your terminal screen! Once again the steps are written in code form down below.

```
ls /dev/tty.*
```
The output of the above command should appear as follows:

![image](https://github.com/ronils428/ese519-notes/blob/main/ports.png)

```
screen /dev/tty.usbmodem14201
```
Using the above command I can directly connect to the RP2040’s Serial Console. Whenever I need to exit, I can simply hit <kbd>Ctrl</kbd> + <kbd>A</kbd> + <kbd>K</kbd> and then <kbd>y</kbd> when the prompt asking to confirm pops up.


# Installing the C SDK

Now we will be moving on to installing the C SDK onto the Adafruit QT Py board. We will be follwoing this guide (https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf), however since this guide was meant for the Pico breakout board for the 
RP2040 some of the steps will be irrelvant / different. I've included all the necessary steps I took to get the hello_world example up and running. FOR ALL OF THE MULTI-STEP CODE BLOCKS ONLY INPUT ONE LINE AT A TIME AND HIT ENTER AFTER EACH COMMAND.

For this first part, you can start with the board unplugged. In your terminal input the following commands to create a new folder on your computer in a specific location. 

```
cd ~/
mkdir pico
cd pico
```

From here we will need to clone the pico-sdk and pico-examples git repositories.

```
git clone -b master https://github.com/raspberrypi/pico-sdk.git
cd pico-sdk
git submodule update --init
cd ..
git clone -b master https://github.com/raspberrypi/pico-examples.git
```

Next, because we are using macOS, we need to get homebrew to finish the installation. The command to install homebrew is shown below. To find the official, most updated version, checkout the Homebrew website (https://brew.sh/).
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

From here we need to install the toolchain.
```
brew install cmake
brew tap ArmMbed/homebrew-formulae
brew install arm-none-eabi-gcc
```

Now we should have everything installed and we should be inside the ~/pico directory in our terminal. Typing ```ls``` in the terminal should output two folders ```pico-sdk``` and ```pico-examples```. We then want to follow the commands down below to ensure that everything is up to date.
```
cd pico-sdk
git pull
git submodule update
```

We are almost ready to output Hello World! to our serial console! We just need to get the files ready. We do this by creating a build directory. From the pico directory we created earlier, ```cd``` into ```pico-examples``` and create a build directory.
```
cd pico-examples
mkdir build
cd build
```

Then, assuming you cloned the ```pico-sdk``` and ```pico-examples``` repositories into the same directory side-by-side, set the
```PICO_SDK_PATH```.
```
export PICO_SDK_PATH=../../pico-sdk
```

Then run the following command:
```
cmake ..
```

Before moving on, we need to check one thing first. ```cd``` all the way back out of the build directory and into the hello_world folder. The file path that we want to target is ```hello_world/serial``` and use ```touch``` to open the CMakeLists.txt file. Ensure that the following two lines are in the .txt file exactly as follows. This will ensure that our serial message can be ready via USB.
```
pico_enable_stdio_usb(hello_world 1)
pico_enable_stdio_uart(hello_world 0) 
```

If everything works which it did for me, you can then hit ```ls``` and see all the ready made examples. For now we will just be getting the ```hello_world``` example ready. 

```
cd hello_world
make -j4
```

Use the commands that you've learned this far in the guide (```cd``` and ```ls```) to ensure that a ```hello_usb.uf2``` file has been created inside the hello_world/usb folder.

Now we are ready to connect the board. Connect the board to your computer via micro-USB cable, making sure that you hold down the
```BOOT``` button to force it into USB Mass Storage Mode. The ```BOOT```. The boot button is shown below.

![image](https://github.com/ronils428/ese519-notes/blob/main/boot.png)

After holding the button and connecting to the computer, after a while you should see a drive popup with a name that starts with RPI. You can now let go of the ```BOOT``` button. In your Finder, click on the top menu bar and click on Go -> Enclosing Folder. This should open up a Finder window where your pico folder resides. Click inside pico -> pico-examples -> build -> hello_world -> usb and drag the ```hello_usb.uf2``` file into the drive that popped up earlier. This drive should automatically disappear from your computer. After about two minutes, head over to the terminal and type the commands from the "Connecting to Serial Console" section. I've included them down below for your convenience. 
```
ls /dev/tty.*
```

If you see your board connected in the ports then go ahead and use the same screen command as before.

```
screen /dev/tty.usbmodem14201
```

If everything worked then you should be seeing the following in your terminal screen.

![image](https://github.com/ronils428/ese519-notes/blob/main/helloworld.png)


CONGRATULATIONS YOU HAVE SUCCESSFULLY GOT YOUR QT PY BOARD SENDING DATA OVER USB VIA THE C SDK!!

Notes:
- This was definitely no easy task so congrats for making it this far!
- Following the instructions very closely helped tremendously. I personally ran into some issues where I was connected to the board but I didn't hold down the ```BOOT``` button so I was uploading code to the python version.
- Was also previously trying to follow the Linux instructions in the guide when I needed to scroll down to seciton 9.1 which had instructions for installing the toolchain on macOS.
