# Set up 

To get started you will need to:
1. [Generate a Deployment Key](#generate-a-deployment-key)
1. [Configure Git](#configure-git)
1. [Install Obsidian](#install-obsidian)
1. [Install and Configure Obsidian-Git](#obsidian-git)
1. [Install and Configure Templater](#templater)

## Generate a Deployment Key 

Generate an SSH key (with no password) to be used solely for this project (also known as a deployment key). For the comment, be sure to substitute flast for your first initial and last name.

```bash
# Generate an SSH key, replacing flast with your initial and last name
ssh-keygen -t ed25519 -C "MAC-0000XXX valkyrie deployment key" -f ~/.ssh/valkyrie-0000XXX -q -N ""

# Copy the public key to your clipboard
cat ~/.ssh/valkyrie-0000999.pub | pbcopy
```

Add this deployment key to the `valkyrie` GitLab project [here](https://github.com/fortalice/valkyrie/settings/keys). 

Use the format `MAC-0000XXX` for your key name, and don't forget to select the `Grant write permissions to this key` checkbox.

## Configure Git 

For this repository, we are going to make some changes to the .git/config file

```bash
# Clone and move into the repository  
git clone --config core.sshCommand="ssh -i ~/.ssh/valkyrie-0000XXX -F /dev/null" git@github.com:fortalice/valkyrie.git
cd valkyrie

# Set up a few git options for this repository only
git config pull.rebase true
git config fetch.prune true
git config core.sshCommand "ssh -i ~/.ssh/valkyrie-0000XXX -F /dev/null"

# Set up identity 
git config --global user.name "MAC-0000XXX"
git config --global user.email "MAC-0000XXXX@local"
```

## Install Obsidian

Download and Install [Obsidian](https://obsidian.md/)

## Plugins 

Community plugins can be found in the Obsidian Settings menu located in the lower-left corner of the Obsidian window (look for the gear icon). From there, select the `Community Plugins` tab on the left side of the screen. Disable `Safe Mode`, and then select the Browse button.

### Obsidian Git 

Search for the `Obsidian Git` community plugin and enable it from the `Community Plugins` panel in the settings menu. Configure it (left panel under `Plugin Options -> Obsidian Git`) using the following settings:

```
Vault Backup Interval: 60 
Auto Pull Interval: 10 
Commit Message: MAC-0000XXX {{date}} 
Date Placeholder: YYYY-MM-DD HH:mm:ss 
Pull Changes Before Push: Enabled (default)  
```

### Templater 

Search for the `Templater` community plugin by SilentVoid and enable it from the `Community Plugins` panel in the settings menu. Configure it (left panel under `Plugin Options -> Templater`) using the following settings:

```
Template folder location: 04 - Templates 
Trigger Templater on new file creation: Enabled
Add New Folder Template: / - Templates/0400 - Gen_Note.md
```

And that's it! You should be ready to go.
