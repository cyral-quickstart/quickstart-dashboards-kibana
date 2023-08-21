# Kibana Dashboards and Elastic Index Templates

This repo contains example files that can be used for visualizing Cyral data activity logs.

# Installation

This guide assumes that you have the correct permissions to import saved objects and create index templates in your Kibana / Elasticsearch installation.

Before configuring a Cyral sidecar to send logs to your Elasticsearch, Logstash, Kibana (ELK) instance, you will need to first install the provided [index_template](index_templates/cyral_data_activity_log.json) file.

After this is installed, you can import the provided dashboards that will provide example visualizations of your Cyral data activity logs.

## Index Template

1. Login to Kibana
2. Navigate to Dev Tools located under the Management section
3. Add the below request + JSON into the left pane where the complete JSON is the contents of the [index_templates/cyral_data_activity_log.json](index_templates/cyral_data_activity_log.json)
   ```PUT _index_template/cyral_index_template
{
  "index_patterns": [
    "filebeat-*",
    "cyral-data-activity-logs*"
  ],
  "version": 1,
  "_meta": {
    "description": "Cyral Index Template Pattern for creating elasticsearch indices"
  },
  ...
4. click the Click to send request button (it looks like a play button at the top right corner of the pane)

### Additional Steps (Optional)

If this ELK instance already has some `cyral-data-activity-log*` indexes, then you’ll need to possibly reindex those. If you are receiving shard errors on those existing `cyral-data-activity-logs*` indexes, perform the following steps.

1. You’ll need to reindex each old index so that it will be built using the provided template using the Dev Tools request similar to the below. This command should tell ELK to copy the cyral-data-activity-logs-2023.05.05 into cyral-data-activity-logs-2023.05.05-new and reindex it. As this matches our pattern above for the template, cyral-data-activity-logs*, it should be correctly indexed into the format that our dashboards and scripted fields expect.
   - This does not accept wildcards so it’ll need to be repeated for each index that exists.
  ```POST _reindex
{
  "source": {
    "index": "cyral-data-activity-logs-2023.05.05"
  },
  "dest": {
    "index": "cyral-data-activity-logs-2023.05.05-new"
  }
}
2. Once an index has been reindexed, you’ll want to delete the old index so that you no longer receive any errors. You can issue the below Dev Tools request for each index to delete it
  ```DELETE cyral-data-activity-logs-2023.05.05

## Example Dashboards

1. Login to Kibana
2. Navigate to Stack Management located under the Management section
3. Click on Saved Objects
4. Click on the Import link in the upper right corner
5. Click to browse for a file to import
6. Locate the [Example dashboard](dashboards/Standard_dashboard_v2.0.ndjson) in this repo
7. Make sure `Automatically overwrite all saved objects?` is enabled so that it will update any existing Cyral examples
8. Click the `Import` button
