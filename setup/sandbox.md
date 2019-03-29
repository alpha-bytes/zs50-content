## ZS50 Sandbox
ZS50 sandboxes sit atop and expand the capabilities of the web-based sandboxes provided by CS50. Sandboxes are ephemeral environments created from Docker images. If you're interested in learning more about how CS50 provides these environments you can find out more at https://cs50.readthedocs.io/sandbox/. 

### Setup
To get started with a ZS50 sandbox you just need to: 
1. Sign up for a Github account
2. Click [here](https://sandbox.cs50.io/?name=ZS50_0.2&script=mkdir%20classes%20%26%26%20npm%20i%20git%2Bhttps%3A%2F%2Fgithub.com%2Falpha-bytes%2Fzs50.git%20%26%26%20npm%20link%20.%2Fnode_modules%2Fzs50&window=editor&window=terminal) to spin up a ZS50 sandbox. 

Upon clicking the link, you will be asked to log into your Github account for authorization (if you're not logged in already). From there, you'll be directed to your ready-made ZS-ified dev environment. 

### Important Points
1. You should only use the link above to create your sandbox *one time*. After that, you can access the same sandbox from the `Recent sandboxes` tab of https://sandbox.cs50.io. Each time you use the link above you spin up a *new* ZS50 sandbox, meaning any files or configuration from your prior sandbox is not persisted. 
2. Sandbox environments are ephemeral and **may expire at any point** (hey, that's the tradeoff of a free service, no?). This means that, to make sure you don't lose any Apex classes you've worked on and would like to save, you should download them to your local machine on a regular basis. 
3. Upon spinning up your sandbox the `zs50` command line utility should be automatically linked. To check, type the command `zs50 auth` into the command line. You should see the following: 
```
No authorized dev org linked for ZS50. Authorize now? (Y/N)
```

If instead you see...
```
bash: zs50: command not found
```

...have no fear. Just type the following command into your terminal and hit return: 
```
npm link ./node_modules/zs50
```

Try that `zs50 auth` command again, and this time it should work. If not, call in the helpdesk: 
![sadPanda](https://media.giphy.com/media/zrdUjl6N99nLq/giphy.gif)

If your `zs50` command is linked then you're good to go. Jump to the [zs50 cli github repo](https://github.com/alpha-bytes/zs50) for the full documentation on this tool. 