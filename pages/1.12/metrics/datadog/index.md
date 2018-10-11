---
layout: layout.pug
title: Sending DC/OS Metrics to Datadog
menuWeight: 3
excerpt: Sending DC/OS metrics to Datadog
beta: true
enterprise: false
---


DC/OS 1.12 sends metrics via [Telegraf](/1.12/overview/architecture/components/#telegraf), which may be configured to export metrics to Datadog. There is no need to install a metrics plugin, as in DC/OS 1.9, 1.10, and 1.11. This guide details how to add the appropriate configuration to DC/OS. Please note that this functionality is experimental, and that modifying files in `/opt/mesosphere` is not supported. 

**Prerequisite:**

- You must have the [DC/OS CLI installed](/1.12/cli/install/) and be logged in as a superuser via the `dcos auth login` command.

# Configuring Telegraf to export metrics to Datadog

1. Create a file named `datadog.conf` with the following content:

    ```
    # Transmit all metrics to Datadog
    [[outputs.datadog]]
      ## Datadog API key
      apikey = "my-secret-key"
      ## Connection timeout
      # timeout = "5s"
    ```

1. Replace the `apikey` value with your Datadog API key.

1. On every node in your cluster:

   1. Upload the `datadog.conf` file to `/opt/mesosphere/etc/telegraf/telegraf.d/datadog.conf`
   1. Restart the Telegraf process with your new configuration by running `sudo systemctl restart dcos-telegraf`
   1. Check the Datadog UI to see DC/OS metrics arriving