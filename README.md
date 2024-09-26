# Creating Digital Ocean Droplets using an existing Droplet
By the end of this tutorial you will have a good understanding of how to use an existing Digital Ocean Droplet to create new Droplets. This will be particularly useful in the future when you may need to create multiple Droplets with the same parameters.

Here is a brief overview of the steps you will take to accomplish your goal:
- Create ssh keys.
- Create a Personal Access Token for doctl to access your Digital Ocean account.
- Setup a Cloud-init configuration file for your new Droplets.
- Utilize doctl to create new Droplets using the Cloud-init config file, as well as your new ssh key.

To complete this tutorial you will just need a few things:
- Internet access.
- A Digital Ocean account.
- A Droplet running Arch Linux.
- A shell on your local machine to connect to your Droplet.

**Note:** Everything from here forward will be using your Arch Linux Droplet unless otherwise specified.

## Creating a Personal Access Token
Just before we hop into our Arch Droplet, we are going to create a Personal Access Token so that we can use our Digital Ocean account remotely later on.

1. Open https://cloud.digitalocean.com/account/api/tokens on your local machine
2. Click **Generate New Token**
3. Give your token a meaningful name, and allow full access.

**Note:** You can choose what commands your token has access to, but for this tutorial we will just set a quick expiry and allow all access.

## Creating SSH Keys
SSH, also known as secure shell, is simply a way for us to remotely access another computer.

SSH keys are generally made up of two parts, the public key, and the private key. The public key can be thought of as a lock that we are using to secure the connection. The private key can be thought of as the key that unlocks the lock. Everybody can see the public key, but private keys are hidden.

1. Run the following command, filling in the `<name>` section with a nickname for your key:
`ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "<name>"`

- `ssh-keygen` is the command to create an ssh key.
- `-t` is to signify the type of key to create.
- `ed25519` is the type of key we are choosing to create.
- `f` is the filename for the key file
- `~/.ssh/do-key` is the key file. It's worth noting that `<do-key>` can technically be any file name, we are just using it as a short hand for "DigitalOcean-key".
- `-C` is to provide a comment.
- `<name>` is our comment. We use this just to give the key a name for ourselves.

2. Enter a blank passkey.

**Note:** Generally you should enter a passkey, but in this situation you will not have any sensitive information on your Droplet so it is not neccessary.

If successful, you should now see a key fingerprint and some random art.

Optionally you can type `find .ssh` to confirm your key exists.

This will print out all the files within our `.ssh/` directory, given it exists.

## Setting up Cloud-init
Cloud-init is essentially used for launching remote cloud servers. In this case we will be creating a cloud-init configuration file for any new Droplets we creating. This is useful because we can do things like initialize users, set their privileges, and pre-download packages.

1. Type `nvim cloud-init-arch-config.yml` to create a .yml cloud-init config file using neovim.

- A .yml or YAML file is essentially a more readable data file, kind of like json.

2. Copy and paste the code block below into neovim, filling in the `<name>` sections with a nickname. Once you're done you can type `:exit` to exit nvim.
```
#cloud-config
users:
  - name: <name>
    primary_group: <name>
    groups: wheel
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    shell: /bin/bash
    ssh-authorized-keys:
      - <ssh public key>

packages:
  - ripgrep
  - rsync
  - neovim
  - fd
  - less
  - man-db
  - bash-completion
  - tmux

disable_root: true
```

Now we have to go back and copy our SSH key so that we can add it to our config file.

3. Type `cat .ssh/do-key.pub`, this will output the public key we made earlier given you also named it `do-key`.

4. Copy the public key to the clipboard.

5. Type `nvim cloud-init-arch-config.yml` to edit our file with neovim.

6. Click **i** to enter insert mode, and then paste your public key where it says `<ssh public key>`, deleting the placeholder text before doing so.

7. Click **esc** to exit insert mode.

At this point your file should look something like this:

`IMAGE HERE`

8. Type `:exit` once again to leave nvim.

You've now successfully created a cloud-init config file, but what did we even put in that file?

- `users` specifies the follow entries are us adding a user.
    - `name` is the name of the user.
    - `primary_group` is the group associated to this user. It's good practice for this to just be the same as the user's name.
    - `groups` are additional groups that we are adding the new user to. In this case we are adding them to the `wheel` group, which allows them to use `sudo` which elevates commands to admin privilege.
    - `sudo` specifies what the sudo command allows the user to do. In this case they are allowed to do anything, without any password to block them.
    - `shell` specifies the location of the shell.
    - `ssh-authorized-keys` adds the corresponding key to the user's authorized keys file.
- `packages` preinstalls all the corresponding packages to the user's instance.
- `disable_root` disables access to the root user on the instance. This is good practice since the user can already use commands at a root user's privilege level using sudo. We can also track users usage of the sudo command, whereas if we allow access to the root user we could not.

## Installing doctl

## 