# Docker in vagrant
This repository contains a vagrantfile for easily working with docker on
Windows and OSX. The box used is based on the excellent boxes from
[bento](https://github.com/chef/bento). This project uses a simplified version
of the Debian box from the bento repository.

When using this project you'll still need to install the docker cli locally,
and when working with compose you'll also need to install docker-compose
locally.

## Dependencies

* [Virtualbox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)
* [Docker](https://www.docker.com/)
* [Docker Compose](https://docs.docker.com/compose/) (optional)
* [Packer](https://www.packer.io/) (optional, only for building the box)

## Usage
The best way to work with this box is to start it when starting to work on any
project. To do this simply clone this project using git or download the project
tarball and extract it to some location, e.g.:

    git clone https://github.com/tweedegolf/vagrant-docker.git

Then while inside the project directory run:

    vagrant up

This should start the box, and you should be ready to start using docker as if
it was running on a linux machine. Note that the box only allows for projects
in your user home folder. Files outside your user home cannot be read. You may
want to change the `Vagrantfile` to be able to read these.

### OSX/Linux
To start working with projects you still need to point your local docker client
to the virtual machine. To do this you can run the following command:

    export DOCKER_HOST=tcp://127.0.0.1:2375

You may want to put this line in your `.bashrc`, `.bash_profile` or `.profile`,
so you don't have to put it in every shell you open up. Note that using this
is incompatible with using Docker for Mac/Windows. If you want to use any of
those you'll have to run `export DOCKER_HOST=` to remove the export.

### Windows
Under Windows we can set an environment variable in cmd using:

    set DOCKER_HOST=tcp://127.0.0.1:2375

When using a powershell you can use:

    $env:DOCKER_HOST = "tcp://127.0.0.1:2375"

You may want to put the environment variable in your default settings. Changing
this depends on the Windows version you're using. See some of these
[instructions](http://www.computerhope.com/issues/ch000549.htm) on setting
environment variables.

## Development
The box is built using [packer](https://www.packer.io/). To create a new
version of the box make sure you have packer installed and then run:

    packer build debian-jessie.json

This will build and upload the debian jessie box to the HashiCorp Atlas. Note
you'll need to have installed virtualbox in order for this to work.
