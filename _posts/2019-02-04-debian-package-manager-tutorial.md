---
layout: post
title: Ubuntu and Debian Package Management Tutorial
date: '2019-02-04T14:32:17.289492'
author: Sujay S Kumar
tags: 
---

Package management is one of the components that contribute to the steep learning curve of using a linux system for a majority of first time linux users. Coming from the GUI environment of Windows or MacOS where installing/removing applications consist of clicking on _Next_ -> _Next_ -> _I Agree_ -> _Install/Uninstall_, the terminal based package managers might be intimidating. And this issue is not constrained to first time users either. Even though I have used a linux system as my primary OS for several years now, I end up googling the errors and copy-pasting the answers from SO without the slightest idea of what I am doing most of the times. Hence, I am writing this blog post to be a glossary of the commands that we can use to fix our own package errors or to know what each command is doing even if we end up copy-pasting from the internet to fix our problems.

RHEL-based distributions:
- Packaging format: `RPM`
- Tools: `rpm`, `yum`

Debian and Ubuntu based distributions:
- Packaging format: `.deb`
- Tools: `apt`, `apt-get`, `dpkg`

In this blog post, I will be covering the Debian/Ubuntu based distributions, since well, I am a Ubuntu user.

### Debian/Ubuntu Package Management Tools

- `apt-get`: 
    
    `apt-get` is used to interface with remote repositories. Main difference with `apt` is that `apt` pulls from the remote repository into a local cache and then uses this copy in the cache. `apt-get` uses the same methodology but refreshes the cache every time.

- `apt-cache`: 
    
    Like discussed earlier, this is the local cache that `apt` uses.

- `aptitude`: 
    
    Has both capabilities of the above tools. Can be dropped in place of both `apt-get` and `apt-cache`.

- `dpkg`: 
    
    The above tools are used to manage and pull packages from remote repositories. `dpkg` operates on the actual `.deb` files. It cannot resolve dependencies like `apt-*`.

- `tasksel`: 
    
    This is used for grouping of similarly related packages. For eg, if you want a MEAN stack setup, you can use `tasksel` to install all required packages instead of installing each of them individually.

### Commands

1. `sudo apt-get update`
    
    Updating the local cache with the latest info about the remote repositories.
    
2. `sudo apt-get upgrade`
    
    Upgrade any component that do not require you to remove any other packages.
    
3. `sudo apt-get dist-upgrade`
    
    Upgrade components when you are OK with removing/swapping other packages.
    
4. `apt-cache search _package_`
    
    Search for _package_ in the repositories. You will have to run `sudo apt-get update` before doing this.

5. `sudo apt-get install _package_`

6. `sudo apt-get install _package1_ _package2_`

7. `sudo apt-get install _package=version_`
    
    Install a specific version of the _package_.
    
8. `sudo dpkg-reconfigure _package_`
    
    Reconfigure _package_. Most of the packages, after you install them, they go through a configuration script that configures your system to use that particular package. Sometimes this include user prompts as well. If you want to do this configuration yourself after installation or rerun the configuration, use this command.

9. `apt-get install -s _package_`
    
    Dry-run installation. Using this command you can see all the changes that installing _package_ might do to your system without actually doing those changes. One important note is that this command does not require `sudo` and hence can be used in systems where you do not have root privileges.

10. `sudo dpkg --install _debfile.deb_`
    
    Install directly from the _debfile.deb_ file. Note that this DOES NOT resolve dependencies.
    
11. `sudo apt-get install -f`
    
    This command is used to fix dependency issues. While installing a package, if there are unmet dependencies, the installation will fail. If you've noticed, `apt-*` commands resolve these dependencies automatically. Whereas using `dpkg` to install `.deb` files directly does not resolve dependencies. Hence, run this command in those scenarios.

13. `sudo apt-get remove _package_`
    
    Remove the _package_ without removing the configuration files. If you accidentally remove a package or install the same package after some time, you will need to save the configuration you did for your first installation.

14. `sudo apt-get purge _package_`
    
    Remove the _package_ and all it's configuration files.

15. `sudo apt-get autoremove`
    
    Remove any packages that were installed as dependencies and are no longer required. Use `--purge` to remove the configuration files of these packages.

16. `sudo apt-get autoclean`
    
    Remove out-of-date packages in your system that are no longer maintained in the remote repository.

17. `apt-cache show _package_`
    
    Show information about _package_.
    
18. `dpkg --info _debfile.deb_`
    
    Show information about the _debfile.deb_ file.
    
19. `apt-cache depends _package_`
    
    Show dependencies of the _package_.
    
20. `apt-cache rdepends _package_`
    
    Show packages that are dependent on this _package_ (reverse dependency)

21. `apt-cache policy _package_`
    
    If there are multiple versions of _package_ installed, this command shows all the versions and the version being used as default.

22. `dpkg -l`
    
    Shows a list of every installed/partially-installed packages in the system. Each package is preceded by a series of characters.
    
    First character:
        
        - _u_: unknown
        - _i_: installed
        - _r_: removed
        - _p_: purged
        - _h_: version is held
    
    Second character:
    
        - _n_: not installed
        - _i_: installed
        - _c_: configuration files are present, but the application is uninstalled
        - _u_: unpacked. The files are unpacked, but not configured yet
        - _f_: package is half installed, meaning that there was a failure part way through an installation that halted the operation
        - _w_: package is waiting for a trigger from a separate package
        - _p_: package has been triggered by another package
    
    Third character:
        
        - _r_: re-installation is required. Package is broken.

23. `dpkg -l _regex_`
    
    Search for all packages matching the _regex_.

24. `dpkg -L _package_`
    
    List all files controlled by this _package_.

25. `dpkg -S _/path/to/file_`
    
    This is the inverse of `-L`, which gives a list of all files controlled by a package. `-S` prints the package that is controlling this file.

26. `dpkg --get-selections > ~/packagelist.txt`
    
    Export the list of installed packages to `~/packagelist.txt` file which can be used to restore in a new system. This is analogous to `requirements.txt` file in python.

27. `sudo add-apt-repository _ppa:owner_name/ppa_name_`
    
    This is used to add a PPA (Personal Package Archives). When you cannot find the required package in the default set of repositories and is available in another repository, we can add that PPA and install packages from there. This is specific to Ubuntu systems as of now. Before installing packages from the newly added repository, update the local cache with `sudo apt-get update`. Usually PPAs have a smaller scope than a repository.

28. **Adding a new repository:** 
    - Edit the `/etc/apt/sources.list` file
    - Create a new `/etc/apt/sources.list.d/new_repo.list` file (Must end with `.list`)
    
        Inside the file, add the new repository with the following format.
        `deb_or_deb-src url_of_repo release_code_name_or_suite component_names`
    
    - _deb_ or _deb-src_: This identifies the type of repository. It can be either `deb` (for conventional repositories) or `deb-src` (for source repositories)
    - _url_: URL of the repository.
    - _release_code_name_ or _suite_: Code name of your distribution's release
    - _component_names_: Labels for the source.

29. **Exporting a package list:**
    
    If you want to setup a new system with all the packages from the old system, you first have to export the package details from the old system and install it in the new system. 
    - `dpkg --get-selections > ~/packagelist.txt`: Export all package names
    - `mkdir ~/sources`
    - `cp -R /etc/apt/sources.list* ~/sources`: Backup your sources list.
    - `apt-key exportall > ~/trusted_keys.txt`: Backup your trusted keys

30. **Importing a package list:**
    - `sudo apt-key add ~/trusted_keys.txt`
    - `sudo cp -R ~sources/* /etc/apt/`
    - `sudo dpkg --clear-selections`: Clear all non essential packages from the system for a clean slate.
    - `sudo apt-get update`
    - `sudo apt-get install dselect`: Install dselect tool
    - `sudo dselect update`
    - `sudo dpkg --set-selections < packagelist.txt`: Import from the package list.
    - `sudo apt-get dselect-upgrade`
