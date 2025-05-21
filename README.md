# Malicious Traffic Entering the Network
In this project we're going to see if there were any successful malicious traffic entering our network.

> [!note]
> To better understand this section, consider reviewing the [Threat Maps Creation (Deep Dive)](https://github.com/LCJones73/Threat-Maps-Creating-Deep-Dive) first.<BR>

The following code will generate our Threat Map:

![image](https://github.com/user-attachments/assets/477740b7-be6c-4098-a873-7a3feedaa7f8)

This the Threat Map it generates:

![image](https://github.com/user-attachments/assets/31b87086-660d-49b0-8d98-b9ce5057311c)

As you can see there are the colored circles. The red indicates that it's highly likely something is compromised and has entered our network.

Now let's get the KQL code (below) and use the Microsoft Sentinel Logs to examine it:

![image](https://github.com/user-attachments/assets/b783931f-175e-4e99-b1a9-cec1fc3fd9b4)

Looking at this code we can break some of it down so that it makes sense as to what it is searching for.

Line 3 is where we see our "FlowType" is looking for "MaliciousFlow" that is logged.

Next we see the section of code called "MaliciousFlows" where we are indicating the types of data we need to determine this. You will notice the IPV4 again for our Geo Location data.

Examining it a bit further you see the "Project" on line 9 which is requesting specific data from the logs, which generates the following information:

![image](https://github.com/user-attachments/assets/9aea2dad-13a1-4452-9051-371aa6b78795)

> [!IMPORTANT]
> Let's Break This KQL Code Down:<BR><BR>
> To start, since we've been creating these Threat Maps, we've been using the "Law-Cyber-Range" which is configured as an Network Service Group (NSG). This is how everything is logged for us to request the data we need to see whatever we're looking for. In this case for this project we are searching for MaliciousFlow entering our network.
>
> If you wanted you could scan a specific IP to see if there is simliar activity happening. You'll notice on Line 4 we have this code: //| where SrcIP_s == "10.0.0.5"<BR><BR>
> The "//" at the beginning takes all data after for that line and turns it into a "Comment". This means when you run the query, that line will not be used.<BR><BR>
> If you remove the "//" and change or use a similiar IPAddress it will scan only that IP "10.0.0.5". Ours is commented out so it scans the NSG we're using.<BR>
>
> ### Now, let’s take a closer look at why this information matters and how it contributes to our analysis:
>
> Shows Key Metadata for Each Flow
> The final output includes:
> 
> - TimeGenerated:	Shows when the threat happened
> - IpAddress:	The source of the malicious flow
> - DestinationIpAddress:	Target of the traffic
> - DestinationPort:	Helps infer service type (e.g., 22 → SSH)
> - Protocol:	Indicates the layer 7 protocol (e.g., HTTP, RDP)
> - NSGRuleMatched:	What network security rule allowed or blocked it
> 
> This helps analysts triage and pivot—e.g., was this RDP traffic from China allowed by a misconfigured NSG?

This is a great starting point for Threat Hunting, for both blue and red teams, like Capture The Flag Cybersecurity events for example.
 - Analysts can enrich incidents with this data and assess severity
 - Pentesters can use it to simulate attack traffic and verify if it appears in this table (Detection Validation).

## Final Thoughts
This query is a powerful enrichment + detection tool. It’s especially useful in:
- Threat detection
- Attack surface analysis
- Incident response
- NSG configuration auditing

Keep in mind there are other ways to sharpen your KQL skills, if this is something you want to do. One example is the following: [KC7 Cyber](https://kc7cyber.com/)

This is a great tool you can use that starts at beginner levels, and moves to harder scenarios. I definitely suggest doing their "_KQL 101_", as it will set additional foundational knowledge.
