This file is used to install Anaconda and set Sublime as the default text editor. Be very careful when you install the Anaconda. By the way, try to avoid the stupid software store

---

# 1. Install Anaconda

The fisrt thing I want to say about Anaconda is that: Never install it through the `apt-get`. I think this command always makes lots of trouble. It seems help you simplify the process. However, it will make much more obasacles later. As we always says, official ones are the best. Here is the [link.](https://docs.anaconda.com/anaconda/install/linux/)

1. Find the Anaconda Offcial website.

You can just Google it. Then choose the file you want to install, click "Download".

2. Verify data integrity with SHA-256, then install with bash. (Follow the instructions). Two things you need to be careful here:

- We recommend you accept the default install location. Do not choose the path as /usr for the Anaconda/Miniconda installation.

- “Do you wish the installer to initialize Anaconda3 by running conda init?” We recommend “yes”. (Otherwise, you need to first run `source <path to conda>/bin/activate` and then run `conda init`. Much more trouble)

3. `source ~/.bashrc` or Close and open your terminal window.

4. To run conda from anywhere without having the base environment activated by default, use`conda config --set auto_activate_base False`. This only works if you have run conda init first.

5. Verify your installation. You can use the instruction [here.](https://docs.anaconda.com/anaconda/install/verify-install/) `conda list` or `conda info` both help.

6. Notice that the Anaconda Group does not add anaconda-navigator into Ubuntu 18.04. To open the navigator, run the following line when you open the terminal:
```
source ~/anaconda3/bin/activate root
anaconda-navigator
```

# 2. Install Sublime

As we said before, there are tons of way to install it, but install via software store is the least thing you should do. Here is the guidances.

1. Go to the official website of Sublime, find the [install pages.](https://www.sublimetext.com/docs/3/linux_repositories.html)

2. Use the following lines for installation.
```
# Through apt
# Install the GPG key:
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

# Ensure apt is set up to work with https sources:
sudo apt-get install apt-transport-https

# Select the channel to use:
# Stable
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

# Dev
echo "deb https://download.sublimetext.com/ apt/dev/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

#Update apt sources and install Sublime Text
sudo apt-get update
sudo apt-get install sublime-text
```

3. Now we can set the Sublime as the default text editor. [Guidance](https://askubuntu.com/questions/396938/how-do-i-make-sublime-text-3-the-default-text-editor)

- Make sure that `/usr/share/applications/sublime_text.desktop` exists: `ls /usr/share/applications/sublime_text.desktop`

- Then, open `/usr/share/applications/defaults.list` with Sublime: `subl /usr/share/applications/defaults.list`

- Search for all instances of `gedit` and replace them with `sublime_text`. Save the file, log out and back in, and you should be all set.

- Don't forget to log out and back in.
