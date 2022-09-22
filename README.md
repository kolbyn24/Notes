# Set up 

To get started you will need to:
1. [Setting up github](#Setting up github)
1. [Install Obsidian](#install-obsidian)
1. [Install and Configure Obsidian-Git](#obsidian-git)
1. [Install and Configure Templater](#templater)

## Setting up github

Following this guide [windows github install](https://github.com/gitobsidiantutorial/obsidian-git-tut-windows/blob/main/README.md)


## Install Obsidian

Download and Install [Obsidian](https://obsidian.md/)

## Plugins 

Community plugins can be found in the Obsidian Settings menu located in the lower-left corner of the Obsidian window (look for the gear icon). From there, select the `Community Plugins` tab on the left side of the screen. Disable `Safe Mode`, and then select the Browse button.

### Obsidian Git 

Search for the `Obsidian Git` community plugin and enable it from the `Community Plugins` panel in the settings menu. Configure it (left panel under `Plugin Options -> Obsidian Git`) using the following settings (if you do not see these settings try closing and reopening obsidian, also make sure .git is in the root of your vault directory):

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

Can use ctrl+p or command+p and type git to manually push changes