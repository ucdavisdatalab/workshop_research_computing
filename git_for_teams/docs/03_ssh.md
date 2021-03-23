SSH Keys and GitHub
===================

When you work with remote repositories on GitHub, you'll often need to enter 
your username/password to identify yourself. This is for two reasons: 1) it 
allows GitHub to track who has made changes to what files; 2) it adds a layer 
of security to projects, letting teams control who can make changes to their 
files. Remember too that repositories can be public or private; this layer of 
security also lets teams control who has access to files in the first place.

SSH Keys: A Short Primer
------------------------

It can be a pain, though, to have to enter and re-enter your credentials when 
making changes. More, passwords can be lost or worse, stolen. To avoid these 
problems, we can set up an **SSH key**. SSH keys (short for "Secure Shell") 
are special, machine-readable credentials that allow users to safely connect 
and authenticate with remote servers over unsecure networks. Their encryption 
protocols offer stronger protection than even the most complex passwords and 
they come with the added benefit of freeing users from the need to remember 
a password.

SSH keys have two parts: 

1. A _public key_, which encrypts messages intended for a particular recipient. 
This can be stored on remote servers, or even shared with others, to facilitate 
secure data transfers
2. A _private key_, which deciphers messages encrypted by the public key. Your 
private key is the only thing capable of unlocking what is sent with your public 
key. It stays on your computer and should never be shared with anyone

Beyond what security measures an SSH key brings, it also acts as your digital 
signature. GitHub uses this internally to verify that you are, in fact, who 
you say you are when you commit code to a repository.

Connecting to GitHub with SSH
-----------------------------

GitHub offers thorough, straightforward documentation for setting up an SSH key 
with its services, which we won't repeat here. Instead, please visit the link 
below and follow the instructions there to get yourself set up. During the 
interactive session of this workshop, we will go through these steps together.

[Connecting to GitHub with SSH](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)
