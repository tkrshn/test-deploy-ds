App used in conjunction with 100_<customer>_splunkcloud forwarding app and org_all_forwarder_outputs base configs app.

App Name:	000_all_forwarder_outputs_route_onprem_and_cloud  

Created by:	Hywel Matthews - hmatthews@splunk.com



OVERVIEW
############
This app is to be used in conjunction with a customers 100_<customer>_splunkcloud forwarding app and org_all_forwarder_outputs base configs app, to dual forward to the customers current target servers, and their Splunk Cloud stack.

By creating two output groups, they will operate independently of each other; If one output group is unavailable, the other will continue to forward, unlike _TCP_ROUTING which will case the other to block.


A customers 100_<customer>_splunkcloud forwarding app will take ASCII precedence, therefore this app is named 000_all_forwarder_outputs_route_onprem_and_cloud to take  ASCII precedence over a customers 100_<customer>_splunkcloud forwarding app.


A customers 100_<customer>_splunkcloud forwarding app includes default/outputs.conf:

	[tcpout]
	defaultGroup = splunkcloud


This app 000_all_forwarder_outputs_route_onprem_and_cloud appends the tcpout group name from the org_all_forwarder_outputs base configs app, to create the second output group:


	[tcpout]
	defaultGroup = splunkcloud,primary_indexers


INSTRUCTIONS
############
1) Check org_all_forwarder_outputs is deployed

2) Download 000_all_forwarder_outputs_route_onprem_and_cloud

3) If the base configs app org_all_forwarder_outputs base configs app is not used
   a) Define the target current servers as below:

	#[tcpout:primary_indexers]
	#server = <server1>:9997, <server2>:9997,<server3>:9997,<server4>:9997

   OR
 
   b) Download the base app org_all_forwarder_outputs, update the server list
4) Deploy org_all_forwarder_outputs, remove the legacy outputs configuration

5) Deploy 000_all_forwarder_outputs_route_onprem_and_cloud and 100_<customer>_splunkcloud to the forwarders required to dual forward to the current targets and Splunk Cloud indexers

6) Must implementing blocking, if either on-premise or Splunk Cloud are unavailable, to ensure events are not dropped to either groups:
	dropClonedEventsOnQueueFull = -1




NOTE
PII (personally identifiable information)
Please note: data forwarded to Splunk with this add-on might include personally identifiable information. You are responsible for complying with applicable laws	
