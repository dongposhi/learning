


- MCM 是一套很棒的发布机制。它的目的不是为了审计。而是一种以回答问题的方式帮助发布者以审慎的态度全面多角度的思考发布内容，提高发布成功率，减少异常的机制。
- MCM的一个模板：
	- Title
	- OverView
		- Timing
			- scheduled start/end (Timezone should be clarify)- 
	- Description
		- Outage / Activity Details
			- What's the purpose of the activity?
			- What will be required to execute the change?
			- What's the expected end state of the system after the change?
			- What's assumptions, if any, are being made about the state of the system at the time of the change?
		- Impact / Risk assessment
			- Why is it necessary? What is the impact of not making the change?
			- Why is this the correct time to complete the CM?
			- Are there any related, prerequisite changes upon which this CM hinges?
			- Will the CM be in any way instrusive, and if so, how will you konw? What teams, services or functionality will be impacted?
			- Is the change has critical impact on online business? 
			- How has the change being tested to verify it's safe for production?
		- Affected services
			- Does the change involve other dependencies? Are they aware of the CM and on the cc-list or approval list?
			- Provide links to your dependencies dashboard
		- Worst case scenarios
			- What could happen if everything goes wrong with the change?
			- How does the CM attempt to mitigate the risk?
		- Rollback Procedure
			- What conditions would indicate a need to rollback?
			- In the event of problems, what will you do to return your system to a known good state?
		- If this is a software or infrastucture change, has the rollback procedure been verified in a development environment?
- MCM review 的角度
	- Layer 1 review
		- When, where, how( is CD or not?) to execute the CM, Do they make sense 
		- Do the change has CR, adequately test, proved rollback preparation?
		- Does the CM been send to right person to review?
		- Check whether to CM involve cross region, security issue.
	- Layer 2 review
		- Does the layer 1 review completed with good quality?
		- Metrics 
		-  All Scenarios check
		- Steps check
		- Rollback check 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUwOTEzMzU4Nl19
-->