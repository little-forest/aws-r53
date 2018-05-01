# aws-r53
AWS Route53 manipulation helper script

```
Usege:
  aws-r53 (get|update) [-p PROFILE] [-z ZONE_ID] [-n NAME] [-t TYPE] [-l TTL] [-v VALUE] [-wVDh]
    get:    Get DNS record (type and name should be specified)
    update: Update DNS A recotd
          -p : aws-cli's profile name (Option)
          -z : Route53's Hosted zone ID
          -n : DNS record name (ex: example.com)
          -t : DNS record type (ex: A, CNAME, TXT, ...)
          -v : DNS record value
          -l : TTL to set (default is 300)
          -V : Verbode mode
          -D : dry-run
          -w : Wait for UPSERT change info to be INSYNC
          -h : display usage

Configuration file:
  You can configure some options in '~/.aws-r53rc'. Here is example configuration.
      AWS_CONFIG_FILE=~/.aws/credentials
      ZONE_ID=XXXXXXXXXXXXXXXXXXXX
      PROFILE=aws-r53

Example:
  * Get all record set of specified hosted zone id.
      aws-r53 get -z ZONE_ID -V

  * Get Sepcified DNS record.
      aws-r53 get -z ZONE_ID -t A -n example.com

  * Update DNS A record and wait for synchronization.
      aws-r53 update -z ZONE_ID -t A -n example.com -v IP_ADDRESS -w
```

# Requirements

* aws-cli
* jq
