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

3. Optionally, type `find .ssh` to confirm our key exists.

This will print out all the files within our `.ssh/` directory, given it exists.

## Setting up Cloud-init on your Droplet

## Installing doctl on your Droplet

## 