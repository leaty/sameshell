# SAMESHELL
Use your personal shell config across all chained ssh/su sessions.

This isn't exactly a program, but rather an idea with example shell configurations. Currently, there are only examples for bash, but any shell would work just fine- just change everything "bash" to your shell. Feel free to add examples for other shells with a PR.

I imagine it could be rewritten quite nicely too, e.g. to detect remote shells available.

## Why?
Have you ever wanted your aliases, PS1 setup and environment configurations across all ssh/su sessions?  Well, look no further. Of course, you could- if it's not too much of a bother for you, manually configure your personal configuration on all the servers/logins you access. However, if you share the same logins with others, that might not be what the *others* want.

SAMESHELL achieves this without actually changing anything, it's all temporary in your session by using process substitution. Granted, it's not a complete "environment" copy, as there may be programs missing on a remote server.

However, by adding package manager install commands in your personal configuration, I guess you.. technically could do that. In such cases though, I would recommend first checking if they're installed, to avoid unnecessary load times when the package manager fetches repository metadata before looking up said programs.

## How it works
It works by sending a command to start the shell with a specific configuration "file", but the *file* is generated with process substitution instead i.e. `<()`. Because of this, SAMESHELL is able to essentially build a shell configuration consisting of the original remote/login configuration (skippable) together with your personal configuration.

### Chained sessions
As an extra, within said built shell configuration, it sets a temporary environment variable containing your personal configuration. This adds support for chained ssh/su sessions, meaning you can retain your personal configuration while doing ssh, then su, then ssh ... To infinity and beyond!

Examples of chained sessions can be found in the sample configuration.

## Footprint
Needless to say, the configuration for this is tiny. However, it grows with your personal configuration. In an attempt to both minify your configuration and make any configuration compatible, lines starting with `#` are removed (comments), after which it's encoded with base64.

If your configuration is large enough to make your shell open slowly (that would have to be a true mammoth of a config). It could be moved to read your configuration from within the SAMESHELL commands, which would slow the commands down instead- but at least your terminal startup is sprightly again.

## Gotchas

### Command names
Consider changing the command names for any of the reasons below. I personally have `zh`, `zo` and `zu` respectively.

#### `ssh`
* Now requires `;` at the end of any direct command you wish to run
* Stays logged after your command

#### `sudo`
* Now requires `su` i.e. `sudo su myuser`

#### `su`
* Inability to run a direct command, because `su` can only take one `-c/--session-command` at once
