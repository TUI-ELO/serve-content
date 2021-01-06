Download and install Go quickly with the steps described here.

For other content on installing, you might be interested in:

*   [Managing Go installations](https://golang.org/doc/manage-install.html) -- How to install multiple versions and uninstall.
*   [Installing Go from source](https://golang.org/doc/install-source.html) -- How to check out the sources, build them on your own machine, and run them.

## 1\. Go download.

Click the button below to download the Go installer.

Don't see your operating system here? Try one of the [other downloads](https://golang.org/dl/).

## 2\. Go install.

Select the tab for your computer's operating system below, then follow its installation instructions.

Linux Mac Windows

If you have a previous version of Go installed, be sure to [remove it](https://golang.org/doc/manage-install) before installing another.

1.  Download the archive and extract it into /usr/local, creating a Go tree in /usr/local/go.
    
    For example, run the following as root or through `sudo`:
    
    tar -C /usr/local -xzf go1.15.6.linux-amd64.tar.gz
    
2.  Add /usr/local/go/bin to the `PATH` environment variable.
    
    You can do this by adding the following line to your $HOME/.profile or /etc/profile (for a system-wide installation):
    
    export PATH=$PATH:/usr/local/go/bin
    
    **Note:** Changes made to a profile file may not apply until the next time you log into your computer. To apply the changes immediately, just run the shell commands directly or execute them from the profile using a command such as `source $HOME/.profile`.
    
3.  Verify that you've installed Go by opening a command prompt and typing the following command:
    
    $ go version
    

## 3\. Go code.

You're set up! Visit the [Getting Started tutorial](https://golang.org/doc/tutorial/getting-started.html) to write some simple Go code. It takes about 10 minutes to complete.

![The Go Gopher](https://golang.org/lib/godoc/images/footer-gopher.jpg)