---
title: "Create a Linux Virtual Machine on a Mac"
date: 2023-03-25T08:28:37Z
draft: false
toc: true
authorbox: true
categories:
  - Linux
---

As a technical writer, I often need to work with different operating systems and environments to test applications. It would be a nuisance if I needed a separate machine for every test case. Happily, it's possible to run multiple operating systems on a single machine.

In this tutorial, I'll explain how to create a Linux virtual machine (VM), using two tools: VirtualBox and Vagrant. VirtualBox provides the virtualization and Vagrant automates the creation and configuration of virtual machines.

By the end, you'll have a Linux VM that you can quickly spin up in moments.

## Step 1: Install VirtualBox

To create virtual machines, you need a virtualization platform. There are lots to choose from, but I'm using VirtualBox. Why? Because it's both free and easy to use.

Go to [VirtualBox downloads](https://www.virtualbox.org/wiki/Downloads) and choose the version for your Mac: either Intel or Arm64. If your Mac has an M1 or M2 chip, you need the **Developer preview for macOS/Arm64**. Note the warnings that this version is likely to contain bugs.

![Download VirtualBox for Mac](/images/virtualbox-versions.png)

Once you've double-clicked the downloaded file, you're guided through the installation process.

Open VirtualBox to make sure it's working. You should see a welcome screen:

![Welcome to VirtualBox](/images/virtualbox-welcome.png)

## Step 2: Install Vagrant

Although you could just work with VirtualBox directly, HashiCorp's Vagrant makes it easier to create and configure virtual machines.

Go to [Vagrant downloads](https://developer.hashicorp.com/vagrant/downloads) and choose the best installation method for you. You can either download the binary or use the [package manager Homebrew](https://brew.sh). There's only one version for macOS.

If you already have Homebrew installed, run the following command:

```shell
brew install hashicorp/tap/hashicorp-vagrant
```

Next, verify that Vagrant was installed correctly:

```shell
vagrant --version
```

You should see a response similar to:

```shell
Vagrant 2.3.4
```

You have everything you need to create a virtual machine :tada:

## Step 3: Create a Virtual Machine

On Vagrant Cloud, you'll find a [catalogue of boxes](https://app.vagrantup.com/boxes/search), or virtual machine images, to download.

You can either browse to find what you want, or search at the top of the page. Not all boxes work on VirtualBox, so you might find it helpful to filter the results.

![Browse Vagrant Cloud](/images/vagrant-cloud.png)

The first part of the name is the company who maintains the box. You could be taking a risk if you download a box from an unknown person or organization. It's also important to consider how recently the box was updated. 

There are hundreds of Linux distributions and versions available. 

I'm choosing `bento/ubuntu-20.04`, as it's a company I recognize and the box has been updated recently. 

For installation instructions, select the box, then the **New** tab:

![Use Vagrant box](/images/vagrant-use-box.png)

Before you can run those commands, you need a directory for your virtual machine. This is where Vagrant stores the necessary files. Switch to your newly created directory and run the first command:

```shell
vagrant init bento/ubuntu-20.04
```

This command instructs Vagrant to initialize a virtual machine using the specified box.

You'll see a message that says:

```shell
A `Vagrantfile` has been placed in this directory. You are now ready to `vagrant up` your first virtual environment!
```

Before you `vagrant up`, take a quick peek at the `Vagrantfile`. This very long file contains all the configuration for your virtual machine. Most of it is commented out, but there are options for specifying the amount of memory and to install additional software. Lurking in the middle, you'll see the box you chose:

![Vagrantfile](/images/vagrant-vagrantfile.png)

For now, just use the default Vagrantfile by running:

```shell
vagrant up
```

You'll see a message to say "Box 'bento/ubuntu-20.04' could not be found. Attempting to find and install..." This is because Vagrant needs to download that box (or virtual machine image) before it can run it. If you had previously used this box, it would already be available locally. Of course, the download time depends on the speed of your internet connection. A box like this probably takes 2-5 minutes.

Lots of information pops up on the screen. Eventually, you'll see the pleasing announcement: **"Machine booted and ready"**.

For confirmation, open VirtualBox. You should see that a virtual machine has appeared in the list.

![New virtual machine in VirtualBox](/images/virtualbox-new-vm.png)

It's alive!

## Step 4: Connect to your virtual machine

Vagrant makes it simple to interact with your virtual machine. You might have noticed during bootup that it created an SSH username and password for you. I'm sure you don't want any more passwords to remember. Fortunately, Vagrant has also created and stored a key.

To connect to your virtual machine, run the following command: `vagrant ssh`

The stored key lets you straight in :key:

You'll see that your prompt has changed. The prompt is now for your Ubuntu box, not your Mac. You're logged in as a user called `vagrant`.

![Vagrant SSH](/images/vagrant-ssh.png)

Now you can interact with this machine. 

One of the great features of Vagrant is that it allows you to share files between your local and virtual machines.

On your virtual machine, run `ls /vagrant`. Here you'll see the `Vagrantfile` you used to boot up. Any files you place in this directory are available to your virtual machine. This means you can use your favourite editor on your Mac to create and edit files, then make them available to your VM. Also, you can generate test results on your VM and access them directly from your Mac.

Once you've finished, enter `exit` to return to your Mac prompt.

## Step 5: Tear down your virtual machine

There are three main methods for tearing down or stopping your virtual machine:

- `vagrant suspend`
- `vagrant halt`
- `vagrant destroy`

If you want to pause your VM temporarily and retain all the resources, use the `vagrant suspend` command. This retains the machine's current running state.

Alternatively, you can shut down your VM gracefully with `vagrant halt`. This preserves the disk contents. This method takes longer to restart than `vagrant suspend`.

The `vagrant destroy` command removes all traces from your system, thereby reclaiming disk space and RAM. It's not as destructive as it sounds. The `Vagrantfile` remains, so you can use `vagrant up` at any time to restore it. However, it'll take longer than `vagrant halt` or `vagrant suspend`.

## Conclusion

As you've seen, VirtualBox and Vagrant provide a neat solution to running a Linux virtual machine on your Mac.

Inevitably, there are snags. I couldn't get VirtualBox to work on my M2 Mac, although it was fine on my ancient MacMini. The developers of VirtualBox have experienced some problems with Arm64 Macs. It's also important to emphasize that this version is still in beta and subject to bugs :bug:

If you don't have an elderly Mac, look out for a future tutorial on using Terraform (another HashiCorp product) to quickly spin up and tear down a virtual machine on Amazon Web Services.

In the meantime, there's lots more you can do with both VirtualBox and Vagrant. Take a look at the [Vagrant Catalog](https://app.vagrantup.com/boxes/search) and [documentation](https://developer.hashicorp.com/vagrant/docs) for other ideas.

You can also watch a video of this tutorial:

{{< youtube KAd7FafXfJQ >}}
