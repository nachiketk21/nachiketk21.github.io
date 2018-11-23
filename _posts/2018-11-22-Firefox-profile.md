---
layout: post
title: Setting new profiles to FIREFOX
author: Nachiket
---

During automation for a website we had to set a firebug and netexport profile to every new webdriver instance in firefox . We were using selenium webdriver with Ruby . There is a solution using java . We did it for ruby in following way

```Ruby
    profile = Selenium::WebDriver::Firefox::Profile.new
    profile.add_extension('./resources/firebug@software.joehewitt.com.xpi')
    profile.add_extension('./resources/netexport@getfirebug.com.xpi')

    profile['app.update.enabled'] = false
    # Setting Firebug preferences
    profile['extensions.firebug.currentVersion'] = '1.12.5'
    profile['extensions.firebug.addonBarOpened'] = true
    profile['extensions.firebug.console.enableSites'] = true
    profile['extensions.firebug.script.einableSites'] = true
    profile['extensions.firebug.net.enableSites'] = true
    profile['extensions.firebug.previousPlacement'] = 1
    profile['extensions.firebug.allPagesActivation'] = 'on'
    profile['extensions.firebug.onByDefault'] = true
    profile['extensions.firebug.defaultPanelName'] = 'net'
    # Setting netExport preferences
    profile['extensions.firebug.netexport.alwaysEnableAutoExport'] = true
    profile['extensions.firebug.netexport.autoExportToFile'] = true
    profile['extensions.firebug.netexport.Automation'] = true
    profile['extensions.firebug.netexport.showPreview'] = false
    path = File.join(File.join(Dir.pwd), '@file_name')
    profile['extensions.firebug.netexport.defaultLogDir'] = path.gsub('/', '\\')
    profile['update_preferences'] = true
    $driver = Selenium::WebDriver.for(:firefox, profile: profile)
```

Create a resources folder at the root with following files in it which will install firebug and netexport from that folder.

**Please find two files (You can google it.)**

``` firebug@software.joehewitt.com.xpi ```

``` netexport@getfirebug.com.xpi ```
