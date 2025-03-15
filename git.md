# Various Git Knowledge

## Seperating identities
It's possible to use two seperate GitHub accounts (e.g. I use a seperate account for college), or any given set of identites.

### Create folders/files
Create the following folders (if they don't exist):
- `~/.config`
- `~/.config/git`

Create a folder for each Git identity you want to use. You will need to keep any Git repos inside the corresponding folder. I use the following:
- `~/code`
- `~/college`

Create a new file in `~/.config/git` for each Git identity you want to use. I use the following:
- `~/.config/git/personal`
- `~/.config/git/college`

### Setting up the new config files
For each of the new files you made, add the following (replacing placeholders):
```
[user]
    email = "email@example.com"
    name = "A cool name"
[credential]
    username = a-cool-username
```
> [!TIP]
> You can use the anonymous commit emails as seen below.


### Setting up `~/.gitconfig`
Remove the existing `[user]` settings from your config, if there are any.

#### Includes

For each idenity, add the following to your config, replacing `~/path/to/folder/` with your new folders and `identity_here` with the filename you chose earlier:
```
[includeIf "gitdir:~/path/to/folder/"]
    path = ~/.config/git/identity_here
```
> [!WARNING]
> The trailing `/` in the path automatically is replaced with `/**` by Git. Since we're trying to do it by *parent folder*, either a trailing slash or `/**` are a REQUIREMENT! 

### Logging into GitHub accounts

> [!INFO]
> You can skip this if you are not using multiple accounts with GitHub.

#### On protected actions
If you attempt a protected action with a user Git does not have credentials for, it will automatically prompt for log-in.

#### Manually logging in

##### Checking if we're using Git Credential Manager
Run `git config credential.helper`. If it returns `manager`, you can follow the next steps.

##### Logging in to a new account
Run `git credential-manager github login` and follow the prompts.

## Anonymous commit emails
Get your GitHub email from [here](https://github.com/settings/emails), and make sure "Keep my email address private" is checked. The commit email should be after the text "We will instead use" near your primary email.

### Setting the email globally
> [!WARNING]
> This takes priority over includes! Do not do this if you use more than one Git identity frequently.

Run the following command, noting the placeholder:
```
git config --global user.email "1337+REPLACEME@users.noreply.github.com"
```

### Setting per-repo
This requires an intitalized Git repository. Run the following command, noting the placeholder:
```
git config user.email "1337+REPLACEME@users.noreply.github.com"
```



## Git Squash, Rebase, and Merge
### merge (`merge`)
Append the commit history of one branch to another via a merge commit.
```
M=merge commit
O=base commit
X=modifications

main    - O - X - X M 
branch    \ - X - X |
```
### squash (`merge --squash`)
"Squash"es all the commits of a branch down. `HEAD` will be identical to a merge commit, but the main commit history will omit the branch's commits.
```
M=merge commit
O=base commit
X=modifications

main    - O - - - - - - M
branch    \ - X - X - /
```
### rebase (`rebase`)
Changes the base commit of a branch.
```
O=mainline commit
X=modifications

main    - O - O - O -
branch    \ - X - X -
--rebase to HEAD--
main    - O - O - O -   
branch            \ - X - X 
```
