# Swift Grok Patterns

This repo is dedicated to **swift logs** and **logstash** grok patterns, which are intended to feed indices into Elasticsearch.
However, the grok patterns and filter conf files can be included in other Elasticsearch pipelines, like Filebeat, or others.

## Setup

You need to move `conf.d/*` and `extra_patterns` under `/etc/logstash/` and restart the Logstash service.

 * `extra_patterns/swift`: Customized grok patterns to process Swift logs using regular expressions.
 * `conf.d/10 ~ 30*`: The filter conf files scan the Swift logs and apply tags.

### Resource

SwiftStack leverages *rsyslog* as log sender and receiver. Check the resource folder for additional information.

### Grok Patterns Schema
 * [swift grok pattern schema](https://docs.google.com/a/swiftstack.com/spreadsheets/d/e/2PACX-1vTnWdxCnTi47YQ-rLSCL4e4cuTasL102Zc1pktm5AZk8hj-2hvNr-y0upzubazEZ1-x51ncAAOMYnSD/pubhtml)

 * [openstack swift offical log pattern](https://docs.openstack.org/developer/swift/logs.html)
