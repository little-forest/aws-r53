# About

'aws-r53' is an AWS Route53 manipulation helper script. This script helps us updating Route53's resource record sets, so we can easily implement dynamic DNS.

This script is very simple and has very few dependent packages, so you can easily install it.

# Requirements

In order to use aws-r53, the following commands should be included in PATH environment variable. 

* [aws](https://aws.amazon.com/cli/)
* [jq](https://stedolan.github.io/jq/)

# How to install

1. Install the aws-cli and configure it.
2. Create an IAM user for aws-r53, which is attached a policy as [described later](#required-aws-policy).
3. Download aws-r53.

```
$ curl -o aws-r53 https://raw.githubusercontent.com/little-forest/aws-r53/master/aws-r53
$ chmod +x aws-r53
```

4. Create a file named /etc/aws-r53rc with the following contents. 
```
AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXXXX
AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
ZONE_ID=XXXXXXXXXXXXXXXXXXXX
```


# Usage

```
Usege:
  aws-r53 (get|update|auto-update|show-ip) [-p PROFILE] [-z ZONE_ID] [-n NAME] [-t TYPE] [-l TTL] [-v VALUE] [-wVDh]
    get:         Get DNS record (type and name should be specified)
    update:      Update DNS A record
    auto-update: Update DNS A record using IP_SUPPLIER_SCRIPT
    show-ip:     Show IP address using IP_SUPPLIER_SCRIPT
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

      AWS_PATH=<PATH_TO_AWS_CLI> (option)
      AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXXXX
      AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
      ZONE_ID=XXXXXXXXXXXXXXXXXXXX
      PROFILE=aws-r53
      IP_SUPPLIER_SCRIPT=\"/usr/local/bin/xxxx\"

  If both files are specified, ~/.aws-r53rc will take precedence.

Logging:
  aws-r53 outputs log message to syslog with logging tag "aws-r53".
  You can check logs with following command.

    sudo grep " aws-r53: " /var/log/messages

Example:
  * Get all record set of specified hosted zone id.
      aws-r53 get -z ZONE_ID -V

  * Get Sepcified DNS record.
      aws-r53 get -z ZONE_ID -t A -n example.com

  * Update DNS A record and wait for synchronization.
      aws-r53 update -z ZONE_ID -t A -n example.com -v IP_ADDRESS -w

  * Auto-Update DNS A record.
      (IP address is auto-accuired using IP_SUPPLIER_SCRIPT specified in aws-r53rc)
      aws-r53 auto-update -z ZONE_ID -t A -n example.com -w
```

# Required AWS Policy

aws-r53 requires the following AWS policy.

```javascript
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

