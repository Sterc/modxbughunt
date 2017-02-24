# Contributors guide modxbughunt

## 1. Tools
- The command line
- Git
- PHP Storm or other client <- **IMPORTANT**: settings: spaces
- Apache or nginx on MAMP or Vagrant

## 2. Github
- You'll need a Github account which can be made on: <https://github.com/>
- Then you need a fork of MODX Revolution, which you can get here: <https://github.com/modxcms/revolution>

## ? 3. Signed MODX document ?
- <https://docs.modx.com/community/contribute/becoming-a-contributor>

## 4. Setting up MODX
- Local install from Git
  + Add upstream
  + Build ??
  + IMPORTANT: don’t remove setup after installing!
- A database using utf8 + utf8_unicode.ci
- Set up test content, URL's and settings
  + Friendly URL’s + paths
  + Disable cache 
  + Published-default
  + Content-types HTML
  + Cache clear

## 5A. Bug fixing workflow
1. Pick an issue
2. Prevent duplicate work by claiming it through commenting on it 
3. Tag or label it with #modxbughunt
4. Create a branche -> feature/bug
5. Fix bug 
6. ```git add + commit (tag #modxbughunt in message) + push origin```
7. Github PR to modxcms/revo
8. Git checkout/pull? origin develop

## 5B. Test workflow 
1. Pick a pull request
2. Check whether it’s really a bug or not, confirm or reject in comment, if confirmed, go to the next step
3. Claim it, let people know you’re testing this by commenting
4. Add the fork used by the concerned pull request as a remote for your repo using: git remote add forkname URL-of-the-fork
5. Then fetch using: git fetch forkname
6. Then integrate the concerned pull request using: git pull forkname branchename
7. Clear cache of both MODX and browser
8. Test whether the bug is really fixed or not
9. Is it fixed? Let us know by mentioning an integrator in your comment. Not fixed? Let the fixer know by mentioning his name in a comment.

## Troubleshooting Git issues
- Too many unnecessary files
- Don’t move folders in finder, it might break git-config
