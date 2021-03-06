
## Tips and Cheats

Many APIs are documented here: [http://devdocs.io/](http://devdocs.io/)

### Miscellaneous Mac Tips

*Finder*
To show hidden files and directories by:

```
 defaults write com.apple.Finder AppleShowAllFiles YES
```

Then use option/right click hold, and choose option to 'relaunch'
To return to 'normal', reverse but switch YES to NO.

Git must have proxy set in .bash_profile to get through firewall:

```
export http_proxy=www-proxy.xyz.com:8080
```

#### bash command line basics:

References:

* [Sed - An intro and tutorial](http://www.grymoire.com/Unix/Sed.html)

To create a gzip tar:

```
tar -czf eventmgmt.tar.gz eventmgmt/
```

To extract from a gzip tar:

```
tar -xzf eventmgmt.tar.gz [optional_file_to_extract]
```

To kill a process using it's pid file:

```
kill -9 `cat some_process.pid` (note back ticks are used)
```

To list processes by name (e.g. node):

```
ps -ef | grep -i node
```
```
pgrep -lf node
```

and to KILL the processes found with pgrep, use:

```
pkill -f node
```

To find a process using a port (e.g., 8080)

```
lsof -i tcp:8080
```

To find processes that are listening:

```
lsof -i -P | grep LISTEN
```

To find a file:

```
sudo find / -name "filename"
```

To find the size of a directory:

```
du -h --max-depth=1 some_directory/
```

To find number of files within a directory:

```
ls some_directory/ | wc -l
```

To execute a command in each directory…  here doing so for immediate directories only...

```
find . -maxdepth 1 -type d -exec sh -c '(cd {} && COMMAND)' ';'
```

#### Tmux

References:
* [The Text Triumvirate](http://www.drbunsen.org/the-text-triumvirate/)
* [Tweaking tmux and vim](https://wincent.com/blog/tweaking-command-t-and-vim-for-use-in-the-terminal-and-tmux)
* [tmux shortcuts and cheatsheet](https://gist.github.com/MohamedAlaa/2961058)

CTRL-a (default is CTRL-b) then:
* d  = detach (exit back to normal terminal) and use normal vim keys
* s  = create a new pane below the current one, resize with C-a Shift-J/K
* \[ = this starts 'copy mode' which is scrollable (type q to exit copy mode)
(or use trackpad to navigate and resize panes)

#### MacVim / Vim

References:
* [Janus](https://github.com/carlhuda/janus)
* See tmux references above

Install Solarized for both vim and iTerm2

*Unite*

<space>f list files with fuzzy search
    (NOTE: if list is missing files, CRTL-l (lower case L))
    i  - toggles view style
    gh - toggles showing hidden files
<space>b list buffers
<space>g grep
<space>t tags

*EasyMotion*
<Leader><Leader>fA = highlight 'finds' of 'A'

*ZoomWin*
Ctrl-w o = zoom/unzoom a pane when using split panes

*TagBar*
<Leader>rt = toggle tagbar (CTAG tags of current file)

<Leader>/ = toggle commenting current line / select lines

*Ack*
<Leader>f = bring up :Ack

*Grails functions*
Ctrl-i = goto Groovy class under cursor

<Leader>h = Show all subclasses of class name under cursor as a quickfix list

*Syntastic*
The JavaScript checker is configured to be 'eslint' (which must be installed via npm) 

Errors are shown in the gutter, and can also be viewed in the 'locations list'
(akin to the 'quick list', albeit slightly different).

To open the locations list, use:  :Errors
To close the locations list:      :lclose

*Commentary*
To comment out the current line:  gcc
To comment out a selection:       gc

References:
* [Using Marks](http://vim.wikia.com/wiki/Using_marks)

Commands:
To remove highlights from current/last search:
```
:noh
```

To replace a word:
```
%s/word/newword/g
```

To replace a case-sensitive word:
```
:%s/word\C/newword/g
```

To replace tabs with spaces:
```
:retab
```

To select columns:
ctrl-V goes into visual mode, selecting columns - use esc esc to fill other columns

To remove Windows line returns that show as ^M, use:
```
:%s/^V^M//g
```

(type this as :%s/CTRL-V-CTRL-M//g  (hold CTRL and type VM)k
(note that it will appear as :%s/^M//g when displayed)

To tag a project in order to use exuberant tags plugin:
```
   ctags .
```

To delete a file, use:
```
:Explore -- and then use 'R' to remove the highlighted file
```

#### Git

To see the history of a file:

```
git --no-pager log --pretty=oneline --name-only --abbrev-commit --follow -p some_file
```

To list contributors (and show number of files):

```
git ls-files -z | xargs -0n1 git blame -w | perl -n -e '/^.*?\((.*?)\s[\d]{4}/; print $1,"\n"' | sort -f | uniq -c | sort -n
```

To show specific contibution statistics for a contributor:

```
git log --shortstat --author "chasdev" | grep "files changed" | awk '{files+=$1; inserted+=$4; deleted+=$6} END {print "files changed", files, "lines inserted:", inserted, "lines deleted:", deleted}'
```

*tags*

Delete a remote tag named '12345':

```
git tag -d 12345
git push origin :refs/tags/12345
```

Push a tag:

```
git push origin my-tag
```

To ensure the remote tags are fetched:

```
git fetch -t
```

##Using mitmproxy to intercept/modify HTTP traffic

[mitmproxy](http://mitmproxy.org)
[tutorial (intercept https traffic of phone)](http://blog.philippheckel.com/2013/07/01/how-to-use-mitmproxy-to-read-and-modify-https-traffic-of-your-phone/)

To intercept HTTP requests, configure the browser to use localhost:8888 as a proxy.  (For chrome, install ‘SwitchySharp’ extension to easily switch proxies.)
Then run:

```
mitmproxy -p 8888
```

#### Brew

If 'brew update' fails with conflicts, cd /usr/local which is a git directory, and stash or commit on a different branch, then re-try.

#### Couchdb

```
brew update
brew install couchdb
```

```
To test CouchDB run:
    curl http://127.0.0.1:5984/

The reply should look like:
    {"couchdb":"Welcome","uuid":"....","version":"1.5.0","vendor":{"version":"1.5.0-1","name":"Homebrew"}}

To have launchd start couchdb at login:
    ln -sfv /usr/local/opt/couchdb/*.plist ~/Library/LaunchAgents
Then to load couchdb now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.couchdb.plist
Or, if you don't want/need launchctl, you can just run:
    couchdb
```

#### Redis

```
brew update
brew install redis
```

To have launchd start redis at login:

```
    ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
```

Then to load redis now:

```
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
```

Or, if you don't want/need launchctl, you can just run:

```
    redis-server /usr/local/etc/redis.conf
```

#### MongoDB

Mongo Database

```
brew update
brew install mongodb
```

To have launchd start mongodb at login:

```
    ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
```

Then to load mongodb now:

```
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
```

Or, if you don't want/need launchctl, you can just run:

```
    mongod --config /usr/local/etc/mongod.conf
```

