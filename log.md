[TOC]


# UBUNTU 22.04

## Install system

* Delete all existing partitions on Samsung T7
* Reboot from external USB
* `Install Ubuntu`
* `Normal installation` and `Install third-party software`
* `Something else`
* New partitions on Samsung T7 as follows: 
  * `/dev/sdc1` `108MB` `Primary` `Beginning of this space` `EFI System Partition`
  * `/dev/sdc2` `500007MB` `Primary` `Beginning of this space` `EXT4 File System` `/`
* Device for boot: `/dev/sdc1`
* Personalization
  * your name: `ColinFX`
  * computer name: `ColinFX-Ubuntu2204`
  * password: `12345678`

## Virgin snapshot

* Snapshot of home folder
```bash
colinfx@ColinFX-Ubuntu2204:~$ ls -a
.             .bashrc  Desktop    .local    .profile  Templates
..            .cache   Documents  Music     Public    Videos
.bash_logout  .config  Downloads  Pictures  snap
```
* Show current terminal shell
```bash
colinfx@ColinFX-Ubuntu2204:~$ echo "$SHELL"
/bin/bash
```
* Show current python version
```bash
colinfx@ColinFX-Ubuntu2204:~$ python3 --version
Python 3.10.6
```
* Check all installed python versions
```bash
colinfx@ColinFX-Ubuntu2204:~$ compgen -c python | sort -u | grep -v -- '-config$' | while read -r p; do
     printf "%-14s  " "$p"
     "$p" --version
done
python3         Python 3.10.6
python3.10      Python 3.10.6
python3-futurize  0.18.2
python3-pasteurize  0.18.2
```

## Ubuntu Software setup

* Update all software from `Ubuntu Software`
* **Error** `Unable to update "Snap Store": snap "snap-store" has running apps`
* **Try** update all packages from terminal
```bash
colinfx@ColinFX-Ubuntu2204:~$ killall snap-store
colinfx@ColinFX-Ubuntu2204:~$ sudo apt-get update && sudo apt-get upgrade
colinfx@ColinFX-Ubuntu2204:~$ reboot
```
* **Failed, Unsolved** error still exists

## Uninstall useless pre-installed applications

* Uninstall useless pre-installed applications from `Ubuntu Software`: AisleRiot Solitaire, Mahjongg, Mines, Sudoku
* *Error* `Unable to remove "AisleRiot Solitaire": no packages to remove`
* *Try* uninstall apps by `snap` command in terminal
```bash
colinfx@ColinFX-Ubuntu2204:~$ sudo snap list
colinfx@ColinFX-Ubuntu2204:~$ sudo snap remove aisleriot mahjongg mines sudoku
```
* *Failed* `snap "aisleriot" is not installed`
* *Try* uninstall apps by `apt` command in terminal
```bash
colinfx@ColinFX-Ubuntu2204:~$ sudo apt list *aisleriot*
aisleriot/jammy,now 1:3.22.22-1 amd64 [installed,automatic]
colinfx@ColinFX-Ubuntu2204:~$ sudo apt list *mahjongg*
gnome-mahjongg/jammy,now 1:3.38.3-2 amd64 [installed,automatic]
colinfx@ColinFX-Ubuntu2204:~$ sudo apt list *mines*
gnome-mines/jammy,now 1:40.1-1 amd64 [installed,automatic]
colinfx@ColinFX-Ubuntu2204:~$ sudo apt list *sudoku*
gnome-sudoku/jammy,now 1:42.0-1 amd64 [installed,automatic]
colinfx@ColinFX-Ubuntu2204:~$ sudo apt remove aisleriot
colinfx@ColinFX-Ubuntu2204:~$ sudo apt remove gnome-mahjongg
colinfx@ColinFX-Ubuntu2204:~$ sudo apt remove gnome-mines
colinfx@ColinFX-Ubuntu2204:~$ sudo apt remove gnome-sudoku
```
* Log out and relog in to refresh installed list in `Ubuntu Software` (reported bug)
* *Solved*

## Chinese keyboard

* `Settings` - `Language an Region` - `Manage Installed Languages`
* `Install completely language service` as language service initialization
* Add Chinese and French as system language
* Install `ibus-pinyin` from terminal
```bash
colinfx@ColinFX-Ubuntu2204:~$ sudo apt-get install ibus-pinyin
colinfx@ColinFX-Ubuntu2204:~$ reboot
```
* `Settings` - `Keyboard` add Chinese (Intelligent Pinyin) and French keyboard
* *Error* unable to type in Chinese characters
* Reboot again
* *Solved*

## Reallocate swap file

* Show all existing swap file
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo swapon --show
NAME      TYPE SIZE USED PRIO
/swapfile file   2G   0B   -2
```
* Turn off all existing swap file
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo swapoff -a
```
* Reallocate swap file size
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo fallocate -l 20G /swapfile
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo chmod 600 /swapfile
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo mkswap /swapfile
mkswap: /swapfile: warning: wiping old swap signature.
Setting up swapspace version 1, size = 20 GiB (21474832384 bytes)
no label, UUID=0c68e32a-5f70-4111-b45f-cd6240e34cec
```
* Activate newly allocated swap file
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo swapon /swapfile
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo swapon --show
NAME      TYPE SIZE USED PRIO
/swapfile file  20G   0B   -2
(base) colinfx@ColinFX-Ubuntu2204:~$ reboot
```

## Install applications

* Install `code`, `DataSpell`, `DataGrip` and `slack` from `Ubuntu Software`
* Install `Vim` from terminal
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install vim
```
* Install `Steam` from terminal
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install steam
```
* Install `zoom-client` from `Ubuntu Software`
  * **Error, unsolved** virtual background is not working, as well as Cheese

## Hide folders without renaming

* Create a new file `.hidden` in the repository and add name of folders to be hidden in the file

```
Zomboid
```

## Show seconds in toolbar

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ gsettings set org.gnome.desktop.interface clock-show-seconds true
(base) colinfx@ColinFX-Ubuntu2204:~$ reboot
```

* *Failed*, try GUI way

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install gnome-tweaks
```

* Open `Tweaks` and `Top Bar` - `Seconds`
* *Solved*

# MARKDOWN

## VSCode with Markdown

* Install `Markdown All in One` plugin by `Yu Zhang` in `VSCode`
* To open preview, use `Ctrl`+`Shift`+`V`
* To insert automatic table of contents, use command `Markdown All in One: Create Table of Contents` in the VSCode command palette (use `Ctrl`+`Shift`+`P`)

## Typora

* Install `Typora` from `Ubuntu Software`
* To export a markdown file as PDF, use `File` - `Export` in Typora


# PYTHON

## Install Anaconda

* Preparation
```bash
colinfx@ColinFX-Ubuntu2204:~$ sudo apt install curl
colinfx@ColinFX-Ubuntu2204:~$ sudo apt update && sudo apt upgrade
```
* Download `Anaconda3-2022.05-Linux-x86_64.sh` from `https://repo.anaconda.com/archive/` to `~/Downloads`
* Check file SHA-256 code and compare to the archive site
```bash
colinfx@ColinFX-Ubuntu2204:~/Downloads$ sha256sum Anaconda3-2022.05-Linux-x86_64.sh
```
* Install
```bash
colinfx@ColinFX-Ubuntu2204:~/Downloads$ bash Anaconda3-2022.05-Linux-x86_64.sh
Anaconda3 will now be installed into this location:
/home/colinfx/anaconda3

Installed package of scikit-learn can be accelerated using scikit-learn-intelex.
    More details are available here: https://intel.github.io/scikit-learn-intelex
    
    For example:
        $ conda install scikit-learn-intelex
        $ python -m sklearnex my_application.py
       
installation finished.
Do you wish the installer to initialize Anaconda3
by running conda init? [yes|no]
[no] >>> yes
no change     /home/colinfx/anaconda3/condabin/conda
no change     /home/colinfx/anaconda3/bin/conda
no change     /home/colinfx/anaconda3/bin/conda-env
no change     /home/colinfx/anaconda3/bin/activate
no change     /home/colinfx/anaconda3/bin/deactivate
no change     /home/colinfx/anaconda3/etc/profile.d/conda.sh
no change     /home/colinfx/anaconda3/etc/fish/conf.d/conda.fish
no change     /home/colinfx/anaconda3/shell/condabin/Conda.psm1
no change     /home/colinfx/anaconda3/shell/condabin/conda-hook.ps1
no change     /home/colinfx/anaconda3/lib/python3.9/site-packages/xontrib/conda.xsh
no change     /home/colinfx/anaconda3/etc/profile.d/conda.csh
modified      /home/colinfx/.bashrc

==> For changes to take effect, close and re-open your current shell. <==

If you'd prefer that conda's base environment not be activated on startup, 
   set the auto_activate_base parameter to false: 

conda config --set auto_activate_base false

Thank you for installing Anaconda3!
```
* Attention! New contents added to `~/.bashrc` from `l.119`. The backup of the original file is stored as `~/.bashrc(copy)`. The new file will automatically activates conda environment for every new shell, added contents are below:
```bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/colinfx/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/colinfx/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/home/colinfx/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/colinfx/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```
* Activate conda environment, `(base)` appeared, check conda version and python version
```bash
colinfx@ColinFX-Ubuntu2204:~$ source .bashrc
(base) colinfx@ColinFX-Ubuntu2204:~$ conda --version
conda 4.12.0
(base) colinfx@ColinFX-Ubuntu2204:~$ python --version
Python 3.9.12
```
* Check new python versions
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ compgen -c python | sort -u | grep -v -- '-config$' | while read -r p; do
     printf "%-14s  " "$p"
     "$p" --version
done
python          Python 3.9.12
python3         Python 3.9.12
python3.10      Python 3.10.6
python3.9       Python 3.9.12
python3-futurize  0.18.2
python3-pasteurize  0.18.2
```
* Open Anaconda Navigator GUI
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ anaconda-navigator
```

## Add Anaconda Navigator to applications list 

* Create desktop shortcut file
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ gedit ~/.local/share/applications/anaconda.desktop
```
* Edit file in the opened window and add contents below: 
```
[Desktop Entry]
Version=1.0
Type=Application
Name=Anaconda
Exec=/home/colinfx/anaconda3/bin/anaconda-navigator
Icon=/home/colinfx/anaconda3/lib/python3.9/site-packages/anaconda_navigator/app/icons/Icon1024.png
Terminal=false
```

## Anaconda virgin snapshot

* Pre-installed applications under `base(root)` environment: JupyterLab, Notebook, Qt Console, Spyder, VS Code, Datalore, IBM Watson Studio Cloud, Oracle Data Science Service
* Check conda environments
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ conda env list
# conda environments:
base                  *  /home/colinfx/anaconda3
```
* Snapshot of home folder
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ ls -a
 .                 .cache       .gnupg             .profile
 ..                .conda       .ipython           Public
 .anaconda         .condarc     .local             snap
 anaconda3         .config      Music              .ssh
 .bash_history     .continuum   .nv                .sudo_as_admin_successful
 .bash_logout      Desktop      .pam_environment   Templates
 .bashrc           Documents    Pictures           Videos
'.bashrc (copy)'   Downloads    .pki               .vscode
```

## Anaconda with Jupyter Notebook

* `Jupyter Notebook` already installed in `base(root)` environment
* Install `Notebook extensions`
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ conda install -c conda-forge jupyter_contrib_nbextensions
```

## Anaconda with Jupyter Notebook Quickstart

* To run the `Jupyter Notebook`, click `Jupyter Notebook` under `base(root)` in `Anaconda Navigator`...
* ... or simply run it from terminal
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ jupyter notebook
```
* To end the `Jupyter Notebook`, close the tab in firefox and stop the server in terminal...
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ jupyter notebook list
Currently running servers:
http://localhost:8888/?token=12d3a7f1682b21497adf348a8b228f024985fcae98a4e28a :: /home/colinfx
(base) colinfx@ColinFX-Ubuntu2204:~$ jupyter notebook stop 8888
Shutting down server on 8888...
```
* ... or simply click on `Quit` on the tab in firefox

## Anaconda environments with Jupyter Notebook

* For the sake of testing, create a conda environment
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ conda create --name test python=3.9.12 ipykernel
## Package Plan ##
  environment location: /home/colinfx/anaconda3/envs/test
  added / updated specs:
    - ipykernel
    - python=3.9.12
(base) colinfx@ColinFX-Ubuntu2204:~$ conda env list
# conda environments:
base                  *  /home/colinfx/anaconda3
test                     /home/colinfx/anaconda3/envs/test
```
* Create python kernel for the new environment
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ conda activate test
(test) colinfx@ColinFX-Ubuntu2204:~$ ipython kernel install --user --name=test
Installed kernelspec test in /home/colinfx/.local/share/jupyter/kernels/test
```
* To list all installed python kernels:
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ jupyter kernelspec list
```
* Launch `Jupyter Notebook` under `base(root)`
```bash
(test) colinfx@ColinFX-Ubuntu2204:~$ conda deactivate
(base) colinfx@ColinFX-Ubuntu2204:~$ jupyter-notebook
```
* Now select different kernels in `Notebook` tab
* To delete the kernel: 
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ jupyter kernelspec uninstall test
```
* To delete the new conda environment:
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ conda env remove --name test
```
* Remark! [Other possible solutions](https://towardsdatascience.com/get-your-conda-environment-to-show-in-jupyter-notebooks-the-easy-way-17010b76e874)

## Run Anaconda environment with Jupyter Notebook in PyCharm 

- In `Anaconda navigator`, install package `jupyter` for the environment
- Start new `.ipynb` file in PyCharm and the Jupyter kernel will automatically start

## PyTorch Environment with Anaconda

* Create environment and install [pytorch](https://pytorch.org/get-started/locally/)
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ conda create --name=torch python=3.9.12 ipykernel
(base) colinfx@ColinFX-Ubuntu2204:~$ conda activate torch
(torch) colinfx@ColinFX-Ubuntu2204:~$ conda install pytorch torchvision torchaudio cudatoolkit=11.6 -c pytorch -c conda-forge
(torch) colinfx@ColinFX-Ubuntu2204:~$ conda install scikit-learn
(torch) colinfx@ColinFX-Ubuntu2204:~$ ipython kernel install --user --name=torch
Installed kernelspec torch in /home/colinfx/.local/share/jupyter/kernels/torch
```

## VSCode with Anaconda

* Install `Python`, `Pylance`, `Jupyter`, `Jupyter Keymap` and `Jupyter Notebook Renderers` in `VSCode`
* Select Conda environment from bottom-right corner button
  * base interpreter: ~/anaconda3/bin/python
  * torch interpreter: ~/anaconda3/envs/torch/bin/python
* *Info* We noticed you're using a conda environment. If you are experiencing issues with this environment in the integrated terminal, we recommend that you let the Python extension change "terminal.integrated.inheritEnv" to false in your user settings. -> `No`

## VSCode Collaborating

* Install `Live Share` extension by `Microsoft`
* Login with GitHub account `xiao.fei@polytechnique.edu`
* Copy the link `https://prod.liveshare.vsengsaas.visualstudio.com/join?5B6CB51A711BB2D7E15A66BB3A1175F9E46C`
* To use the link, press `Ctrl`+`Shift`+`P` in `VSCode` an select `Join COllaboration Session`

## Pycharm-Professional with Anaconda

* Install `Pycharm-Professional` from `Ubuntu Software`
* Add Conda interpreter
  * `Previously configured interpreter` -> `Add interpreter` -> `Add local interpreter`
  * `Conda ENvironment` -> `Interpreter`
  * To add `base`, select `~/anaconda3/bin/python`
  * To add `torch`, select `~/anaconda3/envs/torch/bin/python`
* Auto `Updating indexes` and `Updating Python interpreter`

## DataSpell for Jupyter Notebook

* Install `DataSpell` from `Ubuntu Software`
* Select default environment as the base conda environment from `/home/colinfx/anaconda3/bin/python`
* Open directly jupyter notebook files

# SQL

## Install PostgreSQL-12

* Tutorial from [here](https://computingforgeeks.com/install-postgresql-12-on-ubuntu/)
* Import GPG key
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```
* Add repository contents to system
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
```
* Install
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt update
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt -y install postgresql-12 postgresql-client-12
```
* Verify the installed version
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ psql --version
psql (PostgreSQL) 12.12 (Ubuntu 12.12-1.pgdg22.04+1)
```

## PostgreSQL-12 Quickstart

* Tutorial from [here](https://linuxhint.com/start-postgresql-linux/)
* To verify the active status of `PostgreSQL`
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo systemctl status postgresql
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor pr>
     Active: active (exited) since Tue 2022-09-27 21:00:58 CEST; 1min 40s ago
    Process: 24855 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 24855 (code=exited, status=0/SUCCESS)
        CPU: 2ms
```
* Reboot to apply changes
* To start `PostgreSQL` from terminal:
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo -i -u postgres
postgres@ColinFX-Ubuntu2204:~$ psql
psql (12.12 (Ubuntu 12.12-1.pgdg22.04+1))
Type "help" for help.
postgres=# 
```
* To show all tables: 
```bash
postgres=# \dt
           List of relations
 Schema |    Name    | Type  |  Owner   
--------+------------+-------+----------
 public | heldoffice | table | postgres
 public | office     | table | postgres
 public | person     | table | postgres
(3 rows)
```
* To quit the process: 
```bash
postgres=# \q
postgres@ColinFX-Ubuntu2204:~$ exit
logout
(base) colinfx@ColinFX-Ubuntu2204:~$ 
```

## Permission management trouble shooting

* *Error*, unable to import file from user directory 
```bash
postgres=# \copy geonames FROM '/home/colinfx/Dev/polytechnique/inf553/data/dossier-geo/lieux_notables.csv' CSV header;
/home/colinfx/Dev/polytechnique/inf553/data/dossier-geo/lieux_notables.csv: Permission denied
```
* *Try*, may import from root directory
```bash
postgres=# \copy geonames FROM '/tmp/lieux_notables.csv' CSV header;
ERROR:  duplicate key value violates unique constraint "geonames_pkey"
DETAIL:  Key (geonameid)=(2659086) already exists.
CONTEXT:  COPY geonames, line 2
```
* *Problem* The server automatically activate during installation is running as `postgres` user but the file is in the personal folder of `colinfx`

* Check current server status and change to `postgres` user

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo systemctl status postgresql
[sudo] password for colinfx: 
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor pr>
     Active: active (exited) since Mon 2022-10-24 18:14:03 CEST; 1h 50min left
    Process: 1680 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 1680 (code=exited, status=0/SUCCESS)
        CPU: 4ms

Oct 24 18:14:03 ColinFX-Ubuntu2204 systemd[1]: Starting PostgreSQL RDBMS...
Oct 24 18:14:03 ColinFX-Ubuntu2204 systemd[1]: Finished PostgreSQL RDBMS.
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo su postgres
```

* Try `pg_ctl` command to verify system path

```bash
postgres@ColinFX-Ubuntu2204:/home/colinfx$ pg_ctl --help
pg_ctl: command not found
```

* Add system path to run `/usr/lib/postgresql/12/bin/pg_ctl`

```bash
postgres@ColinFX-Ubuntu2204:/home/colinfx$ PATH=$PATH:/usr/lib/postgresql/12/bin && export PATH
postgres@ColinFX-Ubuntu2204:/home/colinfx$ pg_ctl --help
could not change directory to "/home/colinfx": Permission denied
pg_ctl is a utility to initialize, start, stop, or control a PostgreSQL server.
postgres@ColinFX-Ubuntu2204:/home/colinfx$ cd ~
```

* Show all running data clusters

```bash
postgres@ColinFX-Ubuntu2204:~$ pg_lsclusters
Ver Cluster Port Status Owner    Data directory              Log file
12  main    5432 online postgres /var/lib/postgresql/12/main /var/log/postgresql/postgresql-12-main.log
```

* Shut down the current server

```bash
postgres@ColinFX-Ubuntu2204:~$ pg_ctl stop -D /var/lib/postgresql/12/main
waiting for server to shut down.... done
server stopped
postgres@ColinFX-Ubuntu2204:~$ pg_lsclusters
Ver Cluster Port Status Owner    Data directory              Log file
12  main    5432 down   postgres /var/lib/postgresql/12/main /var/log/postgresql/postgresql-12-main.log
postgres@ColinFX-Ubuntu2204:~$ systemctl status postgresql
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor pr>
     Active: active (exited) since Mon 2022-10-24 18:14:03 CEST; 1h 28min left
    Process: 1680 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 1680 (code=exited, status=0/SUCCESS)
        CPU: 4ms
```

* Create new work space

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ cd Dev/polytechnique/inf553
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf553$ mkdir data
```

* Initialise the database directory

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf553$ /usr/lib/postgresql/12/bin/initdb ./data
The files belonging to this database system will be owned by user "colinfx".
This user must also own the server process.

The database cluster will be initialized with locale "en_GB.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory ./data ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... Europe/Paris
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

initdb: warning: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    /usr/lib/postgresql/12/bin/pg_ctl -D ./data -l logfile start
```

* Start the new server

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf553$ /usr/lib/postgresql/12/bin/pg_ctl -D ./data -l ./log start
waiting for server to start.... stopped waiting
pg_ctl: could not start server
Examine the log output.
```

* **Error, unsolved** examine the log output

```
2022-10-24 16:56:24.718 CEST [13715] LOG:  starting PostgreSQL 12.12 (Ubuntu 12.12-1.pgdg22.04+1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 11.2.0-19ubuntu1) 11.2.0, 64-bit
2022-10-24 16:56:24.719 CEST [13715] LOG:  listening on IPv4 address "127.0.0.1", port 5432
2022-10-24 16:56:24.725 CEST [13715] FATAL:  could not create lock file "/var/run/postgresql/.s.PGSQL.5432.lock": Permission denied
2022-10-24 16:56:24.726 CEST [13715] LOG:  database system is shut down
```

* Display current user and groups

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf553$ id
uid=1000(colinfx) gid=1000(colinfx) groups=1000(colinfx),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),122(lpadmin),134(lxd),135(sambashare)
```

* Show `/var/run/postgresql/` owner user and group

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf553$ ls -l /var/run/
total 24
drwxrwsr-x  3 postgres            postgres              60 Oct 24 16:44 postgresql
```

* Add `colinfx` to `postgres` group

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf553$ sudo usermod -a -G postgres colinfx
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf553$ id
uid=1000(colinfx) gid=1000(colinfx) groups=1000(colinfx),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),122(lpadmin),134(lxd),135(sambashare)
```

* **Attention** Need to shut down the cluster every time after reboot



# JAVA

## Install OpenJDK

* Install Default-JDK
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install default-jdk
```
* Verify JRE and JDK versions
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ java -version
openjdk version "11.0.16" 2022-07-19
OpenJDK Runtime Environment (build 11.0.16+8-post-Ubuntu-0ubuntu122.04)
OpenJDK 64-Bit Server VM (build 11.0.16+8-post-Ubuntu-0ubuntu122.04, mixed mode, sharing)
(base) colinfx@ColinFX-Ubuntu2204:~$ javac -version
javac 11.0.16
```

## Install Eclipse

* Install `Eclipse` from `Ubuntu Software`


# C/C++

## Install compilers

* Install `build-essential` metapackage containing `gcc`, `gpp`, `libc6-dev`, `make` and `dpkg-dev`
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install build-essential
```

## CLion with C/C++

* Install `CLion` from `Ubuntu Software`
* Open a new project and set open project wizard as default configuration
  * CMake: Bundled (Version: 3.23.2)
  * Build Tool: Detected: ninja
  * C Compiler: Detected: cc
  * C++ Compiler: Detected: c++
  * Debugger: Bundled GDB (Version: 12.1)

## Check local C++ standrad version

* Create a C++ file and run:
```
#include <iostream>
int main() {
    std::cout << __cplusplus << std::endl;
    return 0;}
```
* The result `201402` is indicating `C++14` standard
* Other possible results are: `201703` for `C++17`, `201103` for `C++11` an `199711` for `C++98`

## CMake quickstart

* These commands are tested under the online JupyterLab MAP579 by CNRS
* Under current directory we have: 

```
CMakeLists.txt
main.cpp
resource.cpp
resource.hpp
```

* Now to build the project: 

```bash
(base) jovyan@jupyter-xiao-2efei-40polytechnique-2eedu:~/persistent/lab6/static/step_0$ cmake -B build -S .
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /opt/conda/bin/x86_64-conda-linux-gnu-cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /opt/conda/bin/x86_64-conda-linux-gnu-c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/jovyan/persistent/lab6/static/step_0/build
(base) jovyan@jupyter-xiao-2efei-40polytechnique-2eedu:~/persistent/lab6/static/step_0$ cmake --build build
make: Warning: File 'Makefile' has modification time 239 s in the future
make[1]: Warning: File 'CMakeFiles/Makefile2' has modification time 239 s in the future
make[2]: Warning: File 'CMakeFiles/main.dir/flags.make' has modification time 239 s in the future
make[2]: warning:  Clock skew detected.  Your build may be incomplete.
make[2]: Warning: File 'CMakeFiles/main.dir/flags.make' has modification time 239 s in the future
[ 25%] Building CXX object CMakeFiles/main.dir/main.cpp.o
[ 50%] Building CXX object CMakeFiles/main.dir/resource.cpp.o
[ 75%] Building CXX object CMakeFiles/main.dir/manager.cpp.o
[100%] Linking CXX executable main
make[2]: warning:  Clock skew detected.  Your build may be incomplete.
[100%] Built target main
make[1]: warning:  Clock skew detected.  Your build may be incomplete.
make: warning:  Clock skew detected.  Your build may be incomplete.
```

* To run the project: 

```bash
(base) jovyan@jupyter-xiao-2efei-40polytechnique-2eedu:~/persistent/lab6/static/step_0$ ./build/main
resource constructor
begin of main
construct manager
resource acquire: 0
resource acquire: 1
end of main
destroy manager
resource destructor
```

* A general self-made C++ library should be organised in such structure: 

```
CMakeLists.txt
	examples
		CMakeLists.txt
		main.cpp
    include
    	generative
    		core.hpp
    		shape.hpp
    		...
    src
    	CMakeLists.txt
    	core.cpp
    	shape.cpp
    	...
```

 

# R

## Install R

- Install R and other prerequisites from terminal

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt update
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt -y install r-base gdebi-core
```

- Check installed version

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ R --version
R version 4.1.2 (2021-11-01) -- "Bird Hippie"
Copyright (C) 2021 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under the terms of the
GNU General Public License versions 2 or 3.
For more information about these matters see
https://www.gnu.org/licenses/.
```

## Install RStudio IDE

- Download RStudio `.deb` package of the corresponding Ubuntu version from [posit.co](https://posit.co/download/rstudio-desktop/)
- Autoinstall RStudio and related packages from terminal

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Downloads$ sudo gdebi rstudio-2022.12.0-353-amd64.deb 
```

# LATEX

## Install Texlive

* Instll full texlive package
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt-get install texlive-full
```

## VSCode with LaTeX

* Install `LaTeX Workshop` extension by `James Yu` in VSCode


# NVIDIA

## Install Nvidia-smi

* Check display hardware
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo lshw -C display
  *-display UNCLAIMED       
       description: 3D controller
       product: GP107M [GeForce GTX 1050 Ti Mobile]
       vendor: NVIDIA Corporation
       physical id: 0
       bus info: pci@0000:01:00.0
       version: a1
       width: 64 bits
       clock: 33MHz
       capabilities: pm msi pciexpress bus_master cap_list
       configuration: latency=0
       resources: memory:ec000000-ecffffff memory:c0000000-cfffffff memory:d0000000-d1ffffff ioport:3000(size=128) memory:ed000000-ed07ffff
  *-display
       description: VGA compatible controller
       product: CoffeeLake-H GT2 [UHD Graphics 630]
       vendor: Intel Corporation
       physical id: 2
       bus info: pci@0000:00:02.0
       logical name: /dev/fb0
       version: 00
       width: 64 bits
       clock: 33MHz
       capabilities: pciexpress msi pm vga_controller bus_master cap_list rom fb
       configuration: depth=32 driver=i915 latency=0 resolution=1920,1080
       resources: irq:135 memory:eb000000-ebffffff memory:80000000-8fffffff ioport:4000(size=64) memory:c0000-dffff
```
* Open `Software Updater` and start auto partial upgrade
```
Install(1)
	linux-modules-nvidia-510-5.15.0-48-generic
No longer needed(1)
	linux-modules-nvidia-510-5.15.0-25-generic
```
* Reboot
* Open `Software Updater` and confirm that all softwares are up-to-date
* Open `Software & Updates` - `Additional Drivers`
* Select `Using NVIIA river metapackage from nvidia-river-515 (proprietary, tested)`
* Reboot
* To check graphic card status: 
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ nvidia-smi
Mon Oct  3 20:26:40 2022       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 515.65.01    Driver Version: 515.65.01    CUDA Version: 11.7     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   59C    P8    N/A /  N/A |      4MiB /  4096MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      1495      G   /usr/lib/xorg/Xorg                  4MiB |
+-----------------------------------------------------------------------------+
```
* For more settings, open newly installed `NVIDIA X Server Settings`

## Real time Nvidia-smi monitoring

- Run in terminal the command below, modify the number `1` for other refresh rate

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ watch -n1 nvidia-smi
```




# SJTU VPN

## Installation

* Tutorial from [here](https://net.sjtu.edu.cn/info/1200/2665.htm)
* Install `strongswan`
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt update && sudo apt upgrade
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install strongswan strongswan-swanctl
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install libstrongswan-extra-plugins libcharon-extra-plugins
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install resolvconf curl
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo ln -s /etc/ssl/certs/* /etc/ipsec.d/cacerts/
```
* Edit `/etc/ipsec.conf` file to add contents: 
```
conn "sjtu-staff"
    keyexchange=ikev2
    left=%config
    leftsourceip=%config4,%config6
    leftauth=eap-peap

    ike=aes256-sha1-modp1024,3des-sha1-modp1024!
    esp=aes128-sha2_256-modp1024,3des-sha1-modp1024!

    right=vpn.sjtu.edu.cn
    rightid=%any

    rightsubnet=0.0.0.0/0,2000::/3

    rightauth=pubkey
    eap_identity="ColinFX" # jAccount ID
    auto=add
    aaa_identity="@radius.net.sjtu.edu.cn"
    
conn "sjtu-student"
    keyexchange=ikev2
    left=%config
    leftsourceip=%config4,%config6
    leftauth=eap-peap

    right=stu.vpn.sjtu.edu.cn
    rightid=@stu.vpn.sjtu.edu.cn

    rightsubnet=0.0.0.0/0,2000::/3
    rightauth=pubkey
    eap_identity="ColinFX" # jAccount ID

    auto=add
    aaa_identity="@radius.net.sjtu.edu.cn"
```
* Edit `/etc/ipsec.secrets` to add contents, complete `******` as jAccount password: 
```
"ColinFX" : EAP "******" # jAccount Password
```
* Reread ipsec config file
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo ipsec rereadall
```
* Reboot

## Quickstart

* To connect to VPN:
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo ipsec up "sjtu-student"
```
* To disconnect: 
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo ipsec down "sjtu-student"
```

# Telecom-Paris VPN

- Tutorial from [here](https://eole.telecom-paris.fr/vos-services/services-numeriques/connexions-aux-reseaux/openvpn-avec-ubuntu)
- Install `OpenVPN`:

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt-get install network-manager-openvpn-gnome openvpn
```

- Download `telecom-paris.ovpn` from [here](https://vpn.telecom-paris.fr/linux/telecom-paris.ovpn)
- `Settings` - `Network` - `VPN` - `Add VPN` - `Import from file` and select the downloaded setup file
- Enter username and password in the authentication section
- Connect from dashboard or within settings

# SPEIT-IE Server

## Connect to server from terminal

* Activate SJTU-VPN
* Run in terminal with password `123456`: 

```bas
(base) colinfx@ColinFX-Ubuntu2204:~$ ssh -p 51600 xiaofei@202.120.55.134
```

## Keep terminal running after logout: `tmux`

* Install `tmux` by `apt`
* To start a new shell: 

```bash
xiaofei@speit-ie:~/DCTweet2022$ tmux
```

* To connect to last shell: 

```bash
xiaofei@speit-ie:~/DCTweet2022$ tmux attach
```

* To leave current shell, press `Ctrl`+ `b` and then `d`

## Remote development via PyCharm

* `PyCharm` - `File` - `Remote Development`
* Select `Connect to SSH` and enter Username: `xiaofei`, Host: `202.120.55.134`, Port: `51600`
* Enter password
* Auto detect IDE and select project directory

## Check remaining disk space

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ df -h
```

# POLYTECHNIQUE EDUROAM

## Set eduroam from Settings

* `Settings` - `Wi-Fi`
* Select `eduroam` and set options, complete `******` as X identification password: 
  * Security: WPA nd WPA2 entreprise
  * Authentication: Tunnelled TLS
  * Inner authentication: PAP
  * Anonymous identity: `anonymous@polytechnique.fr`
  * Domain: (None)
  * No CA certificate is required: Yes
  * Username: `xiao.fei@polytechnique.fr`
  * Password: `******`

# GITHUB

## Setup

* Install git

```bash	
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install git
```

* Set username and email

```bash	
(base) colinfx@ColinFX-Ubuntu2204:~$ git config --global user.name "ColinFX"
(base) colinfx@ColinFX-Ubuntu2204:~$ git config --global user.email "xiao.fei@polytechnique.edu"
(base) colinfx@ColinFX-Ubuntu2204:~$ git config --global user.name
ColinFX
(base) colinfx@ColinFX-Ubuntu2204:~$ git config --global user.email
xiao.fei@polytechnique.edu
```

## Add SSH key

* Check for existing SSH keys, look for any file in `.pub` format

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf554/kaggle$ ls -al ~/.ssh
total 12
drwx------  2 colinfx colinfx 4096 Nov 14 14:24 .
drwxr-x--- 44 colinfx colinfx 4096 Nov 14 14:11 ..
-rw-r--r--  1 colinfx colinfx  142 Nov 14 14:24 known_hosts
```

* Generate a new SSH key

```bash	
(base) colinfx@ColinFX-Ubuntu2204:~/Dev/polytechnique/inf554/kaggle$ ssh-keygen -t ed25519 -C "xiao.fei@polytechnique.edu"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/colinfx/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/colinfx/.ssh/id_ed25519
Your public key has been saved in /home/colinfx/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:iGZ/mxAERK3ygthNPZd4rTOq/e+eynxBmLjpxUWNiag xiao.fei@polytechnique.edu
The key's randomart image is:
+--[ED25519 256]--+
|   o+. . . +     |
|     .o . + .    |
|     +.o *       |
|  . Eo=.* +      |
|.o =+ oBS+       |
|o ooo.o.* .      |
|   . .oo.o .     |
|     .o= o..     |
|    ....B*=      |
+----[SHA256]-----+
```

* Copy contents from `/home/colinfx/.ssh/id_ed25519.pub`
* Go to `github.com` - `Settings` - `SSH and GPG keys`
* `New SSH key`
* Paste the contents and confirm

# GNOME EXTENSIONS

## Install Gnome Extension Manager

* Install `Gnome Shell Extension Manager`
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install gnome-shell-extension-manager
```
* Install `Gnome Shell Extensions`
```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install gnome-shell-extensions
```
* To install extensions, open `Extension Manager` and search under `Browse`

## Display net speed

* Install `Net speed Simplified` from `Extension Manager`
* Configure settings
* Reboot

# TeamViewer

## Install TeamViewer

* Install via terminal

```bash
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt update && sudo apt upgrade
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install wget apt-transport-https gnupg2 -y
(base) colinfx@ColinFX-Ubuntu2204:~$ wget -O- https://download.teamviewer.com/download/linux/signature/TeamViewer2017.asc | gpg --dearmor | sudo tee /usr/share/keyrings/teamview.gpg
(base) colinfx@ColinFX-Ubuntu2204:~$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/teamview.gpg] \ http://linux.teamviewer.com/deb stable main" \ | sudo tee /etc/apt/sources.list.d/teamviewer.list
(base) colinfx@ColinFX-Ubuntu2204:~$ sudo apt install teamviewer -y
E: Malformed entry 1 in list file /etc/apt/sources.list.d/teamviewer.list (URI parse)
E: The list of sources could not be read.
```

* *Failed*, try manual install from package
* Download `teamviewer_15.36.8_amd64.deb` from `https://www.teamviewer.com/en/download/linux/` to `~/Downloads`
* Manual install via terminal

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Downloads$ sudo dpkg -i ./teamviewer_15.36.8_amd64.deb 
(Reading database ... 437184 files and directories currently installed.)
Preparing to unpack ./teamviewer_15.36.8_amd64.deb ...
Unpacking teamviewer (15.36.8) over (15.36.8) ...
dpkg: dependency problems prevent configuration of teamviewer:
 teamviewer depends on libminizip1; however:
  Package libminizip1 is not installed.

dpkg: error processing package teamviewer (--install):
 dependency problems - leaving unconfigured
Processing triggers for mailcap (3.70+nmu1ubuntu1) ...
Processing triggers for gnome-menus (3.36.0-1ubuntu3) ...
Processing triggers for desktop-file-utils (0.26-1ubuntu3) ...
Processing triggers for dbus (1.12.20-2ubuntu4.1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Errors were encountered while processing:
 teamviewer
```

* Manual install missing dependency

```bash
(base) colinfx@ColinFX-Ubuntu2204:~/Downloads$ sudo apt install libminizip1
```

* *Solved*

# TO-DO

* postgresql trouble shooting
* intellij
* datagrip JETBRAINS
