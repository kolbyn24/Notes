# Setup 

Original Article:
https://www.trustedsec.com/blog/obsidian-taming-a-collective-consciousness/

To get started you will need to:
1. Setting up github
2. Install Obsidian
3. Install and Configure Obsidian-Git
4. Install and Configure Templater

## Setting up github

Following this guide [windows github install](https://github.com/gitobsidiantutorial/obsidian-git-tut-windows/blob/main/README.md)


## Install Obsidian

Download and Install [Obsidian](https://obsidian.md/)

## Plugins 

Community plugins can be found in the Obsidian Settings menu located in the lower-left corner of the Obsidian window (look for the gear icon). From there, select the `Community Plugins` tab on the left side of the screen. Disable `Safe Mode`, and then select the Browse button.

### Obsidian Git 

Search for the `Obsidian Git` community plugin and enable it from the `Community Plugins` panel in the settings menu. Configure it (left panel under `Plugin Options -> Obsidian Git`) using the following settings (if you do not see these settings try closing and reopening obsidian, also make sure .git is in the root of your vault directory. Make sure the root folder is named "Notes"):

```
Vault Backup Interval: 60 
Auto Pull Interval: 10 
Commit Message: MAC-0000XXX {{date}} 
Date Placeholder: YYYY-MM-DD HH:mm:ss 
Pull Changes Before Push: Enabled (default)  
```

### Obsidian Git auth with SSH on Mac
On my mac, I had to run this command in the root of the Notes directory (I am not sure if this is needed now, try everything else first and let me know if it is or not):

```
git remote set-url origin git@github.com:kolbyn24/Notes.git
```

Generate ssh keys with: 
```
ssh-keygen -b 4096 -t rsa
```

and add the public key to `https://github.com/kolbyn24/Notes/settings/keys/new` (make sure to allow write access)

Finally, add github to your known_host file.

```
github.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOMqqnkVzrm0SdG6UOoqKLsabgH5C9okWi0dh2l9GKJl
github.com ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEmKSENjQEezOmxkZMy7opKgwFB9nkt5YRrYMjNuG5N87uRgg6CLrbo5wAdT/y6v0mKV0U2w0WZ2YB/++Tpockg=
github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==
```

Now try command+p, search for git and click Obsidian git: create backup. Make sure there are no errors.

### Templater 

Search for the `Templater` community plugin by SilentVoid and enable it from the `Community Plugins` panel in the settings menu. Configure it (left panel under `Plugin Options -> Templater`) using the following settings:

```
Template folder location: 04 - Templates 
Trigger Templater on new file creation: Enabled
Add New Folder Template: / - Templates/0400 - Gen_Note.md
```

Make sure to test if it's working before moving on. You should get a drop down after hitting "new note" if you get an error or no drop down then something is wrong. New notes should appear under the 03 section, not at the bottom. 

Important note! The current version of templater is broken as of 12/06/2022. I had to manually downgrade to version 1.14.3 by going to the github page and replacing the .js files.

The issue can be fixed without having to downgrade templater. Line 143 in gen_note.md needs to be changed: https://github.com/trustedsec/Obsidian-Vault-Structure/pull/7/files



And that's it! You should be ready to go.

Can use ctrl+p or command+p and type git to manually push changes using "create backup" or "pull" to get the most recent changes. It should be automatically pushing and pulling, but I like to manually push my changes to avoid conflicts.
 
Use LanguageTool community plugin for spell check or turn on system spellcheck under settings, editor, spellcheck.

I suggest reading through [[01 - Operation]] to get started.

### Troubleshooting

[multiple users](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/multiple-users.md)
If obsidian is asking for a user account every time you push or pull, try setting a default:

```
#Get user list
git credential-manager github list

#Set Default User
git config --global credential.<URL>.username <USERNAME>
git config --global credential.https://github.com.username alice
```
