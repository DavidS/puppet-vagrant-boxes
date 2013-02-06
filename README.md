This repository contains definitions and files necessary for recreating vagrant boxes suitable for running module tests with.

You can access to the produced vagrant boxes here:

<a href="http://puppet-vagrant-boxes.s3.amazonaws.com/index.html">http://puppet-vagrant-boxes.s3.amazonaws.com/index.html</a>

The primary goal is to create boxes that have very little specialisation at this level, the standard veewee templates should be very rarely modified to keep it simple.

## Environment setup

*Note:* You'll need Ruby 1.9.3 and Bundler before you begin.

    $ cd <path to repo>
    $ bundle install

## Adding a new definition

Get a list of available definitions:

    $ veewee vbox templates

Pick one, and define it using:

    $ veewee vbox define '<box_name>' '<template_name>'

Make sure <box_name> follows the convention:

    <osdistro>-<derivative>-<version>-<arch>

* osdistro: for example: centos, ubuntu, windows
* derivative: (optiona) server, desktop
* version: 8, 2008, 1104
* arch: x64, i386

Examples:

    ubuntu-server-1104-x64
    centos-58-i386
    windows-2008r2-x64

Naming caveats:

* The name becomes the hostname of the box ... so you have to be careful.
* Debian/Ubuntu doesn't like underscores in the name.
* A dot in the box name would create a subdomain, which is probably not desirable.

Once you've produced a new box you should modify html/index.html to add it so it can be accessed from the index on S3.

Finally, follow the next steps for building a box.

## Building a box

Pick a box to build:

    $ veewee vbox list

The build it:

    $ veewee vbox build centos-58-x64

At this point it will download necessary ISO's and start building a box.

Now validate the box:

    $ veewee vbox validate 'centos-58-x64'

And export the vm to a .box file:

    $ vagrant basebox export 'centos-58-x64'

## Publishing

Now upload the box located in the current directory 'centos-58-x86_64.box' to the correct S3 bucket, and if you modified the index.html, upload that as well.

If the image is new, or you changed the name, also update the site http://vagrantbox.es/ as well, you can find the repository here: http://github.com/garethr/vagrantboxes-heroku
