# List of Processes
 
## 1. Master Node

| Process                               | Description                                                                                       |
|---------------------------------------|---------------------------------------------------------------------------------------------------|
| nginx                                 | Nginx is used as a reverse proxy, listening on port 443 (https) and redirecting to port 3000 (http). |
| app.rb (port=3000)                    | A [Sinatra web server](https://sinatrarb.com/) that serves the platform's webpages.               |
| extensions/mass.account/p/allocate.rb | Finds an available slave node and signs up a sub-account there.                                   |
| ipn.rb                                | Part of the [my.saas i2p extension](https://github.com/leandrosardi/i2p), used for processing PayPal transactions. |
| expire.rb                             | Removes credits from users who are no longer subscribed (part of the i2p extension).              |
| baddebt.rb                            | Cancels subscriptions for users failing payment (details [here](https://github.com/ConnectionSphere-DEPRECATED/cs-issues/issues/69)). |
| movement.rb                           | Updates the snapshot table to show the net balance of accounts.                                   |

## 2. Slave Nodes

| Process                                  | Description                                                                                         |
|------------------------------------------|-----------------------------------------------------------------------------------------------------|
| nginx                                    | Nginx acts as a reverse proxy, listening on port 443 (https) and redirecting to port 3000 (http).   |
| app.rb (port=3000)                       | A [Sinatra web server](https://sinatrarb.com/) serving the platform's webpages.                     |
| extensions/mass.subaccount/p/ingest.rb   | Moves records from the `import` table to the `import_row` table.                                    |
| extensions/mass.subaccount/p/import.rb   | Processes each record in `import_row` and merges them into the `lead` table.                        |
| extensions/mass.subaccount/p/plan.rb     | Creates requests for inbox checks, connection checks, jobs, enrichment, and outreach.               |
| extensions/mass.subaccount/p/rule.rb     | Handles active rules, triggers records, evaluates filters, and manages outreach, enrichment, and tagging actions. |
| extensions/mass.subaccount/p/timeline.rb | Manages a snapshot of the `timeline` table for tracking performance statistics.                     |

## 3. Worker Nodes

| Process                                 | Description                                                                                         |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------|
| extensions/mass.subaccount/p/launch1.rb | Retrieves a list of basic profiles assigned to the worker node and launches a `basic.rb` process for each profile. |
| extensions/mass.subaccount/p/launch2.rb | Retrieves a list of MTA, API, or RPA profiles assigned to the worker node and launches a `profile.rb` process for each. |
