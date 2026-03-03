# expert-rule
## Steps
- Create your own branch to add the rules
- Add "new pull request" to ask threat expert to review the rules
  - Reviewer - Threat Expert DRI
  - Label - Beta or Stable
  	- NOTE: Beta means rule is deployed only in STG site, Stable means rule is deployed both in STG and PROD site
	
## Writing Conventions
- filename (NEW)
  - Filename should not contain technique ID, engine name, malware name, and hacking tool name
  - If model's logic is about detecting a hacking tool or events from a hacktool, the hacktool should only be mentioned in the model's title/description and not in the filename.
  - Example:
    - Model Title: Download Invoke-Mimikatz.ps1 Obfuscated By Appending It To A JPG File To Memory
      File Name: Download HackTool Obfuscated by Appending It To A JPG File To Memory.json
    - Model Title: BM Policy 4768T - gsecdump
      File Name: bm policy 4768T.json
  - If there's a duplicate filename due to generic "hacktool" word, append the model id in the end of the filename
  - Example:
    - Credential Dumping via HackTool (817e5824-9c6a-420d-b74c-d339118b10bc).json
- schemaVersion
 	- Version of the JSON schema. Current default value is "1.0"
- id
	- A unique uuid for each rule
- name
	- Pure title, no technique ID or engine name on it. Please expect customer might see it
- description
	- Pure description. Please expect customer might see it
- filterSets
	- id
		- Unique uuid of the filter to be matched
	- name
		- Name of the filter to be matched
	- highlight
		- True if the filter has an highlighted object, False if none
	- maxAmount
		- (optional field) Number of Filters will appear in WorkBench. Default is 10
		
	- example for OR operation
	```
	"filterSets": [
	[
		{
			"id": "5369148b-f194-43db-a91b-8eb0d5b7a855",
        		"name": "(T1003) Credential Dumping via Mimikatz",
        		"highlight": true
      		}
    	],
    	[
      		{
        		"id": "58214cc7-4f58-4869-ae2a-eea652c26392",
        		"name": "(T1003) Credential Dumping via Process",
        		"highlight": true
      		}
    	]
  	],
	```
	- example for AND operation
	```
	"filterSets": [
    	[
      		{
        		"id": "857b6396-da29-44a8-bc11-25298e646795",
        		"name": "(T1088) Bypass UAC via shell open registry",
        		"highlight": true
      		},
      		{
        		"id": "ac200e74-8309-463e-ad6b-a4c16a3a377f",
        		"name": "Bypass UAC Via Shell Open Default Registry",
        		"highlight": true
      		}
    	]
  	],
	```	
	**NOTE:** Info level filters are not used in model matching. Only low to critical level filters can be included in the filterSets.
- threshold
	- targetOccurrence
		- Number of times this rule needs to be matched
		- No maximum value. The higher, the more diffcult to trigger rule.
		- If targetOccurrence is more than 10. we suggested to use maxAmount to make sure your filter shows on the Workbench.
	- withinSeconds
		- Time in seconds on how long the rule will wait since the first filter was hit
	- dedupSeconds
		- Time in seconds on how long the rule will wait to deduplicate the occurrence of the filter hit
		- But **this is not being used in SAE rule** design, instead, one event of the same filter per minute is recorded. So even if the events were triggered 10 times within 1 minute and the targetOccurrence is 10, it will still be counted as 1 occurrence **since all events happened within 1 minute**.
	        - if my filterSet has 1 filter and the filter is trigged 100 times in 1 minute, occurrence is still counted as 1
			- if my filterSet has 1 filter and the filter is trigged 10000 times in 1 minute, occurrence is still counted as 1
			- if my filterSet has 4 filters(Filter1 AND Filter2 AND Filter3 AND Filter4) and all of the filters are hit once in the same 1 minute, occurrence is still counted as 1 (not 4!).
			    - Example
				   ```
				    "filterSet": [[
				       {Filter 1},
					   {Filter 2},
					   {Filter 3},
					   {Filter 4}
				    ]]
				   ```
			- if my filterSet has 3 filters(Filter1 OR Filter2 OR Filter3) and 2 of the filters are hit once in the same 1 minute, occurrence is still counted as 1 (not 2!).
			    - Example
				   ```
				    "filterSet": [
						[
				           {Filter 1}
						],
						[
				           {Filter 2}
						],
						[
				           {Filter 3}
						],
				    ]
				   ```
- requiredProducts
	- **Please notice that the logic here is different from filterSets**
	- This field is a 2D array. 
	- The products in inner arrays are by **OR** operation, between inner arrays are by **AND** operation. 
	- In the example below, it means ("Apex One" OR "Trend Micro Deep Security").
	- please refer to https://wiki.jarvis.trendmicro.com/display/SG/IdP+Auth+Service+Product+ID for the exact product code to use.
	- commonly used product code are as follows:

| | product Code|
|-- | --|
|Apex One SaaS | sao|
|DDI | pdi|
|Deep Security SaaS | sds|
|Cloud App Security | sca|
|XDR Endpoint Sensor| xes|
|Deep Security On-Prem| pds|

- author
	- Name of rule creator
- team
	- Team name of the author
- date
	- Date when the rule was created
- lastUpdate
	- Date and time when the rule was updated

```
{
  "schemaVersion": "1.0",
  "id": "25d96e5d-cb69-4935-ae27-43cc0cdca1cc",
  "name": "(T1088) Bypass UAC via shell open registry",
  "description": "Detect adversary tries to escalate the privilege by modifying registry",
  "filterSets": [
    [
      {
        "id": "857b6396-da29-44a8-bc11-25298e646795",
        "name": "(T1088) Bypass UAC via shell open registry",
        "highlight": true
      },
      {
        "id": "ac200e74-8309-463e-ad6b-a4c16a3a377f",
        "name": "Bypass UAC Via Shell Open Default Registry",
        "highlight": true
      }
    ]
  ],
  "threshold": {
    "targetOccurrence": 1,
    "withinSeconds": 30,
    "dedupSeconds": 30
  },
  "requiredProducts": [
    [
      "sao",
      "sds"
    ]
  ],
  "author": "Ted Lee"
  "team": "SAE"
  "date": "2019/09/21"
  "lastUpdate": "2020-02-11T17:24:37.000Z"
}
```
