---
title: Isolation | Wazuh Live Response
date: 2025-03-12 06:00:00 -0600
categories: [BlueOps, Response]
tags: [difficulty:medium, siem:wazuh]
image:
  path: https://github.com/user-attachments/assets/e2e31225-45e8-4583-bf71-d8ae540a88fc
---

In security incident response, the ability to quickly isolate a compromised endpoint is crucial. [C-LR Isolation](https://github.com/lr2t9iz/C-LiveResponse/blob/main/docs/endpoint/C-LR%20Isolatation.md), a submodule of Wazuh Live Response, allows security teams to remotely isolate a Windows endpoint using Wazuh's Active Response and the internal Windows firewall.

This guide will show how to use C-LR Isolation via the Wazuh API and DevTools.

## How C-LR Isolation Works
When a security incident is detected, analysts can send an "isolate" command to an endpoint through the Wazuh API. The endpoint then applies firewall rules to block all network communication except for the Wazuh server and any other necessary hosts.

A "release" command can later be sent to remove the isolation and restore normal connectivity.

> For now, it is only available for Windows endpoints.
{: .prompt-info }

## Usage
- <https://github.com/lr2t9iz/C-LiveResponse/blob/main/docs/endpoint/C-LR%20Isolatation.md>

<br>

- Copy the [isolation.exe](https://github.com/lr2t9iz/wazuh-live-response/tree/main/endpoint/windows/bin) executable to the Active Response folder on the Windows agent `C:\Program Files (x86)\ossec-agent\active-response\bin\`
- We can deploy a [lab environment](https://c-137labs.mitzep.com/posts/wazuh-s1em/) by following these instructions to conduct testing.

### Isolating a Host
- ≡ > Server management > Dev Tools > Console
- Change the agent ID and the Wazuh IPs accordingly.
```json
PUT /active-response?agents_list=001
{
  "command": "!isolation.exe",
  "arguments": ["192.168.1.1", "192.168.1.2"],
  "alert": { 
    "data": { 
      "action": "isolate",
      "user": "c-137labs",
      "debug": false
    }
  }
}
```
- To observe the result, we can run an extended ping and monitor how the connection is lost.
- We can also find the audit event by applying the filter `data.origin.name:"C-LR"` in the Discover section of the Wazuh Dashboard.
- ≡ > Explorer > Discover
![Image](https://github.com/user-attachments/assets/6bc1d43e-ac5c-4dee-908e-d722a036df4c)

### Releasing a Host from Isolation
To remove the isolation and restore normal connectivity
```json
PUT /active-response?agents_list=001
{
  "command": "!isolation.exe",
  "alert": { 
    "data": { 
      "action": "release",
      "user": "c-137labs",
      "debug": false
    }
  }
}
```
- "release" → Removes the firewall rules and restores communication.
![Image](https://github.com/user-attachments/assets/1638a56a-74a4-41ec-9488-200e85632768)

For more details, visit the official [C-LR Isolation Docs](https://github.com/lr2t9iz/C-LiveResponse/blob/main/docs/endpoint/C-LR%20Isolatation.md)
