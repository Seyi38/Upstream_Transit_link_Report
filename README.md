# Upstream and Downstream Transit link Report

## CONTENTS:

1. Autonomous System Number Overview
2. Problem Statement
3. Task Overview
4. Data Overview
5. Data Preprocessing
6. Conclusion


## AUTONOMOUS SYSTEM NUMBER OVERVIEW

An Autonomous System (AS) is a group of one or more IP prefixes (lists of IP addresses accessible on a network) run by one or more network operators that maintain a single, clearly-defined routing policy. Network operators need Autonomous System Numbers (ASNs) to control routing within their networks and to exchange routing information with other Internet Service Providers (ISPs).

There are two different formats to represent ASNs: 2-byte and 4-byte.

A 2-byte ASN is a 16-bit number. This format provides for 65,536 ASNs (0 to 65535). From these ASNs, the Internet Assigned Numbers Authority (IANA) reserved 1,023 of them (64512 to 65534) for private use.

A 4-byte ASN is a 32-bit number. This format provides for 232 or 4,294,967,296 ASNs (0 to 4294967295). IANA reserved a block of 94,967,295 ASNs (4200000000 to 4294967294) for private use.

Breast cancer is cancer that forms in the cells of the breasts

Breast cancer is the most common invasive cancer in women and the second leading cause of cancer death in women after lung cancer. Men can also be diagnosed with breast cancer but it is very rare.

Source: https://www.arin.net/resources/guide/asn/


## PROBLEM STATEMENT

An ISP monitors the external networks they interact with on their upstream and downstream transit link using flow monitors and generate a monthly report of the top 10 interaction on each exit point. The report generated contains the ASN of the different networks and other needed information except the name of the organization. For better reporting to HODs, the name of the corresponding organizations are needed. 

## TASK OVERVIEW

The task is to get network data from an API endpoint and store it in a dataframe. This data contains the autonomous system numbers of different organisations. The code compares the ASN against a dataset of upstream and downsream transit link reports generated at the exit points of the ISP. The ASN is used to extract the corresponding organisation name for each ASN from the downloaded data into the upstream link and downsream transit link dataframe and exported for reporting.

## DATA OVERVIEW

### Data Set Information:

#### Network Data
1. The Network data is in JSON format downloaded from PeeringDB via an API
    Source (https://www.peeringdb.com/)

2. Number of Rows: 23214 

3. Number of Columns: 35

4. Column Names: 
       'id', 'org_id', 'name', 'aka', 'name_long', 'website', 'asn',
       'looking_glass', 'route_server', 'irr_as_set', 'info_type',
       'info_prefixes4', 'info_prefixes6', 'info_traffic', 'info_ratio',
       'info_scope', 'info_unicast', 'info_multicast', 'info_ipv6',
       'info_never_via_route_servers', 'ix_count', 'fac_count', 'notes',
       'netixlan_updated', 'netfac_updated', 'poc_updated', 'policy_url',
       'policy_general', 'policy_locations', 'policy_ratio',
       'policy_contracts', 'allow_ixp_update', 'created', 'updated', 'status'

5. Columns used:
        'id', 'org_id', 'name','asn'
7. Missing attribute values: none

#### Upstream and Downstream Link Data

1. The Upstream and Downstream Link Data is genareted monthly by the ISP and is comprised of 20 csv files generated at each exit point.

2. Number of Columns: 9
3. Column Names:
    'AS Number 1',	'AS Number 2',	'Avg Total Throughput (bits/s)',	'Avg 1 to 2 Throughput (bits/s)',	
    'Avg 2 to 1 Throughput (bits/s)',	'Max Total Throughput (bits/s)', 'Max 1 to 2 Throughput (bits/s)',
    'Max 2 to 1 Throughput (bits/s)',	'Connected Time (seconds)'


### DATA PREPROCESSING

1. The network data from the PeeringDB API was gotten using a get request
2. The JSON keys were printed
3. The relevant key was read into a Pandas dataframe
4. The needed data was read into another Data frame
5. A check for missing values and uniqueness of all AS numbers was done
4. The local file path containing the Upstream and Downstream Link csv files to be imported was specified
5. We iterate over the files in the directory path and store the names of the files in a list for the first
6. We iterate over the csv files in the path and performed the following for each iteration:
  a. Corresponding organisation name for each ASN number is gotten using the map function and stored in a         new column
  b. Replaced "0" with the name of the ISP
  c.  Add '_Export' to the imported file name as the name of the exported files and export to excel.
