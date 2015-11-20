---
layout: post
title: Open Fire IM Gateway in run
date: 2009-01-18 00:00:00
---


Open Fire is one of the most popular XMPP based chat server. Few days back, I have worked with its IM Gateway plugin, which provides gateway connectivity to the other public instant messaging networks.

To access the XMPP services it has Smack API for Java.

For using this add the plugin from Openfire admin or paste the gateway.jar file in its installation plugins directory. After restarting the Openfire service, you will find it in the admin site with name Gateway. Select the messengers you want to use for your application.

First, you have to create an XMPP connection to your code. To register a gateway (e.g., Yahoo, GTalk, etc.) you have to write a method like:

```java
public boolean registerAGateway(XMPPConnection xmpp, String network, String username, String password) {
	boolean success = true;
	network = network.toLowerCase();
	try {
		Registration registration = new Registration();
		registration.setType(IQ.Type.SET);
		Map attributes = new HashMap();
		attributes.put(new String("username"), new String(username));
		attributes.put(new String("password"), new String(password));
		registration.setAttributes(attributes);
		registration.setTo(network + "." + XMPP_HOST);
		registration.setFrom(xmpp.getUser());
		PacketCollector collector = xmpp.createPacketCollector(new PacketIDFilter(registration.getPacketID()));
		xmpp.sendPacket(registration);
		IQ response = (IQ) collector.nextResult(10000);
		collector.cancel();
		if (response == null) {
			success = false;
		} else if (response.getType() == IQ.Type.ERROR) {
			success = false;
		}
	} catch (Exception e) {
		success = false;
	}
	return success;
}
```

Next method is for unregistering the gateway:

```java
public boolean unregisterGateway(XMPPConnection xmpp, String network) {
	boolean success = true;
	network = network.toLowerCase();
	try {
		Registration registration = new Registration();
		registration.setType(IQ.Type.SET);
		registration.setTo(network + "." + XMPP_HOST);
		registration.setFrom(xmpp.getUser());
		Map map = new HashMap();
		map.put("remove", "");
		registration.setAttributes(map);
		PacketCollector collector = xmpp.createPacketCollector(new PacketIDFilter(registration.getPacketID()));
		xmpp.sendPacket(registration);
		IQ response = (IQ) collector.nextResult(10000);
		collector.cancel();
		if (response == null) {
			success = false;
		}
		if (response.getType() == IQ.Type.ERROR) {
			success = false;
		}
	} catch (Exception e) {
		success = false;
		e.printStackTrace();
	}
	return success;
}
```

If you want to get the gateway names for a user:

```java
public ArrayList getRegisteredGateway(XMPPConnection xmpp) {
	ArrayList networks = new ArrayList();
	try {
		ServiceDiscoveryManager sdm = ServiceDiscoveryManager.getInstanceFor(xmpp);
		DiscoverItems items = sdm.discoverItems(XMPP_HOST);
		Iterator itemsIterator = items.getItems();
		while (itemsIterator.hasNext()) {
			DiscoverItems.Item item = (Item) itemsIterator.next();
			if (sdm.discoverInfo(item.getEntityID(), item.getNode()).containsFeature("jabber:iq:registered")) {
				networks.add(item.getName());
			}
		}
	} catch (XMPPException ex) {
		ex.printStackTrace();
	}
	return networks;
}
```

For getting the roster list and their mode, status:

```java
public ArrayList getGatewayBuddies(XMPPConnection xmpp, String network) {
	ArrayList buddyList = new ArrayList();
	String jid = "", name = "", status = "", type = "", mode = "", buddyInfo = "";
	Roster roster = xmpp.getRoster();
	network = network.toLowerCase();
	Collection entries = roster.getEntries();
	for (RosterEntry entry: entries) {
		jid = entry.getUser();
		if (jid.contains("@" + network)) {
			name = entry.getName();
			Presence pr = roster.getPresenceResource(jid);
			status = pr.getStatus(); // yahoo, gtalk
			type = pr.getType().toString(); //yahoo, gtalk
			mode = String.valueOf(pr.getMode()); //yahoo, gtalk (dnd, away, null->presence)
			buddyInfo = jid + "|" + type + "|" + name + "|" + status + "|" + mode;
		}
		buddyList.add(buddyInfo);
	}
	return buddyList;
}
```

It’s also possible to get the avatar images in binary mode for Jabber messages.
That’s all.