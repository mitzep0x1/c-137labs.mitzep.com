---
title: Isolation | Wazuh Live Response
date: 2025-03-12 06:00:00 -0600
categories: [BlueOps, Response]
tags: [difficulty:medium, siem:wazuh]
image:
  path: https://github.com/user-attachments/assets/e2e31225-45e8-4583-bf71-d8ae540a88fc
---

In security incident response, the ability to quickly isolate a compromised endpoint is crucial. [WLR Isolation](https://github.com/lr2t9iz/wazuh-live-response/wiki/WLR-Isolation), a submodule of Wazuh Live Response, allows security teams to remotely isolate a Windows endpoint using Wazuh's Active Response and the internal Windows firewall.

This guide will show how to use WLR Isolation via the Wazuh API and DevTools.

## How WLR Isolation Works
When a security incident is detected, analysts can send an "isolate" command to an endpoint through the Wazuh API. The endpoint then applies firewall rules to block all network communication except for the Wazuh server and any other necessary hosts.

A "release" command can later be sent to remove the isolation and restore normal connectivity.

> For now, it is only available for Windows endpoints.
{: .prompt-info }
