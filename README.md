# Python Tutorial for the Riggleman Lab

This tutorial is intended to run on the `rrlogin` cluster, but it can run on your local machine or another remote server if you want, possibly with some minor modifications.

## 1 Startup

### 1.0 Clone this Repo

To do this, just copy and run the following command wherever you want these files to download to on `rrlogin` (or your machine of choice):

    git clone https://github.com/benlindsay/rrlab-python-tutorial

### 1.1 Install Anaconda

Before doing anything else, you'll need to install the Python 3 version of Anaconda on your account in `rrlogin` (or whatever machine you're using). If you think you have it installed, you can check by typing `python` and making sure it says `Python 3.X.X |Continuum Analytics, Inc.|`. For example, mine looks like:

    lindsb@rrlogin:rrlab-python-tutorial$ python
    Python 3.6.2 |Continuum Analytics, Inc.| (default, Jul 20 2017, 13:51:32)
    [GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>>


Anaconda is a free Python distribution that comes with almost all the Python modules you'll need out of the box, and makes adding/updating any other modules very easy. On `rrlogin`, you can find the installation script at `/opt/share/lindsb/Anaconda3-5.1.0-Linux-x86_64.sh`. If you're on any other machine, just download the installation file from [https://www.anaconda.com/download](https://www.anaconda.com/download) To install Anaconda to your account, just `ssh` into `rrlogin`, then run that script:

    mydesktop:~$ ssh myusername@rrlogin.seas.upenn.edu
    myusername@rrlogin:~$ bash /opt/share/lindsb/Anaconda3-5.1.0-Linux-x86_64.sh

That will ask for a few things from you:

1. Accept terms of service
2. Where to put the Anaconda files. By default it will store them in `~/anaconda3/`, but you can change that to whatever you want. Doesn't really matter.
3. Permission to add a line to your `.bashrc` file that adds the Anaconda folder to your `PATH`. Say yes.

At that point, just source your `.bashrc` file by typing `source .bashrc` on the command line, and you should be set to go! To verify that things are working as expected, type `which python`, and it should spit out `~/anaconda3/bin/python` unless you set different Anaconda path.

### 1.2 Install Jupyter Lab

[Jupyter Lab](https://github.com/jupyterlab/jupyterlab) is a great environment that includes a file browser, and lets you open tabs for jupyter notebooks, text files, or even open command line terminals.

As of 4/23/2018, it doesn't come preloaded in the default Anaconda installation, but you can install with a simple `conda install -c conda-forge jupyterlab`.

### 1.3 Add Command-line Utility Functions

If you're running on a local machine, you just need to type `jupyter lab` to start running a jupyter notebook server in your browser. I like to run my notebooks directly on `rrlogin` to avoid having to move my data around, but there are a couple extra steps we have to do to make that work smoothly.

To make this work, we have to run `jupyter lab --no-browser --port=8888` on `rrlogin` to start a browserless jupyter server remotely. Once that's running, we can connect to that server in our browser by running `ssh -N -L localhost:8888:localhost:8888 username@rrlogin.seas.upenn.edu` on your local machine. Then you can type `localhost:8888` in your favorite web browser and it should open up a page that shows your current working directory on `rrlogin`.

To make this process easier, add the following line to your `.bashrc` on `rrlogin`:

    alias jlremote='jupyter lab --no-browser --port=8888'

And add this to your local `.bashrc`:

    jllocal () {
        url=$(ssh username@rrlogin.seas.upenn.edu \
              '~/anaconda3/bin/jupyter notebook list' \
              | grep http | awk '{print $1}')
        echo "URL to copy to your browser:"
        echo "$url"
        ssh -Y -N -L 'localhost:8888:localhost:8888' 'lindsb@rrlogin.seas.upenn.edu'
    }

After `source`-ing both of your newly edited `.bashrc`s, the process of starting up a jupyter notebook server is now:

1. Type `jlremote` on rrlogin
2. Type `jllocal` on local machine
3. Copy the url that the `jllocal` command spits out to your browser

If you have the `screen` or `tmux` utilities available (`tmux` is available on rrlogin), you can type, for example, `tmux`, to open a persistent subshell, run `jlremote`, detach with `CTRL-b, d`, and from then on, any time in the future that you want to open a jupyter lab session on rrlogin, you simply need to type `jllocal` on your local computer, then copy the url to your browser.

### 1.4 Open `0_Intro_to_Jupyter_Notebooks.ipynb`

Once you're there, you can just navigate to whatever directory you cloned this tutorial into and click on `0_Intro_to_Jupyter_Notebooks.ipynb`
