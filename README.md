# Aboud

'aws-r53' is an AWS Route53 manipulation helper script. This script helps us updating Route53's resource record sets, so we can easily implement dynamic DNS.

This script is very simple and has very few dependent packages, so you can easily install it.

# Requirements

In order to use aws-r53, the following commands should be included in PATH environment variable. 

* [aws](https://aws.amazon.com/cli/)
* [jq](https://stedolan.github.io/jq/)

# How to install

1. Install aws-cli and configure it.
1. Create IAM user for aws-r53 which is attached a policy as described later.
1. Download aws-r53.

```
$ curl -o aws-r53 https://raw.githubusercontent.com/little-forest/aws-r53/master/aws-r53
$ chmod +x aws-r53
```

1. Create a file named /etc/aws-r53rc with the following contents. 

```
AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXXXX
AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
ZONE_ID=XXXXXXXXXXXXXXXXXXXX
```


# Usage

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
  You can configure some options in '~/.aws-r53rc' or '/etc/aws-r53rc'. Here is example configuration.
      AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXXXX
      AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
      ZONE_ID=XXXXXXXXXXXXXXXXXXXX
      PROFILE=aws-r53
  If both files are specified, ~/.aws-r53rc will take precedence.

Example:
  * Get all record set of specified hosted zone id.
      aws-r53 get -z ZONE_ID -V

  * Get Sepcified DNS record.
      aws-r53 get -z ZONE_ID -t A -n example.com

  * Update DNS A record and wait for synchronization.
      aws-r53 update -z ZONE_ID -t A -n example.com -v IP_ADDRESS -w
```

# Required AWS Policy

aws-r53 requires the following AWS policy.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "route53:GetChange",
                "route53:GetHostedZone",
                "route53:ListHostedZones",
                "route53:ListResourceRecordSets",
                "route53:ChangeResourceRecordSets"
            ],
            "Resource": "*"
        }
    ]
}
```

