# SSH-Agent-WSL

The ssh-agent is not launched automatically in the WSL.  
Add these lines to `~/.bashrc` to fix that.

```
# wsfl bash is not a login shell
if [ -d "$HOME/bin" ] ; then
    export PATH="$HOME/bin:$PATH"
fi

# ssh-agent configuration
if [ -z "$(pgrep ssh-agent)" ]; then
    rm -rf /tmp/ssh-*
    eval $(ssh-agent -s) > /dev/null
else
    export SSH_AGENT_PID=$(pgrep ssh-agent)
    export SSH_AUTH_SOCK=$(find /tmp/ssh-* -name "agent.*")
fi
```

Ssh needs still to be configured if you want your keys to be automatically added.  
To do so add the following line to `~/.ssh/config`
```
AddKeysToAgent yes
```
If you are creating a new config file make sure to set the right permissions, i.e. `chmod 600 ~/.ssh/config`
