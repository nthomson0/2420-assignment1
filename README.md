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

## Creating ssh keys
SSH, also known as secure shell, is simply a way for us to remotely access another computer.

SSH keys are generally made up of two parts, the public key, and the private key. The public key can be thought of as a lock that we are using to secure the connection. The private key can be thought of as the key that unlocks the lock. Everybody can see the public key, but private keys are hidden.

Once connected to your Arch Linux Droplet, you can run the following command:
    ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "<name>"

## Creating a Personal Access Token for doctl

## Setting up Cloud-init on your Droplet

## Installing doctl on your Droplet

## 