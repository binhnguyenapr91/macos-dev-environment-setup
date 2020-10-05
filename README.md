## macOS Dev Environment Setup

### Homebrew

#### Install
	xcode-select --install
	
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	
	brew doctor

#### Usage
To install a package (or Formula in Homebrew vocabulary) simply type:
	
	brew install <formula>
	
To see if any of your packages need to be updated:

	brew outdated
	
To update a package:
	
	brew upgrade <formula>

Homebrew keeps older versions of packages installed, in case you want to rollback. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

	brew cleanup
To see what you have installed (with their version numbers):

	brew list --versions

#### Homebrew Services

A nice extension to Homebrew is Homebrew Services. It will automatically launch things like databases when your computer starts, so you don't have to do it manually every time.

Homebrew Services will automatically install itself the first time you run it, so there is nothing special to do.

After installing a service (for example a database), it should automatically add itself to Homebrew Services. If not, you can add it manually with:
	
	brew services <formula>

Start a service with:
	
	brew services start <formula>

At anytime you can view which services are running with:
	
	brew services list

### iTerm2

#### Install

It’s a replacement for the terminal. It offers a lot of features that are really useful. I’ll list my favorite ones below.
To install it, open the terminal (this is the last time you’ll need it), and run:
	
	brew cask install iterm2

Now, feel free to replace terminal from the Dock (if you have it) with iTerm2. Or just open Spotlight and type iTerm2.
Hotkey window, You can show or hide the iTerm2 window via a hotkey from anywhere very quickly.
	
	Preferences > Keys > Hotkey > ☑️ Show/hide all windows with a system-wide hotkey

Since we're going to be spending a lot of time in the command-line, let's install a better terminal than the default one. Download and install iTerm2.
In Finder, drag and drop the iTerm Application file into the Applications folder.

You can now launch iTerm, through the Launchpad for instance.

Let's just quickly change some preferences. In iTerm2 > Preferences..., under the tab General, uncheck Confirm closing multiple sessions and Confirm "Quit iTerm2 (Cmd+Q)" command under the section Closing.

In the tab Profiles, create a new one with the "+" icon, and rename it to your first name for example. Then, select Other Actions... > Set as Default. Under the section General set Working Directory to be Reuse previous session's directory. Finally, under the section Window, change the size to something better, like Columns: 125 and Rows: 35.

When done, hit the red "X" in the upper left (saving is automatic in macOS preference panes). Close the window and open a new one to see the size change.

#### Beautiful terminal

Since we spend so much time in the terminal, we should try to make it a more pleasant and colorful place. What follows might seem like a lot of work, but trust me, it'll make the development experience so much better.

First let's add some color. There are many great color schemes out there, but if you don't know where to start you can try Atom One Dark. Download the iTerm presets for the theme by running:

	cd ~/Downloads
	curl -o "Atom One Dark.itermcolors" https://raw.githubusercontent.com/nathanbuchar/atom-one-dark-terminal/master/scheme/iterm/One%20Dark.itermcolors
	curl -o "Atom One Light.itermcolors" https://raw.githubusercontent.com/nathanbuchar/atom-one-dark-terminal/master/scheme/iterm/One%20Light.itermcolors

Then, in iTerm2 Preferences, under Profiles and Colors, go to Color Presets... > Import..., find and open the Atom One Dark.itermcolors file we just downloaded. Repeat these steps for Atom One Light.itermcolors. Now open Color Presets... again and select Atom One Dark to activate the dark theme (or choose the light them if that's your preference).

Not a lot of colors yet. We need to tweak a little bit our Unix user's profile for that. This is done (on macOS and Linux), in the ~/.bash_profile text file (~ stands for the user's home directory).

We'll come back to the details of that later, but for now, just download the files .bash_profile, .bash_prompt, .aliases attached to this document into your home directory (.bash_profile is the one that gets loaded, I've set it up to call the others):
	
	cd ~
	curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.bash_profile
	curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.bash_prompt
	curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.aliases
	
With that, open a new terminal tab (Cmd+T) and see the change! Try the list commands: ls, ls -lh (aliased to ll), ls -lha (aliased to la).
Now we have a terminal we can work with!

### Git

macOS comes with a pre-installed version of Git, but we'll install our own through Homebrew to allow easy upgrades and not interfere with the system version. To do so, simply run:
	
	brew install git
	
When done, to test that it installed fine you can run:
	
	which git
	
The output should be /usr/local/bin/git.
Let's set up some basic configuration. Download the .gitconfig file to your home directory:
	
	cd ~
	curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitconfig
	
It will add some color to the status, branch, and diff Git commands, as well as a couple aliases. Feel free to take a look at the contents of the file, and add to it to your liking.
Next, we'll define your Git user (should be the same name and email you use for GitHub and Heroku):
	
	git config --global user.name "Your Name Here"
	git config --global user.email "your_email@youremail.com"
	
They will get added to your .gitconfig file.
On a Mac, it is important to remember to add .DS_Store (a hidden macOS system file that's put in folders) to your project .gitignore files. You also set up a global .gitignore file, located for instance in your home directory (but you'll want to make sure any collaborators also do it):
	
	cd ~
	curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitignore
	git config --global core.excludesfile ~/.gitignore

See git log as tree 
	
	git config --global alias.l "log --graph --all --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s %C(white)- %an, %ar%Creset' -n 20"

### Zsh

As macOS’s default shell since Catalina, Zsh is built on top of Bash and provides a lot of cool features.
The first thing I recommend is having Homebrew manage its installation — open iTerm2, and run:
	
	brew install zsh zsh-completions

To update our default shell to be Homebrew’s Zsh, we need to edit the shell’s whitelist: sudo vim /etc/shells. (If you’re not comfortable with Vim, you can use TextEdit instead by running sudo open /etc/shells.)

Add a new line with /usr/local/bin/zsh, save, and close.

To change the default shell, run: 
	
	chsh -s /usr/local/bin/zsh.
	
Restart the terminal, and confirm we’re on the correct Zsh by running: 
	
	echo $SHELL. 
	
You should see /usr/local/bin/zsh.

#### Oh My Zsh

More info here: https://github.com/robbyrussell/oh-my-zsh

Install with curl
	
	sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
	
When the installation is done, edit ~/.zshrc and set ZSH_THEME="agnoster" for the default look. 

#### Install a patched font
##### clone
	
	git clone https://github.com/powerline/fonts.git --depth=1
	
##### install
	
	cd fonts
	./install.sh
##### clean-up a bit
	
	cd ..
	rm -rf fonts

Set this font in iTerm2 (iTerm → Preferences → Profiles → Text → Font), in the dropdown select the desired Font. You will see it change on the fly.
Restart iTerm2 for all changes to take effect.

### Visual Studio Code

With the terminal, the text editor is a developer's most important tool. Everyone has their preferences, but if you're just getting started and looking for something simple that works, Visual Studio Code is a pretty good option.

Go ahead and download it. Open the .dmg file, drag-and-drop in the Applications folder, you know the drill now. Launch the application.
Note: At this point I'm going to create a shortcut on the macOS Dock for both for Visual Studio Code and iTerm. To do so, right-click on the running application and select Options > Keep in Dock.

Just like the terminal, let's configure our editor a little. Go to Code > Preferences > Settings. In the very top-right of the interface you should see an icon with brackets that appeared { } (on hover, it should say "Open Settings (JSON)"). Click on it, and paste the following:
	
	{
	  "editor.tabSize": 2,
	  "editor.rulers": [80],
	  "files.insertFinalNewline": true,
	  "files.trimTrailingWhitespace": true,
	  "workbench.editor.enablePreview": false
	}
	
Feel free to tweak these to your preference. When done, save the file and close it.

Pasting the above JSON snippet was handy to quickly customize things, but for further setting changes feel free to search in the "Settings" panel that opened first (shortcut Cmd+,). When you're happy with your setup, you can save the JSON to quickly restore it on a new machine.

If you remember only one keyboard shortcut in VS Code, it should be Cmd+Shift+P. This opens the Command Palette, from which you can run pretty much anything.
Let's open the command palette now, and search for Shell Command: Install 'code' command in PATH. Hit enter when it shows up. This will install the command-line tool code to quickly open VS Code from the terminal. When in a projects directory, you'll be able to run:
	
	cd myproject/
	code .
	
VS Code is very extensible. To customize it further, open the Extensions tab on the left.

Let's do that now to customize the color of our editor. Search for the Atom One Dark Theme extension, select it and click Install. Repeat this for the Atom One Light Theme.

Finally, activate the theme by going to Code > Preferences > Color Theme and selecting Atom One Dark (or Atom One Light if that is your preference).

### Vim

Although VS Code will be our main editor, it is a good idea to learn some very basic usage of Vim. It is a very popular text editor inside the terminal, and is usually pre-installed on any Unix system.

For example, when you run a Git commit, it will open Vim to allow you to type the commit message.

I suggest you read a tutorial on Vim. Grasping the concept of the two "modes" of the editor, Insert (by pressing i) and Normal (by pressing Esc to exit Insert mode), will be the part that feels most unnatural. Also, it is good to know that typing :x when in Normal mode will save and exit. After that, it's just remembering a few important keys.

Vim's default settings aren't great, and you could spend a lot of time tweaking your configuration (the .vimrc file). But if you only use Vim occasionally, you'll be happy to know that Tim Pope has put together some sensible defaults to quickly get started.

Using Vim's built-in package support, install these settings by running:

	mkdir -p ~/.vim/pack/tpope/start
	cd ~/.vim/pack/tpope/start
	git clone https://tpope.io/vim/sensible.git
	
With that, Vim will look a lot better next time you open it!

### Node.js
#### Install:
	
	brew install node
	
The recommended way to install Node.js is to use nvm (Node Version Manager) which allows you to manage multiple versions of Node.js on the same machine.
Install nvm by copy-pasting the install script command into your terminal.

Once that is done, open a new terminal and verify that it was installed correctly by running:

	command -v nvm	
	
View the all available stable versions of Node with:

	nvm ls-remote --lts
	
Install the latest stable version with:

	nvm install node

It will also set the first version installed as your default version. You can install another specific version, for example Node 10, with:
	
	nvm install 10
	
And switch between versions by using:

	nvm use 10
	nvm use default
	
See which versions you have install with:
	
	nvm ls
	
Change the default version with:

	nvm alias default 10
	
In a project's directory you can create a .nvmrc file containing the Node.js version the project uses, for example:

	echo "10" > .nvmrc
	
Next time you enter the project's directory from a terminal, you can load the correct version of Node.js by running:
	
	nvm use								
	
#### npm

Installing Node also installs the npm package manager.
To install a package:

	npm install <package> # Install locally
	npm install -g <package> # Install globally
	
To install a package and save it in your project's package.json file:
	
	npm install --save <package>
	
To see what's installed:
	
	npm list --depth 1 # Local packages
	npm list -g --depth 1 # Global packages
	
To find outdated packages (locally or globally):
	
	npm outdated [-g]
	
To upgrade all or a particular package:

	npm update [<package>]
	
To uninstall a package:
	npm uninstall --save <package>
	
### SSH

If you don't already have an SSH key, you must generate a new SSH key. If you're unsure whether you already have an SSH key, check for existing keys.

If you don't want to reenter your passphrase every time you use your SSH key, you can add your key to the SSH agent, which manages your SSH keys and remembers your passphrase.

#### Generating a new SSH key

Open Terminal.
Paste the text below, substituting in your GitHub email address.
	
	ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
	
This creates a new ssh key, using the provided email as a label. Generating public/private rsa key pair.

When you're prompted to "Enter a file in which to save the key," press Enter. 

This accepts the default file location. Enter a file in which to save the key (/Users/you/.ssh/id_rsa)

At the prompt, type a secure passphrase. For more information, see "Working with SSH key passphrases".
	
	Enter passphrase (empty for no passphrase): [Type a passphrase]
	Enter same passphrase again: [Type passphrase again]

#### Adding your SSH key to the ssh-agent

Before adding a new SSH key to the ssh-agent to manage your keys, you should have checked for existing SSH keys and generated a new SSH key. When adding your SSH key to the agent, use the default macOS ssh-add command, and not an application installed by macports, homebrew, or some other external source.

Start the ssh-agent in the background.
	
	eval "$(ssh-agent -s)"
	
If you're using macOS Sierra 10.12.2 or later, you will need to modify your ~/.ssh/config file to automatically load keys into the ssh-agent and store passphrases in your keychain.
First, check to see if your ~/.ssh/config file exists in the default location.

	open ~/.ssh/config
	
The file /Users/you/.ssh/config does not exist.
If the file doesn't exist, create the file.
	
	touch ~/.ssh/config
	
Open your ~/.ssh/config file, then modify the file, replacing ~/.ssh/id_rsa if you are not using the default location and name for your id_rsa key.
	
	Host *
    	AddKeysToAgent yes
    	UseKeychain yes
    	IdentityFile ~/.ssh/id_rsa
	
Add your SSH private key to the ssh-agent and store your passphrase in the keychain. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_rsa in the command with the name of your private key file.
	
	ssh-add -K ~/.ssh/id_rsa
	
Note: The -K option is Apple's standard version of ssh-add, which stores the passphrase in your keychain for you when you add an ssh key to the ssh-agent. If you don't have Apple's standard version installed, you may receive an error. For more information on resolving this error, see "Error: ssh-add: illegal option -- K."  

#### Add the SSH key to your GitHub account.

Copy public key to clipboard:
	
	pbcopy < ~/.ssh/id_rsa.pub
	
### JDK 8
#### Install Java 8 JDK
	
	brew tap adoptopenjdk/openjdk
	brew search jdk
	brew cask install adoptopenjdk8
	
#### Check Java version:
	
	java -version
	
#### Get JAVA_HOME Path:
	
	usr/libexec/java_home -V
	
#### Insert to .zshrc: 

	vim ~/.zshrc
	
Insert line above:

	export JAVA_HOME=Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
Source .zshrc:

	source ~/.zshrc
	
Check JAVA_HOME:
	
	echo $JAVA_HOME
	
### MySQL Server (root/Sysadmin1234$)
#### Install:

	brew update
	brew install mysql
	
#### Run:
	
	mysql.server start
	mysql.server stop
	
#### Setup security:
	
	mysql_secure_installation
	
Open:
	
	mysql -u root -p
	
### Elasticsearch
#### Install:
	
	brew install elasticsearch
	
#### Run:

	elasticsearch
	
#### Test: 
	
	curl -XGET 'http://localhost:9200/'
	http://localhost:9200/_plugin/head/
	
### IntelliJ Ultimate

	brew cask install intellij-idea
	
### Postman
	
	brew cask install postman
	
### Workbench

	brew cask install mysqlworkbench
	
### Maven:	
#### Install:
	
	brew install maven
	
#### Check mvn location:

	which mvn

#### Setup:

	vim ~/.mavenrc
	
#### Add to file: 
	
	export JAVA_HOME=$(/usr/libexec/java_home)	
	source .mavenrc
	
#### Test:
	
	mvn -v
	
### Tomcat:
#### Install:
	
	brew install tomcat
	
#### Run:

	catalina run
	catalina stop


