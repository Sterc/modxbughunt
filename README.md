# Contributors guide MODX Bug Hunt
More information about the MODX Bug Hunt can be found on the [modxbughunt.com website](http://www.modxbughunt.com).

## 1. Tools
- A command line interface (Terminal/iTerm/etc)
- Git on your system
- PHP Storm or other client. Please just make sure you use 4 spaces, instead of tabs.
- Some sort of webserver. Apache or NGINX preferred. We use Vagrant boxes, but you might also use [MAMP](https://www.mamp.info/en/)/[XAMPP](https://www.apachefriends.org/index.html).

## 2. Github
- You'll need a Github account which can be made on [Github.com](https://github.com/)
- Then you need a fork of MODX Revolution. Go [here](https://github.com/modxcms/revolution) and click 'Fork' on the top-right.

## 3. Signed MODX Contributors License Agreement
You formally need to sign a Contributors License Agreement before contributing. But, since a lot of people are attending the MODX Bug Hunt, this might take a while before it is approved. Therefore, just get on with coding and testing! [CLA docs on modx.com](https://docs.modx.com/community/contribute/becoming-a-contributor).

## 4. Setting up MODX files from a Git repository
First, clone your Fork on your local machine, into the directory which will be your webroot.

**Please note:** in the examples below, you'll notice SSH-url's (git@github.com:modxcms/revolution.git). Github also offers HTTPS-links, which are easier to use if you're a newbie (https://github.com/modxcms/revolution.git). You can simply replace them in the examples.

```
$ git clone git@github.com:modxcms/revolution.git
$ cd revolution
```

Next, add the original modxcms/revolution reposition as your upstream. We'll discuss the use of this later. Right now, just do it.

```
$ git remote add upstream git@github.com:modxcms/revolution.git
```

Now we need to checkout (read: download) the current development-branch, which is ```2.5.x``` at the time of writing.

```
$ git checkout 2.5.x
```

Make sure your repository is still 'clean'. Make sure you haven't made any changes.

```
$ git status
On branch 2.5.x
Your branch is up-to-date with 'origin/2.5.x'.
nothing to commit, working tree clean
```

If you see the above message, you're totally fine. If you do see changes, you've done something horribly wrong! Make sure it is clean and you've made no changes to files yet. Otherwise your commits will mess up the MODX repository later on.

Next we'll have to do something weird. The git-version of MODX doesn't contain a pre-built core, like the regular MODX download does. We need to build this manually.

Cd into the _build-folder and make sure you're there ;-) You can do this by using the 'pwd' command. It will show the current path.

```
$ cd _build
$ pwd
/your-absolute-path-here/revolution/_build
```

Next, we need to copy (DO NOT RENAME THEM) the following 2 files:
```
$ cp build.config.sample.php build.config.php
$ cp build.properties.sample.php build.properties.php
```

Next, change the build.config.php file to your needs.
```
<?php
/* define the MODX path constants necessary for core installation */
define('MODX_CORE_PATH', dirname(dirname(__FILE__)) . '/core/');
define('MODX_CONFIG_KEY', 'config');

/* define the connection variables */
define('XPDO_DSN', 'mysql:host=localhost;dbname=YOUR_DATABASE_NAME_HERE;charset=utf8');
define('XPDO_DB_USER', 'YOUR_USERNAME_HERE');
define('XPDO_DB_PASS', 'YOUR_PASSWORD_HERE');
define('XPDO_TABLE_PREFIX', 'modx_');
```

The next step requires you to have PHP in your path. Check if you have PHP in your path by doing the following:
```
$ php -v
PHP 7.0.15 (cli) (built: Jan 22 2017 08:51:45) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2017 Zend Technologies
```

**If you do not get something like the above, please ask someone or Google on how to get it installed.**

To build the MODX-core, do the following from within the _build-folder:
```
$ php transport.core.php
[2017-02-24 11:52:17] (INFO @ transport.core.php) Beginning build script processes...
[2017-02-24 11:52:17] (INFO @ transport.core.php) Removed pre-existing core/ and core.transport.zip.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Core transport package created.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Core Namespace packaged.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Default workspace packaged.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged modx.com transport provider.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 2 modMenus.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged all default modContentTypes.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged all default modClassMap objects.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 181 default events.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 225 default system settings.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 2 default context settings.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 1 default user groups.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 1 default dashboards.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 1 default media sources.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 5 default dashboard widgets.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 2 default roles Member and SuperUser.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 6 default Access Policy Template Groups.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 7 default Access Policy Templates.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in 12 default Access Policies.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in web context.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in mgr context.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Packaged in connectors.
[2017-02-24 11:52:17] (INFO @ transport.core.php) Beginning to zip up transport package...
[2017-02-24 11:52:18] (INFO @ transport.core.php) Transport zip created. Build script finished.

Execution time: 1.5657 s
```

If the above shows you some random timezone-issues, it doesn't matter. The important thing is: you need the message "Transport zip created. Build script finished.". If it doesn't, ask around.

## 5. Performing the MODX setup
Now we've got all files in place, we just need to run the MODX Setup. Make sure you have a database ready and use the character set 'utf8_unicode_ci'.

Run your setup and be sure that you **DO NOT remove the setup-folder**. Just leave it there and ignore the annoying red suggestion to remove it. If you do remove it, you'll mess up the MODX repository in your next commit.

After installing, change the following System Settings:
```
friendly_urls     => Yes
use_alias_path    => Yes
publish_default   => Yes
cache_disabled    => Yes
```

Optional (but we prefer no .html-extensions). Change the Content-Type of HTML. Remove the .html File extension. You can do this by clicking "Content" in the top MODX menu and then clicking 'Content Types'.

Clear your cache.

**Also, setup some random content, so you can do some serious testing.***

## 6. Workflows
We've got 2 workflows worked out for you: bug fixing (doing development yourself) and testing (if you're not that much into developing, but do want to help).

## 6A. Bug fixing workflow
1. Pick an issue from the [MODX issue tracker](https://github.com/modxcms/revolution/issues)
2. Prevent duplicate work by claiming it by commenting on it. Something along the lines of: "I'm going to try and fix this today."
3. Next, create a branch from your current development branch (2.5.x), to start working in your own environment.

If the issue you want to fix is a feature, name it feature-ISSUENUMBER. If it is a bug, name it bug-ISSUENUMBER. In this example we'll fix a broken link in the docs. The issue can be [found here](https://github.com/modxcms/revolution/issues/13309). It has issue number 13309.

```
$ git checkout -b bug-13309
```

Next, we'll fix our issue and change some code. If you're confident about your changes, we want to commit it back to Github. We changed the file ```core/lexicon/en/about.inc.php```

Before doing this, we need to check if git is only trying to commit the files you had in mind. Sometimes, another file you don't know of is added to your repo.

```
$ git status
On branch bug-13309
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   core/lexicon/en/about.inc.php

no changes added to commit (use "git add" and/or "git commit -a")
```

If the files you want added to MODX are also in the status-report above, you did well. You're all set! We need to add it to our commit and push it to our online fork on Github. Use the hashtag to reference the issue and/or to tag it for stuff like the MODX Bug Hunt.

```
$ git add .
$ git commit -m "Fixed issue #13309 #modxbughunt"
$ git push origin
git push --set-upstream origin bug-13309
Counting objects: 4, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1.69 KiB | 0 bytes/s, done.
Total 4 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local objects.
To github.com:gpsietzema/revolution.git
 * [new branch]      bug-13309 -> bug-13309
Branch bug-13309 set up to track remote branch bug-13309 from origin.
```

That's it for the command line! Now we need to head over to Github to make a Pull Request (PR).

7. Go to your fork on Github. You'll probably see a message resembling something like ```Your recently pushed branches: bug-13309 (2 minutes ago)```. Click the "compare & pull request" button.

Now you see two repositories being compared. On the left, there is ```modxcms/revolution```, set it to ```base: 2.5.x```. On the right you'll see ```yourname/revolution``` with ```bug-13309```.

If everything is fine, you'll notice a message stating the following: ```Able to merge. These branches can be automatically merged.```.

Please make sure you enter some explanation about your commit for our beloved integrators (the people who will actually merge your code into MODX). This will make it easier for them to test it.

Once you've did this, click the magic button **Create pull request**

Congratulations, you did it!
8. On to the next one! If you want to fix another bug, we first need to be on the ```2.5.x``` branch again. To do this, we first want to make sure that our Fork's ```2.5.x``` branch is in sync with the original ```modxcms/2.5.x``` branch. Do the following to accomplish this:
```
$ git fetch upstream 2.5.x
$ git fetch origin 2.5.x
$ git checkout 2.5.x
$ git pull upstream 2.5.x
```

If in the last step, you get a text editor with a merge message. Just save and quit the editor and you are all fine. If this editor is VI, just hit Escape to exit type-mode, then type ```:wq``` and hit enter.

You now have updated your Fork. Next you can go back to step 1 in 6a. Rinse and repeat!

## 5B. Test workflow
1. Pick a pull request from the current [PR-list on Github](https://github.com/modxcms/revolution/pulls).
2. Read the PR and check if this is something you might be able to test. Check if the issue is still existent and you can reproduce in the current development branch:
```
$ git checkout 2.5.x
```

If you can reproduce it, comment in the PR that you are going to test it. If you can't reproduce it, mention that as-well and mention the user who made the PR.
3. To pull this PR, you need to add the fork of the PR-owner to your remotes. In this example I'm using a random PR. In this case, one by goldsky. In hte example below, you'll see the 'git remote add goldsky' part. 'goldsky' is the name of remote. This can be anything, but we recommend to use the Git-username to make it easy to remember. The 'goldsky:patch-ellipsis' part is the Github-URL of Goldsky's modxcms-fork.

After adding the remote, fetch it and checkout the PR-branch. In this case ```patch-ellipsis```.

```
$ git remote add goldsky git@github.com:goldsky/revolution.git
$ git fetch goldsky
$ git checkout patch-ellipsis
Branch patch-ellipsis set up to track remote branch patch-ellipsis from goldsky.
Switched to a new branch 'patch-ellipsis'
```

4. Clear both your MODX and browser cache
5. Test whether the bug is really fixed or not
6. Is it fixed? Let the integrators and fixer know by mentioning them in your comment. Not fixed? Let the fixer know by mentioning him in a comment.

## Problems, issues, help needed?
Just ask in the [MODX Community Slack #development](https://modx.org/) or on the [MODX Community Forums](https://forums.modx.com/).
