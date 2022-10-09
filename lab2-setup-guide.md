## Computer Specs

|   System      |Spec                                                      |
|--------------:|----------------------------------------------------------|
|   macOS       | Big Sur                                                  |
|   Computer    | MacBook Pro (13-inch, 2018, Four Thunderbolt 3 Ports)    |
|   Processor   | 2.3 GHz Quad-Core Intel Core i5                          |
|   Memory      | 16 GB 2133 MHz LPDDR3                                    |
|   StatUp Disk | Macintosh HD                                             |
|   Graphics    | Intel Iris Plus Graphics 655 1536 MB                     |


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
