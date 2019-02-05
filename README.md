# JIRA importers for RT and Spiceworks

Import issues to JIRA from [RT](https://bestpractical.com/request-tracker/) and [Spiceworks](https://www.spiceworks.com/free-help-desk-software/). Designed to be used as one time migration tools into JIRA.

Both use the [JIRA JSON importer format](https://confluence.atlassian.com/adminjiraserver/importing-data-from-json-938847609.html).

To import the generated JSON into JIRA, browse to the admin page, then under "Import and Export" select "External System Import" and select the JSON option.

#### Installation

These scripts need a few CPAN modules installed. The easiest way to do that is using [cpanminus](https://cpanmin.us/)

```
git clone https://github.com/jamesbraid/jira
cd jira
cpanm -l deps --installdeps .
```

This will download and install the modules from CPAN to a ``deps`` dir. To have perl use that directory, set the ``PERL5LIB`` environment variable:

```
export PERL5LIB=deps/lib/perl5
```

You should then be able to run the scripts.

### rt-jira

This script uses the [RT REST API](https://rt-wiki.bestpractical.com/wiki/REST) to export tickets from [Request Tracker](https://bestpractical.com/request-tracker/) and convert them into a usable JSON file for import into JIRA.

Usage:

```
rt-jira \
    --rt-url http://your-rt-server \
    --rt-user username \
    --rt-pass password \
    --rt-query 'Queue=queue-to-export' \
    --jira-project-name 'Test Project' \
    --jira-project-key TEST \
    --filename export.json
```

Where 

 * rt-url is the URL to your RT server
 * rt-user and rt-pass are a user with access to the tickets you are exporting 
 * rt-query is the query to select the tickets to export; Queue=queue-name will export all tickets in queue-name queue.
 * jira-project-name is the name of the destination JIRA project
 * jira-project-key is the short "key" for the destination JIRA project, used to generate issue IDs by JIRA
 * filename is the output file generated for import into JIRA

### spiceworks-jira 

This script takes a [Spiceworks JSON export](https://community.spiceworks.com/support/help-desk/docs/import-tickets) and transforms it into a format usable by the JIRA JSON importer.

Usage:

```
spiceworks-jira \
    --spiceworks input.json \
    --jira-project-name 'Spiceworks Import' \
    --jira-project-key SPICE \
    --filename export.json
```

Where

 * spiceworks is the filename of the JSON export from Spiceworks
 * jira-project-name is the name of the destination JIRA project
 * jira-project-key is the short "key" for the destination JIRA project, used to generate issue IDs by JIRA
 * filename is the output file generated for import into JIRA
