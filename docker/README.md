## How to use

Copy the `docker-compose` file in a working `netapp-harvest` directory.

The directory should be the full NetApp Harvest, as downloaded from NetApp web site, and the NetApp Manageability SDK must have been properly installed in the `lib/` directory. Please consult NetApp Harvest documentation for how to do this.

Once the environment is ready, just execute `docker-compose up -d` to launch a full NetApp Harvest / Graphite / Grafana stack

## Grafana Configuration

When the environment is ready, you still have to create an API key for NetApp Harvest and create a default data source.

### API key

You can access grafana on port 3000, and generate an **Editor** APPI key, then set this key in `docker-compose.yml` in the NetApp Harvest container environment (don't forget to un-comment the line)

### Data source

Then go in Grafana data sources, and create a new default **Graphite** data source pointing to `http://graphite/`

### Restart NetApp Harvest

You can now restart NetApp Harvest to automatically deploy dashboards in Grafana.

`docker-compose up -d`