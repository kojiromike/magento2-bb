# Chronicle of a Magento 2 Module

I want to learn about Magento 2. I'm going to try to write down what I discover as I go.

## Set Up a Web-Browsable Magento 2 with Sample Data and a Dev Environment

I'll use Docker. Docker's great for creating reusable and sharable environments, but the filesystem share with VirtualBox is very slow. So I won't share with OS X at all. I'll install everything into a Docker Volume Container, and then use Docker containers for every interaction, including even git and file editing (I'll use vim, but it could be done with anything).

### Install Magento

### Set up Vim, XDebug and Vdebug

The tricky part here is to figure out how one Docker container sees another. Getting XDebug on the web server to connect back to Vdebug in Vim in a container that isn't actually the same container as is running the web browser is going to require more work and is a bit offtopic; for now, I've just hardcoded XDebug to connect to the Docker host, and let the Vim container publish its listen port there.

