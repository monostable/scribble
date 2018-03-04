This tutorial is for the [iCEstick Evaluation Kit](http://www.latticesemi.com/icestick) which you can get for about $25 [from](http://uk.farnell.com/lattice-semiconductor/ice40hx1k-stick-evn/ice40-hx1k-icestick-eval-kit/dp/2355207?ost=icestick&selectedCategoryId=&categoryNameResp=All%2BCategories&iscrfnonsku=false) [various](http://www.mouser.com/ProductDetail/Lattice/ICE40HX1K-STICK-EVN/?qs=%2fha2pyFaduiOEqlsaiRfBulNsZFFFJWzq2a0PhVAJbo%3d) [places](http://www.digikey.com/product-search/en?keywords=%20icestick).

We want to spend as little time as possible on the setup. If you are already comfortable with Bash and compilation and have your system set up for that you can follow [the instructions](http://www.clifford.at/icestorm/) from the Ice Storm project but below is an easier route using Vagrant.

Vagrant will set up a virtual machine for you, install all the right tools and make sure the USB connection to the iCEstick is passed through. This should work on any machine be it Linux, Windows or OSX.

1. [Install Virtualbox](https://www.virtualbox.org/wiki/Downloads)
2. [Install Vagrant](https://www.vagrantup.com/)
3. [Install Git](https://git-scm.com/download/)
4. Open a terminal (use Git Bash on Windows) and clone the icestorm-vagrant repository.

```
git clone https://github.com/monostable/icestorm-vagrant
```
(The code-block above indicates this is a bash command to be typed into your terminal.)

We now create the Vagrant machine.

```
cd icestorm-vagrant
vagrant up
```

The above will set up a 64-bit Ubuntu 14.04 virtual machine with all the required tools installed. The compilation steps can take a while so go grab a coffee or something.

After it completes you can login to your newly created virtual machine.

```
vagrant ssh
```

This should say "Welcome to Ubuntu..." and present you with a prompt that looks like this:
```
vagrant@vagrant-ubuntu-trusty-64:~$
```

We are now all on the same page and have the same system with the same tools installed.

Next we will run a simple example through the entire toolchain workflow to end up with some hardware set on your iCE40 FPGA.

