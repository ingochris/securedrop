Tails Guide
===========

To log-in SecureDrop and retreived messages sent by sources, the
journalist must be using the Tails operating system. The admin must also
use Tails to access the *Document Interface* and create new users.

If you followed the `SecureDrop Installation
instructions </docs/install.md>`__ correctly, you should have already
created a *journalist Tails USB* and an *admin Tails USB* and enabled
the persistence volume on each. If you have not, or need to create
another *journalist Tails USB* for a second journalist, follow the steps
below.

If you already know how to boot the *admin Tails USB* or the *journalist
Tails USB* with persistence, you can skip down to the step 'download the
repository'.

Note that for all of these instructions to work, you should have already
installed the main SecureDrop application.

Installing Tails on USB sticks
------------------------------

Tails is a live operating system that is run from removable media, such
as a DVD or a USB stick. For SecureDrop, you'll need to install Tails
onto USB sticks and enable persistent storage.

We recommend creating an initial Tails Live USB or DVD, and then using
that to create additional Tails Live USBs with the *Tails Installer*, a
special program that is only available from inside Tails. *You will only
be able to create persistent volumes on USB sticks that had Tails
installed via the Tails Installer*.

The `Tails website <https://tails.boum.org/>`__ has detailed and
up-to-date instructions on how to download and verify Tails, and how to
create a Tails USB stick. Here are some links to help you out:

-  `Download and verify the Tails
   .iso <https://tails.boum.org/download/index.en.html>`__
-  `Install onto a USB stick or SD
   card <https://tails.boum.org/doc/first_steps/installation/index.en.html>`__
-  `Create & configure the persistent
   volume <https://tails.boum.org/doc/first_steps/persistence/configure/index.en.html>`__

Note for Mac OS X users manually installing Tails
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Tails documentation for `manually installing Tails onto a USB device
on Mac OS
X <https://tails.boum.org/doc/first_steps/installation/manual/mac/index.en.html>`__
describes how to copy the downloaded .iso image to a USB stick in
Section 4, "Do the copy". This section includes the following ``dd``
invocation to copy the .iso to the USB:

::

    dd if=[tails.iso] of=/dev/diskX

This command is *very slow*. In our testing, it took about 18 minutes to
copy the .iso to a USB 2.0 drive. You can speed it up by changing the
arguments to ``dd`` like so:

::

    dd if=[tails.iso] of=/dev/rdiskX bs=1m

Note the change from ``diskX`` to ``rdiskX``. This reduced the copy time
to 3 minutes for us.

Configure Tails for use with SecureDrop
---------------------------------------

Persistence
~~~~~~~~~~~

Creating an encrypted persistent volume will allow you to securely save
information in the free space that is left on the Transfer Device. This
information will remain available to you even if you reboot Tails.
Instructions on how to create and use this volume can be found on the
`Tails
website <https://tails.boum.org/doc/first_steps/persistence/index.en.html>`__.
You will be asked to select from a list of persistence features, such as
personal data. We require that you enable **all** features.

Start Tails and enable the persistent volume
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When starting Tails, you should see a "Welcome to Tails" screen with two
options. Select *Yes* to enable the persistent volume and enter your
password. Select *Yes* to show more options and click *Forward*. Enter
an *Administration password* for use with this specific Tails session
and click *Login*.

Download the repository
~~~~~~~~~~~~~~~~~~~~~~~

The rest of the SecureDrop-specific configuration is assisted by files
stored in the SecureDrop git repository. To get started, open a terminal
and run the following commands to download the git repository. Note that
since the repository is fairly large and Tor can be slow, this may take
a few minutes.

.. code:: sh

    cd ~/Persistent
    git clone https://github.com/freedomofpress/securedrop.git

Passphrase Database
~~~~~~~~~~~~~~~~~~~

As mentioned in the installation documentation, we provide a KeePassX
password database template to make it easier for admins and journalists
to generate strong, unique passphrases and store the securely. Once you
have set up Tails with persistence and have cloned the repo, you can set
up your personal password database using this template.

You can find the template in ``tails_files/securedrop-keepassx.xml``
inside the securedrop repository. Note that you will not be able to
access your passwords if you forget the master password or the location
of the key file used to protect the database.

To use the template:

-  Open the KeePassX program
-  Select ``File``, ``Import from...``, and ``KeePassX XML (*.xml)``
-  Navigate to the location of ``securedrop-keepassx.xml``, select it,
   and click ``Open``
-  Set a strong master password or choose a key file to protect the
   password database
-  Click ``File`` and ``Save Database As``
-  Save the database in the Tails Persistent folder

Set up easy access to the Document Interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To complete setup of the Admin Workstation or Journalist Workstation, we
recommend using the scripts in ``tails_files`` to easily configure Tor
to access the *Document Interface*.

Navigate to the directory with the setup scripts and begin the
installation by typing these commands into the terminal:

::

    cd securedrop/tails_files/
    sudo ./install.sh

Type the administration password that you selected when starting Tails
and hit enter. The installation process will download additional
software and then open a text editor with a file called
*torrc\_additions*.

Edit the file, inserting the *HidServAuth* information for your
SecureDrop instance that you received during the installation process.
The values can be found in
``install_files/ansible-base/app-document-aths``. This information
includes the address to the Document Interface and your personal
authentication string, as seen in the example below:

::

    # add HidServAuth lines here
    HidServAuth gu6yn2ml6ns5qupv.onion Us3xMTN85VIj5NOnkNWzW # client: bob

If you're working on the Admin Workstation, you should also insert the
lines found in ``app-ssh-aths`` and ``mon-ssh-aths``, which will allow
you to connect using a secure shell over Tor to the App and Monitor
Servers. When you are done, click *Save* and close the text editor.

The preceding steps are taken so that no one but the journalist will be
able to access to the *Document Interface*, even if they manage to steal
the journalist's username, password, and two-factor authentication
token.

Our ``install.sh`` sets up an initialization script that automatically
updates Tor's configuration to work with SecureDrop every time you login
to Tails. As long as Tails is booted with the persistent volume enabled
then you can open the Tor Browser and connect to the Document Interface
as normal.

Create bookmarks for Source and Document Interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want, you can open the browser and create bookmarks for the
Source and Document Interfaces. Navigate to the site you wish to
bookmark, select *Bookmarks* and *Bookmark This Page*, give the site a
useful name (e.g. *Source Interface*), and click *Done*. Tails will
remember the bookmarks even if you reboot.
