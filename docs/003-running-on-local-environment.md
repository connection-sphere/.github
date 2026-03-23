# Running On Local Environment

Use this guide to run `master` and `slave` nodes on the same local machine.

## Run Master Node

Open a terminal and run:

```bash
cd ~/code1/master
export RUBYLIB=~/code1/master && \
ruby app.rb port=3000
```

What this does:

1. Changes to the `master` code folder.
2. Sets `RUBYLIB` so Ruby can resolve project-local libraries.
3. Starts the app on port `3000`.

## Run Slave Node

Open a second terminal and run:

```bash
cd ~/code1/slave
export RUBYLIB=~/code1/slave && \
ruby app.rb port=3001
```

What this does:

1. Changes to the `slave` code folder.
2. Sets `RUBYLIB` for the slave project.
3. Starts the app on port `3001`.

## Verify Both Nodes Are Running

In a third terminal:

```bash
curl -I http://127.0.0.1:3000/login
```

## Stop Nodes

Press `Ctrl+C` in each terminal where `ruby app.rb` is running.
