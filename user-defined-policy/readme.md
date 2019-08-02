## User Defined Policy in APIC V2018 for gateway type datapower-api-gateway
User-defined policies provide (UDP) custom processing control of APIs in the Gateway server. UDP in API Connect 2018 is available from Datapower Gateway firmware version 4.1.6.

Steps to create User Defined Policy in API Connect 2018
1.	Create an Assembly Function which acts as the policy descriptor.  Assembly Function is used to specify user-defined policies that API Connect advertises and makes available in the API Connect assembly editor.
2.	Create an Assembly object. It specifies the assembly to apply to calls to the assembly function. An assembly comprises a rule that defines the actions to run against the call and how to handle errors during assembly execution.
3.	Create an API Rule. It specifies API rule that comprises only assembly actions to apply to the API call. An API rule completes the processing of API requests or completes the operations that are required by the API requests.
4.	API Action Associates the processing actions to the API rule. When multiple actions are associated, the API Gateway runs the actions in the top-to-bottom order. E.g. Assembly XSLT Action - use the XSLT action to run a stylesheet from within your assembly.

The dependency tree – Assembly Function --> Assembly --> API Rule --> API Action

To export a newly minted policy - From the Control Panel, click on Export configuration then select Export configuration and files from the current domain, then click Next
Give the export a name {see (1)} then select Assembly Function under Objects {see (2) and (3) and select the appropriate assembly functions then press the ">" arrow {see (4)}, then click Next {see (5)} to be directed to a download screen. More than one policy may exist in a single zip.

![Alt text](/images/export-udp.jpg)
 
After importing the policy, you will need to add the policy to all gateway instances before it will appear in the policy palette. This is done in the API Connect Gateway Service object.

## Comparison and analogy of UDP between V5 & V2018
1.	Assembly Function replaces what was formerly (in V5) called a policy descriptor or policy definition.
2.	The Assembly and API Rule replace what was formerly the main processing rule in V5.
3.	The Assembly Action replaces the V5 MPGW Processing Action.

The snapshot below shows the folder structure in the user-defined policy zip file
 
![Alt text](/images/udp-comparison.jpg)