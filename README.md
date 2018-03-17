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

Configure `~/.ssh/config` if you want your keys to be automatically added to the agent, e.g.
```
IdentityFile ~/.ssh/id_rsa
AddKeysToAgent yes
```
