---
layout: post
title: AEM - How to disable/enable workflow launcher bundle
date: 2015-11-13 00:00:00
---

If you need to disable the AEM workflow launchers and then enable them again after your whatever work is complete, here are the steps.

## Disable
- Open the AEM system console component page,

```
http://localhost:4502/system/console/components
```

- Then look for the items bellow and click the stop/disable button on the right.

```
com.adobe.granite.workflow.core.launcher.WorkflowLauncherImpl
com.adobe.granite.workflow.core.launcher.WorkflowLauncherListener
```


## Enable
- Open the bundles page.

```
http://localhost:4502/system/console/bundles
```

- Look for **Adobe Granite Workflow Corecom.adobe.granite.workflow.core** and expand it by clicking on the small arrow icon.
The components we have disabled in the last step should be listed here.

- Stop and Start it again by clicking on the stop/play icon on the right side.