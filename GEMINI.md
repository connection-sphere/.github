You help me to write code for the MassProspecting project.

## Project Overview

Always check out the architecture of MassProspecting here, in order to understand the whole picture about how MassProspecting works.
https://raw.githubusercontent.com/MassProspecting/docs/refs/heads/main/internals/01-architecture.md

Always check out the installation steps of MassProspecting here, in order to understand how MassProspecting is installed in a local environment for development:
https://raw.githubusercontent.com/MassProspecting/docs/refs/heads/main/internals/02b-installation-for-development.md

Always check out the installation steps of MassProspecting here, in order to understand how MassProspecting is installed in a production environment:
https://raw.githubusercontent.com/MassProspecting/docs/refs/heads/main/internals/02a-installation-for-production.md

## Working Repositories

All the working repositories are located into the folder `/home/blackstack/code1`, belonging to the linux user `blackstack`.

The repositories that we are going to work with are:

- /home/blackstack/code1/master
- /home/blackstack/code1/master/extensions/i2p
- /home/blackstack/code1/master/extensions/mass.commons
- /home/blackstack/code1/master/extensions/mass.account
- /home/blackstack/code1/slave
- /home/blackstack/code1/master/extensions/mass.commons
- /home/blackstack/code1/master/extensions/mass.subaccount

## Running Web Servers

I use to run the web-server of the master node locally in a manuall fashion:

```
su - blackstack
cd code1
cd master
export RUBYLIB=~/code1/master
ruby app.rb port=3000
```

I use to run the web-server of the slave node locally in a manuall fashion:

```
su - blackstack
cd code1
cd slave
export RUBYLIB=~/code1/slave
ruby app.rb port=3001
```

## Screenshots

Many times I want to share screenshots with you.

Such screenshots are saved into `/home/leandro/Pictures/Screenshots`

You have to find the newest file there, as the screenshot that I just have taken.



