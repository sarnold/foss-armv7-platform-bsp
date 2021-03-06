================================
 VCT ARMv7 Devices BSP Manifest
================================

The various branches (other than this one) available here will configure the
repo build for the appropriate branches in each repository and clone them in
the typical fashion, ie, with either poky or oe-core as the parent directory
and the additional metadata layers underneath (as documented in the upstream
setup).

The main branches build either the poky reference distro or oe-core with your
choice of distro.  Note that oe-core will build "distroless" by default, so
you need to configure the DISTRO setting if you need that.

See the `meta-small-arm-extra README file`_ for manual config setup for the
mainline kernel recipes and the Freescale board pages on the `LinuxOnArm wiki`_

.. _LinuxOnArm wiki: https://eewiki.net/display/linuxonarm
.. _meta-small-arm-extra README file: https://github.com/sarnold/meta-small-arm-extra

There are several main branches for each of the above choices: 

* rocko
* morty
* sumo

Select the main build branch using the github branch button above,
which will select the correct manifest branches and BSP/metadata using the
respective branches in this repo as shown below.

Follow the steps in the readme for the branch you select, then use the same
branch name with the "repo init" command.  This will checkout the corresponding
branch HEAD commits on the configured branch for each repository.

You can get status and more using the ``repo`` command in the top level directory
once you've run the ``repo init`` and ``repo sync`` commands.  Try::

  $ repo info
  $ repo show
  $ repo status

See the default.xml file for repo and branch details; the following example is generic
so go back and select a branch from this repo.

Install the repo utility
------------------------

::

  $ mkdir ~/bin
  $ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
  $ chmod a+x ~/bin/repo

Download the BSP source
-----------------------

::

  $ PATH=${PATH}:~/bin
  $ mkdir boundary-bsp
  $ cd boundary-bsp
  $ repo init -u https://github.com/sarnold/foss-imx6-platform-bsp -b poky-sumo
  $ repo sync

At the end of the above commands you have all the metadata you need to start
building with poky and meta-oe on sumo branches.

.. note:: The layers include more than one device BSP layer.  Be sure to
          enable only the BSP you want in bblayers.conf (eg, for rpi device
          use the meta-raspberrypi BSP layer).

To start a simple image build for a raspberrypi board::

  $ cd poky
  $ source ./oe-init-build-env build-dir  # you choose name of build-dir
  $ ${EDITOR} conf/local.conf             # set MACHINE to raspberrypi2
  $ bitbake core-image-minimal            # for rpi2 board

You can use any directory (build-dir above) to host your build. The above
commands will build an image for XXX using the boundary BSP machine config
and the default fsl-linux kernel.

For some machines, you can replace the default fsl kernel with a patched
mainline kernel; see the armv7multi kernel recipes in meta-small-arm-extra
(note new machines will be added as this manifest evolves).

The main source code is checked out in the bsp dir above, and the build output
dir will default to oe-core/build-dir unless you choose a different path above.

Source code
-----------

Download the manifest source here::

  $ git clone https://github.com/sarnold/foss-imx6-platform-bsp

Using Development and Testing/Release Branches
----------------------------------------------

Replace the repo init command above with one of the following:

For developers - thud

::

  $ repo init -u https://github.com/sarnold/foss-imx6-platform-bsp -b poky-thud

For intrepid developers and testers - master

Patches are typically merged into master-next and then are merged into master
after a testing and comment period. It’s possible that master has
something you want or need.  But it’s also possible that using master
breaks something that was working before.  Use with caution.

::

  $ repo init -u https://github.com/sarnold/foss-imx6-platform-bsp -b oe-master


