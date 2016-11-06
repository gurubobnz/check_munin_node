# check_munin_node

This command is a bridge between nagios and munin nodes. We use it to provide nagios based alerts for munin nodes that we are interested in. There is a similar project already called check_munin_rrd which works with the data that the munin core collects from the nodes, but our use case is such that the munin rrd files cannot be accessed from our nagios machine (nor do we want to make them available).

```
usage: check_munin_node [-h] --host HOST [--port PORT] [--service SERVICE]
                        [--metric METRIC] [--message MESSAGE]
                        [--warning WARNING]
                        [--warning-message WARNING_MESSAGE]
                        [--critical CRITICAL]
                        [--critical-message CRITICAL_MESSAGE]

Call out to a munin node and ask for a metric, and emit it in a way that nagios can process

optional arguments:
  -h, --help            show this help message and exit
  --host HOST           Host to contact
  --port PORT           Port to contact (optional, default 4949)
  --service SERVICE     Munin service to call (e.g. df, cpu, load, etc.) - if
                        omitted a list of services on the host will be
                        displayed
  --metric METRIC       Metric to fetch the value of (e.g. user.value,
                        load.value - this must be one of the metrics returned
                        by the service)i - if omitted a list of the metrics
                        provided by the service will be displayed
  --message MESSAGE     message to return for this metric - {VALUE} can be
                        used as a placeholder for the return value (optional)
  --warning WARNING     Value at which the metric is in a warning state - if
                        not supplied the value configured on the munin node
                        will be used (optional)
  --warning-message WARNING_MESSAGE
                        message to return if this metric is in a warning state
                        (optional)
  --critical CRITICAL   Value at which the metric is in a critical state - if
                        not supplied the value configured on the munin node
                        will be used (optional)
  --critical-message CRITICAL_MESSAGE
                        message to return if this metric is in a critical
                        state (optional)

Examples:
---
Return a list of services from example.com:

$ ./check_munin_node --host example.com

Return a list of metrics from the df service on example.com:

$ ./check_munin_node --host example.com --service df

Return a value from the _dev_root metric from the df service on example.com, using
the remote munin config to determine the warning and critical states:

$ ./check_munin_node --host example.com --service df --metric _dev_root

Fully specific example of the above:

$ ./check_munin_node --host example.com --service df --metric _dev_root \
	--message "Everything is OK - disk free is {VALUE}%" \
	--warning 75 --warning-message "Hmm, better check disk space - it's at {VALUE}%" \
	--critical 90 --critical-message "Holy cow, there's less than 10% disk free! (currently {VALUE}% full)"
```
