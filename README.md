# apt-alias

`apt-alias` is a "man-in-the-middle" script to intercept `apt` and `apt-get` commands.  The script logs all
install, remove, and purge commands before continuing with them, which provides a convenient logfile 
summary of which packages users have actively installed or removed over the life of the installation.

By construction, the script is implemented on a per-user basis.  If more than one user installs or removes
packages on a system, each user should have the following changes made within their $HOME directory, or else 
the log file will not included changes to installed packages caused by that user.

To implement:
1) Create aliases
    If `~/.bash_aliases` does not exist, create it.  Add the following lines anywhere:

      alias apt-get='$HOME/bin/apt-get-log'
      alias apt='$HOME/bin/apt-log'

    If you just now created `~/.bash_aliases`, new shell sessions will automatically detect its existence, 
    with no further configuration needed.  To activate these changes in the current shell session, use the
    source command to load the file: `$ source ~/.bash_aliases`.

2) Create scripts
    Create the two files referenced by the aliases in Step 1):
    
      $ touch ~/bin/apt-get-log
      $ touch ~/bin/apt-log
      
    Using your preferred editor, copy the contents of `apt-alias` into them.
    
3) Configure scripts
    In `~/bin/apt-get-log`, set `orig=apt-get`.
    In `~/bin/apt-log`, set `orig=apt`.

Having separate files set to separate aliases allows `apt` commands to be executed as `apt` commands and 
`apt-get` commands to be executed as `apt-get` commands after logging relevant install/remove/purge commands 
to a common file.

TODO:
1) Log which user executed the install/remove/purge command
2) Implement a second log which keeps a list of current user-installed packages (i.e., removed packages are
deleted from the log).  The format should be tailored toward using this file to automate the installation of 
an identical set of packages on a new system.
