---
title: PostgreSQL Integration
tags: [integrations list]
permalink: postgresql.html
summary: Learn about the Wavefront PostgreSQL Integration.
---
## PostgreSQL Integration

PostgreSQL is a popular open source database. This integration installs and configures Telegraf to send PostgreSQL server metrics into Wavefront. Telegraf is a light-weight server process capable of collecting, processing, aggregating, and sending metrics to a [Wavefront proxy](https://docs.wavefront.com/proxies.html).

In addition to setting up the metrics flow, this integration also installs a dashboard. Here's the databases section of a dashboard displaying PostgreSQL metrics:
{% include image.md src="images/postgres_metrics.png" width="80" %}


To see a list of the metrics for this integration, select the integration from <https://github.com/influxdata/telegraf/tree/master/plugins/inputs>.
## PostgreSQL Setup



**Note:** If you use vRealize Operations, the application proxy agent sets up the integration for you. See the [setup instructions](https://YOUR_CLUSTER.wavefront.com/integration/vrops/setup). Otherwise, follow the setup steps on this page.

### Step 1. Install the Telegraf Agent

This integration uses the PostgreSQL input plugin for Telegraf. If you've already installed Telegraf on your server(s), you can skip to Step 2.

Log in to your Wavefront instance and follow the instructions in the **Setup** tab to install Telegraf and a Wavefront proxy in your environment. If a proxy is already running in your environment, you can select that proxy and the Telegraf install command connects with that proxy. Sign up for a [free trial](http://wavefront.com/sign-up/?utm_source=docs.vmware.com&utm_medium=referral&utm_campaign=docs-front-page){:target="_blank" rel="noopenner noreferrer"} to check it out!

### Step 2. Configure PostgreSQL Input Plugin

Create a file called `postgresql.conf` in `/etc/telegraf/telegraf.d` and enter the following snippet:
{% raw %}
```
[[inputs.postgresql_extensible]]
  # specify address via a url matching:
  # postgres://[pqgotest[:password]]@localhost[/dbname]?sslmode=...
  # or a simple string:
  #   host=localhost user=pqotest password=... sslmode=... dbname=app_production
  #
  # All connection parameters are optional.  
  # Without the dbname parameter, the driver will default to a database
  # with the same name as the user. This dbname is just for instantiating a
  # connection with the server and doesn't restrict the databases we are trying
  # to grab metrics for.
  #
  address = "host=localhost user=postgres sslmode=disable"
  # A list of databases to pull metrics about. If not specified, metrics for all
  # databases are gathered.
  # databases = ["app_production", "testing"]
  #
  # Define the toml config where the sql queries are stored
  # New queries can be added, if the withdbname is set to true and there is no
  # databases defined in the 'databases field', the sql query is ended by a 'is
  # not null' in order to make the query succeed.
  # Be careful that the sqlquery must contain the where clause with a part of
  # the filtering, the plugin will add a 'IN (dbname list)' clause if the
  # withdbname is set to true
  # Example :
  # The sqlquery : "SELECT * FROM pg_stat_database where datname" become
  # "SELECT * FROM pg_stat_database where datname IN ('postgres', 'pgbench')"
  # because the databases variable was set to ['postgres', 'pgbench' ] and the
  # withdbname was true.
  # Be careful that if the withdbname is set to false you don't have to define
  # the where clause (aka with the dbname)
  #
  # the tagvalue field is used to define custom tags (separated by comas).
  # the query is expected to return columns which match the names of the
  # defined tags. The values in these columns must be of a string-type,
  # a number-type or a blob-type.
  #
  # Structure :
  # [[inputs.postgresql_extensible.query]]
  #   sqlquery string
  #   version string
  #   withdbname boolean
  #   tagvalue string (coma separated)
  [[inputs.postgresql_extensible.query]]
    sqlquery="SELECT * FROM pg_stat_database where datname"
    version=901
    withdbname=false
    tagvalue=""
  [[inputs.postgresql_extensible.query]]
    sqlquery="SELECT * FROM pg_stat_bgwriter"
    version=901
    withdbname=false
    tagvalue=""
```
{% endraw %}

### Step 3. Restart Telegraf

Run `sudo service telegraf restart` to restart your agent.
