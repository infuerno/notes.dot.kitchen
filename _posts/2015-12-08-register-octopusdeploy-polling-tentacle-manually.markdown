---
layout: post
title: Register Octopus polling tentacle 'manually'
date: '2015-12-08 23:20:38'
---

Octopus tentacles can be configured in listening mode (recommended) or polling mode. 

## Firewall challenges
In **listening mode** with tentacles on remote machines, firewalls need to be configured to allow inbound communication on a port. The default port is 10933, but it can be set to anything (e.g. 443, 80, 8080 etc).

In **polling mode** firewalls may not need to be changed if outbound traffic is allowed on all ports. However, in corporate networks sometimes only ports 80 and 443 are allowed. Octopus tentacles by default require communication on at least two ports at initial installation stage:

1. the octopus web portal to complete a handshake
2. the actual octopus server comms port

These are both configurable. Can we use port 80 for one and port 443 for the other? Port 80 is traditionally associated with HTTP not HTTPS. And although you could set up the web port on port 80 with HTTP this would leave credentials rattling about unencrypted - not ideal. Setting port 80 with HTTPS didn't appear to work for the octopus portal at least (and is confusing at best).

## Proposal
Do in the initial handshake 'manually' and only require the server comms port to be open (and use port 443 for this - which requires the octopus web portal to run on some other port e.g. 8443).

## Solution
1. Register a new machine with Octopus using the REST api
 - Endpoint: https://octopus-server:port/api/machines
 - Headers: `X-Octopus-ApiKey: API-JRIXL3O2GQBKWJE2FI1TKRFTGU`
 - Payload: `{
  "EnvironmentIds": ["Environments-1"],
  "Endpoint": {
  "CommunicationStyle": "TentacleActive",
  "Thumbprint": "F9E49532BAC8772FE6B6A24D0A7671298EB015D4",
  "Uri":"poll://6n7xkg92t8r3szf3zfk2/"
  },
  "Name":"OCTOPUS",
  "Roles": ["octopus-server"],
 "Thumbprint":"F9E49532BAC8772FE6B6A24D0A7671298EB015D4"
}`
2. **Note 1** the "poll" uri in the payload above is required but it appears that any unique string will work. **Note 2** get a list of all environments using https://octopus-server:port/api/environments 
3. The tentacle should now be registered on the server (though not yet connectable)
4. Install the Tentacle
5. Run the following instance creation / configurations manually (replacing the trust certificate thumbprint with the one from the server):
 - `Tentacle.exe create-instance --instance "Tentacle" --config "D:\Octopus\Tentacle.config"`
 - `Tentacle.exe new-certificate --instance "Tentacle" --if-blank`
 - `Tentacle.exe configure --instance "Tentacle" --reset-trust`
 - `Tentacle.exe configure --instance "Tentacle" --home "D:\Octopus" --app "D:\Octopus\Applications" --noListen "True"`
 - `Tentacle.exe configure --instance "Tentacle" --trust=233E6B9580D15A9E90D23424A644025722A89A44`
 - `Tentacle.exe server-comms --style=TentacleActive --host=octopus-server --port=443`

6. Browse to the instance directory and update the subscriptionId from null to the same poll uri used above
7. Start the tentacle
8. Run a "check health" to confirm registered successfully and all working








