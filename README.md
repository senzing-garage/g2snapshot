# g2snapshot

## Overview

The following snapshot utilities analyze the data in a Senzing repository to calculate the following reports:
- **dataSourceSummary** - calculates the duplicates, possible matches and relations by data source.
- **crossSourceSummary** - calculates the duplicates, possible matches and relations across data sources.
- **entitySizeBreakdown** - calculates how many entities have how many records, highlighting possible instances of overmatching.

These reports are placed in a json file that can be viewed with the G2Explorer located 
here: https://github.com/Senzing/g2explorer

It can optionally export the entire entity resolution result set for use in the G2Audit utility located 
here: https://github.com/Senzing/g2audit

Taking a snapshot is part of Senzing's Exploratory Data Analysis toolset which you can read more about here: https://senzing.zendesk.com/hc/en-us/sections/360009388534-Exploratory-Data-Analysis-EDA-

*You will want to install database access as described in the prerequisites below and use G2Snapshot.py on large databases.  If you do not
have database access, you will have to use the G2Snapshot-api-only version which runs slower and has less functionality.*

Usage:

```console
python3 G2Snapshot.py --help
usage: G2Snapshot.py [-h] [-c CONFIG_FILE_NAME] [-o OUTPUT_FILE_ROOT]
                     [-s SAMPLE_SIZE] [-d DATASOURCE_FILTER]
                     [-f RELATIONSHIP_FILTER] [-a] [-D] [-k CHUNK_SIZE]
                     [-t THREAD_COUNT] [-u]

optional arguments:
  -h, --help            show this help message and exit
  -c CONFIG_FILE_NAME, --config_file_name CONFIG_FILE_NAME
                        name of the senzing config file, defaults to
                        /etc/opt/senzing/G2Module.ini
  -o OUTPUT_FILE_ROOT, --output_file_root OUTPUT_FILE_ROOT
                        root name for files created such as
                        "/project/snapshots/snapshot1"
  -s SAMPLE_SIZE, --sample_size SAMPLE_SIZE
                        defaults to 1000
  -f RELATIONSHIP_FILTER, --relationship_filter RELATIONSHIP_FILTER
                        filter options 1=No Relationships, 2=Include possible
                        matches, 3=Include possibly related and disclosed.
                        Defaults to 3
  -a, --for_audit       export csv file for audit
  -D, --debug           print debug statements
```
*the following are not in G2Snapshot-api-only*
```console
  -d DATASOURCE_FILTER, --datasource_filter DATASOURCE_FILTER
                        data source code to analayze, defaults to all
  -k CHUNK_SIZE, --chunk_size CHUNK_SIZE
                        defaults to 1000000
  -t THREAD_COUNT, --thread_count THREAD_COUNT
                        defaults to 0
  -u, --use_api         use api instead of sql to get resume
```

## Contents

1. [Prerequisites](#Prerequisites)
2. [Installation](#Installation)
3. [Typical use](#Typical-use)

### Prerequisites
- Python 3.6 or higher
- Senzing API version 3.0 or higher
- database support for your Senzing database
    - psycopg2 (for postgres)
    - pyodbc (and supporting drivers) for all other databases 

*If using an SSHD container, you should first max it out with at least 4 processors and 30g of ram as the more threads you give it, the faster it will run.  Also, if the database container is not set to 
auto-scale, you should give it a additional resources as well.*

### Installation

1. Place the following files in a directory of your choice:
    - [G2Snapshot.py](G2Snapshot.py) 

2. The senzing environment must be set to your project by sourceing the setupEnv script created for it.


### Typical use

To run for all data sources:
```console
python3 G2Snapshot.py -o /myproject/snapshots/snapshot-mm-dd-yyyy 
```
or if you are planning to perform an audit ...
```console
python3 G2Snapshot.py -ao /myproject/snapshots/snapshot-mm-dd-yyyy
```

This will result in the following files being generated ...
- /myproject/snapshots/snapshot-mm-dd-yyyy.json
- /myproject/snapshots/snapshot-mm-dd-yyyy.csv (if you use -a for_audit option)

*You will want to change the -o output_path to a file and directory of your choice.  It is a good idea though to create a snapshots directory and include the date you took the snapshot in your file name
as snapshots can be compared through time*


Or, for a specific data source:
```console
python3 G2Snapshot.py -o /myproject/snapshots/snapshot1-mm-dd-yyyy -d mydatasource
```

*Please note the -d data source option was added as normally there is **one primary data source** you are trying to resolve against itself 
and see what other data sources match it. For instance you might want to compare your customers against a watch list or reference data such
as a list of registered companies.*

*Do not use the -d option iterively for each data source.   It would be more efficient to run the snapshot wide open as it will analyze
all the data sources against each other.*

Optional parameters:
- The -c config_file parameter is only required if your project's G2Module.ini file can't be found in the usual location.
- The -s sample_size parameter can be added to inlcude either more or less samples in the json file.
- The -f relationship_filter can be included if you don't care about relationships.  It runs faster without computing them.   However, it is highly recommended that you at least include possible matches.
- The -a for_audit parameter can be included if you also want the audit csv file to be generated.
- The -k chunk_size parameter may be required if your database server is running out of temp space. Try 500000 (500k) rather than default of 1 million if you have this problem.
- The -u use_api parameter can be used if it becomes necessary in the future due to database sharding.

**- The -t thread_count parameter can be included to spin up more or less threads than are automatically calculated.**

*With enough database capacity and application threads, you should see speeds of 3-5k entities processed per second.  Its a matter of monitoring the database and sshd container processor utilization. If 
both are below 80%, you can increase the -t thread-count which currently defaults to 4 per processor.*

**- The -d datasource_filter parameter can be used to specify a single data source. This can significantly reduce the time it takes to take a snapshot on a small data source.**

