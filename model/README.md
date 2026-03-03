# expert-model
## Steps
- Create your own branch to add the models
- Add "new pull request" to ask threat expert to review the models
  - Reviewer - Threat Expert DRI
  - Label - Beta or Stable
  	- NOTE: Beta means model is deployed only in STG site, Stable means model is deployed both in STG and PROD site
- Limit the changes in PR to at most 10 file changes. This is to prevent reviewers to be overwhelmed and confused. If changes are needed, it is recommended to be split.
- For Beta models, submitter must submit the migration PR separately. If there are urgent requests, you can inform SAE Threat Expert to assess the request.
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
	- A unique uuid for each model. Submitter must provide the unique UUID if the submission is new and it should be of UUID Version 4 format.
- name
	- Since customers can see the tile, please use pure title, no technique ID or engine name on it.
	- No Malware/Tool/Group name. Refer to the list: https://attack.mitre.org/software/ https://attack.mitre.org/groups/
	- You may refer here for filter name guidelines: https://wiki.jarvis.trendmicro.com/x/bGSJHg
- description
	- Pure description and as descriptive as possible. Customers can see it, thus, it will be helpful if it is descriptive.
	- You may refer here for description guidelines: https://wiki.jarvis.trendmicro.com/x/_CgiPw
- riskLevel
	- Severity. Value can be low, medium, high, critical. Please use all lowercase.
- scope
	- Value can be "user" for a single user, and "endpoint" for a single endpoint
- defaultEnabled
	- Default model setting once delivered to customers. Just add if the value is false.
	- If ever modifying this field(true to false, false to true), you will need to renew the Model Id to refresh the states; otherwise the modification won't take effects.
- enable (deprecated)
	- Replaced by "defaultEnabled"
- rules
	- id
		- Unique uuid of the rule to be matched
	- name
		- Name of the rule to be matched
	- score
		- Score of the rule once matched
- threshold
	- targetScore
		- Points needed for the model to be triggered
	- withinSeconds
		- Time in seconds on how long the model will wait since the first rule was hit
	- dedupSeconds
		- Time in seconds on how long the model will wait to deduplicate the occurrence of the rule hit
- requiredProducts
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

- tags
	- MITRE ID of the techniques in this model
- playbooks
	- Choose the appropriate one:
		- For Models which are using **DETECTION based filters**, kindly use the following playbook id:
			- id: **a4c36882-115b-4296-a5e3-051fc303e0ca**
			- name: **Default Playbook**
			- provider: **WORKBENCH**
			- type: **DETECTION**
		- For Models which are using **TELEMETRY based filters**, kindly use the following playbook id:
			- id: **bd6c9ef1-965d-4204-b9f2-d39003708f18**
			- name: **Default Playbook**
			- provider: **WORKBENCH**
			- type: **DEFAULT**
	- Include playbook for AIR:
		- id: **f87e38bc-b0a9-4997-8b6a-d0762a7bcace**
		- name: **Generic file/ip/url/email investigation**
		- provider: **AIR**
		- type: **AUTOMATED_RESPONSE**
	- ref: https://wiki.jarvis.trendmicro.com/display/XSAE/Workbench+Playbook+Ids
		
- visibleLevel
	- Level of visibility 
	- Range

		|     User  	|     Range 	|
		| ------------- | ------------- |
		| Customer	|      1-10	|
		| MSP		|     11-20	|
		| MDR		|     21-30	|
		| XDR		|     51-98	|
		| Heartbeat	|         9	|

	- During model testing, set the visibleLevel to 51 and silent to true. After the testing period and the results are expected, set the visibleLevel to Customer (1-10) and silent to false.
	- More information: https://wiki.jarvis.trendmicro.com/x/JpMtIw
- globalModel
	- Indicate whether the model is a global model released and maintaned by Trend Micro
- silent
	- Default value is true. A silent model will be loaded, processed, and logged as usual except that its alerts will not be sent to WorkBench
- author
	- Name of model creator
- team
	- Team name of the author
- date
	- Date when the model was created
- lastUpdate
	- Date and time when the model was updated. The datetime format is "RFC 3339"

```
{
  "schemaVersion": "1.0",
  "id": "b9debb23-fb53-43a0-b01b-00ce8d6f53f7",
  "name": "Disabling Security Tools for Process Injection",
  "description": "Detects Suspicious Disabling of Security Tools for Process Injection",
  "riskLevel": "medium",
  "scope": "endpoint",
  "enable": true,
  "rules": [
    {
      "id": "683ad23b-832c-4377-8651-8cc10346f775",
      "name": "Security Tools Disabled",
      "score": 5
    },
    {
      "id": "c2bd634d-12b3-4e06-905d-4edd231bf5bd",
      "name": "Process Injection",
      "score": 5
    }
  ],
  "threshold": {
    "targetScore": 10,
    "withinSeconds": 300,
    "dedupSeconds": 300
  },
  "requiredProducts": [
    [
      "sao",
      "sds"
    ]
  ],
  "tags": [
    "MITRE.T1089",
    "MITRE.T1055"
  ],
  "playbooks": [
    {
      "id": "bd6c9ef1-965d-4204-b9f2-d39003708f18",
      "name": "Default Playbook",
      "provider": "WORKBENCH",
      "type": "DEFAULT"
    },
    {
      "id": "f87e38bc-b0a9-4997-8b6a-d0762a7bcace",
      "name": "Generic file/ip/url/email investigation",
      "provider": "AIR",
      "type": "AUTOMATED_RESPONSE"
    }
  ],
  "visibleLevel": 61,
  "globalModel": true,
  "silent": true,
  "author": "Johnlery Triunfante"
  "team": "SAE"
  "date": "2019/02/10"
  "lastUpdate": "2020-02-10T02:46:00.000Z"
}
```
