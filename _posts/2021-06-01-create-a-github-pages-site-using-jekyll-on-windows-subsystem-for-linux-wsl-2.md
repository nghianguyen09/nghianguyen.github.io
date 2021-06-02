---
layout: post
title:  Create a GitHub Pages site using Jekyll on Windows Subsystem for Linux (WSL 2)
author: Nghia Nguyen
tags:   [WSL 2, Windows Subsystem for Linux, Jekyll, GitHub Pages]
comments_id: 1
---
With the Windows Subsystem for Linux (WSL) we are now able to run a Linux environment directly on Windows 10. I was able to successfully create a GitHub Pages site with Jekyll on WSL 2.

The following are steps I have followed and done.

## Step 1: Install WSL 2 (Windows Subsystem for Linux) on Windows 10
I installed WSL 2 on Windows 10 following 6 manual installation steps from [Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10#manual-installation-steps)

At the final step (step 6) in the installation guide, I selected [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)

I did not install Windows Terminal. You can do that if you want.

## Step 2: Install Jekyll on Unbuntu
Launch Ubuntu terminal, and follow installation steps from [Install Jekyll on Ubuntu](https://jekyllrb.com/docs/installation/ubuntu)

Now we need to confirm that Jekyll and prerequisites are installed on Ubuntu by outputting the version of `Jekyll` and `Gem` on Ubuntu terminal. To do this, run the commands on Ubuntu terminal:

```bash
$ jekyll -v
```
and 

```bash
$ gem -v
```

You should see something similar to mine
![](/asset/images/2021-06-01/check-jekyll-and-gem.png)

## Step 3: Creating a GitHub Pages site with Jekyll

1. Open Ubuntu terminal

2. Navigate to the location where you want to store your site's source files.

    To access to the Windows file system from Ubuntu terminal, you need to use `/mnt/<drive letter>/`. Example usage would be `cd /mnt/c` to access the `c:\` drive. 
    
    I store my site files on `d:\sites`, so I navigate to that folder (remember to change the path if yours is different than mine). Run the command:

    ```bash
    $ cd /mnt/d/sites
    ```
3. To create a new Jekyll site under the folder `jekyll-new-site-using-wsl2`, use the command `jekyll new <path>`:

    ```bash
    $ jekyll new jekyll-new-site-using-wsl2
    ```

    You should see a message about new site created
    ![](/asset/images/2021-06-01/new-jekyll-site-created.png)

4. Open the `Gemfile` that Jekyll created under the folder `jekyll-new-site-using-wsl2` with Vim on Ubuntu terminal or any your favorite text/code editor on Windows such as Notepad++ or Visual Code

    Content of the Gemfile before editing

    ```ruby
    source "https://rubygems.org"
    # Hello! This is where you manage which Jekyll version is used to run.
    # When you want to use a different version, change it below, save the
    # file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
    #
    #     bundle exec jekyll serve
    #
    # This will help ensure the proper Jekyll version is running.
    # Happy Jekylling!
    gem "jekyll", "~> 4.2.0"
    # This is the default theme for new Jekyll sites. You may change this to anything you like.
    gem "minima", "~> 2.5"
    # If you want to use GitHub Pages, remove the "gem "jekyll"" above and
    # uncomment the line below. To upgrade, run `bundle update github-pages`.
    # gem "github-pages", group: :jekyll_plugins
    # If you have any plugins, put them here!
    group :jekyll_plugins do
    gem "jekyll-feed", "~> 0.12"
    end

    # Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
    # and associated library.
    platforms :mingw, :x64_mingw, :mswin, :jruby do
    gem "tzinfo", "~> 1.2"
    gem "tzinfo-data"
    end

    # Performance-booster for watching directories on Windows
    gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
    ```

    5. Add `"#"` to the beginning of the line that starts with `gem "jekyll"` to comment out this line.

    6. Add the `github-pages gem` by removing `#` at the begining the line starting with `# gem "github-pages"` to uncommnent this line.

    Content of the Gemfile after editing in 5 & 6
    ```ruby
    source "https://rubygems.org"
    # Hello! This is where you manage which Jekyll version is used to run.
    # When you want to use a different version, change it below, save the
    # file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
    #
    #     bundle exec jekyll serve
    #
    # This will help ensure the proper Jekyll version is running.
    # Happy Jekylling!
    # gem "jekyll", "~> 4.2.0"
    # This is the default theme for new Jekyll sites. You may change this to anything you like.
    gem "minima", "~> 2.5"
    # If you want to use GitHub Pages, remove the "gem "jekyll"" above and
    # uncomment the line below. To upgrade, run `bundle update github-pages`.
    gem "github-pages", group: :jekyll_plugins
    # If you have any plugins, put them here!
    group :jekyll_plugins do
    gem "jekyll-feed", "~> 0.12"
    end

    # Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
    # and associated library.
    platforms :mingw, :x64_mingw, :mswin, :jruby do
    gem "tzinfo", "~> 1.2"
    gem "tzinfo-data"
    end

    # Performance-booster for watching directories on Windows
    gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
    ```

    7. Save and close the Gemfile.

    8. From the Ubuntu terminal, make sure your current folder is `/mnt/d/sites/jekyll-new-site-using-wsl2` (if not, run command `cd /mnt/d/sites/jekyll-new-site-using-wsl2` to navigate to that folder), run the command

    ```bash
    $ bundle update
    ```

## Step 4: Testing your GitHub Pages site locally with Jekyll

1. Continue on Ubuntu terminal from Step 3.8, your current working folder should be `/mnt/d/sites/jekyll-new-site-using-wsl2` (if not, run command `cd /mnt/d/sites/jekyll-new-site-using-wsl2` to navigate to that folder)

2. Run command

    ```bash
    $ bundle install
    ```

3. Run your Jekyll site locally, using command

    ```bash
    $ bundle exec jekyll serve
    ```

    You should see output similar to below screenshot
    ![](/asset/images/2021-06-01/jekyll-local-site-started.png)

4. Open your web browser to http://127.0.0.1:4000

    ![](/asset/images/2021-06-01/browse-local-site.png)


Awesome! You run Ubuntu natively on Windows.

Happy using Windows Subsystem for Linux!