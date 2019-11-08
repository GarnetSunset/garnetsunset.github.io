## Getting recon information without ever stepping foot in the building.

Understanding the Service
------

Forcepoint's Websense Endpoint web proxy service installs itself, with a Root CA, and configures a proxy via Windows Administration. 

![alt text](https://github.com/GarnetSunset/garnetsunset.github.io/blob/master/images/proxywindows.png?raw=true "Windows Proxy Settings")

Of course, different companies need different configurations. This leads into our next topic.

The PAC File
------

Since each company needs it's own configuration file, and the ability to update it, the obvious solution is to 
host it online for endpoints to reach out to for updates.

### What's a PAC file?

A pac file is the configuration file, hosted on Forcepoint's proxy url, namely,
`pac.webdefence.global.blackspider.com/proxy.pac?p=`

Everything after "p=" is the ID of the pac file you're requesting. e.g `trhsv69m`, which will be relevant later.

![alt text](https://github.com/GarnetSunset/garnetsunset.github.io/blob/master/images/proxyconnected.png?raw=true "Forcepoint Proxy Settings")

### What's in a PAC file?

PAC Files are Javascript files full of various if statements. 

### Why is this important?

When configuring your network for Forcepoint, you'll want to whitelist certain items, including banking 
info, or even **internal URLs** that might break when funneled through a proxy, or might be too sensitive to decrypt over the wire. 

## What's the problem then?

With internal URLs, you have an idea of the internal layout of a network, as well as any sites 
that could be used for exfilration of data. (Google Drive for example) The 10 or so PAC files I've 
gone through allowed for me to understand which company they were for, and extenal webVPN urls. Including one company who exposed their
theoretically unpatched Citrix webVPN portal through a pac file. 

## But these urls are randomized so who cares?

This is where things get interesting. After comparing the URLs of various PAC files, I figured out a pattern that I turned into a regular
expression. 

``^(([b-df-hj-np-tv-z2346789]{8})\2?(?!\2))+$``

To break it down: Consanants only, no 0's, 1's, or 5's, characters can only consecutively repeat twice, each string is also exactly 8 characters in length. 

Those are **pretty** concise parameters to easily obtain this kind of info.

Cons to this technique: Completely random who you might get.

I've included a script with this blogpost if you want to test this out for yourself. 

The link to the POC [here.](https://gist.github.com/GarnetSunset/f22be96afd0328b01a77b131707ebe5c)
