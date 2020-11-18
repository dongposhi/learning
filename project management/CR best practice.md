# Code Review practices

## Overall

Insist high standard always the top bar and we must follow. Right now, we are under "Agile Dev" and we should not let the "CR Load" spend too much SDE time and slow down the project progress. Below document is team level CR guideline, will be used for new SDE ramp up and making decision for CR discussion.

## Goal

* Any change(not only code change, but configuration changes as well) which is going to impact production(in general, every code changes should be reviewed). 
* Ideally, 100% code needs to be reviewed every CR.
    * [Q]Integration test , do we need to review each line change?
* As many as we could to review via tool, as less as we should review by manual.
* Infrastructure as code and each infrastructure change need to be reviewed.

## Best practice

We will try below practice to help our team improve code quality and fast the review progress.

### As a Publisher:*

* When planning work, book time (efforts, duration) for CR
* New/Edit “interface” should be in a separated CR
    * Coral model 
    * Lambda handler
    * Data structure
* Respect the reviewer’s time
    * Size of the code review doesn’t go into multiple pages. Try not to.
        * Try best to narrow down your change to one page.
    * Self code reviewing to catch obvious gotchas before sending it to the reviewer.
    * Respond with all the comments in a single iteration that includes all functional/maintainability/testing comments and nitpicks for both source code and unit tests.
* Follow team level code style with some tools help. 
    * 
    * CheckStyle — Link (https://code.amazon.com/packages/ISSTCommonBuildLogic/trees/mainline) 
        * [Sample (https://code.amazon.com/packages/MerchantExportPreferenceAppTierService/commits/a3d63ff32661a83caadbb6d2bfb587376301a4fb#)]This tool using the default check style tool and you can apply it via involve ISSTCommonBuildLogic.
        * CheckStyle will auto applied when running brazil build 
    * JSCodeStyle — Link (https://code.amazon.com/packages/ISSTJSCodeStyle/trees/mainline)
        * [Sample (https://code.amazon.com/packages/SCLensSellerAccessManagementServiceCDK/commits/9218163f23a6ed4010992d37d0567043d85b0f57#)]This tool using ESLint and Prettier to help us format the code style.
        * Style check will auto applied when running Brazil build
            * You may need to run “npm run fix” to manual fix the style problem.
    * CRUX Rules — Sample (https://code.amazon.com/gc/rules/for-package/SCLensSellerAccessManagementService)
        * You may need to setup these rules by manual for now until we figure out a way to create team level rules
* Add sufficient description. Most CRs should be understandable without referring to a project design document. If multiple revisions are published, describe each revision or the revision which has big changes. Follow the team template (https://code.amazon.com/packages/ISSTMarkdownTemplates/trees/mainline) or you have better one.
* Talk in-person to the submitter if the comments go back and forth couple of times to decrease reaction time.
* [Nice to have]Can be helpful to leave "Resolve Comment" button for reviewer to use for double-checking your change and signing off on it

### As a Reviewer:

* Be nice; nobody want’s their work critiqued with unnecessary tone or language. If somebody worked hard on a CR and you know it, give them some public encouragement in the form of a CR comment!
    * Like, “Thanks for the update”, “I like this code style”, “Nice way”
* Keep an open mind to other ways of doing things (i.e. just because someone did something differently than you would have done it, that's no reason to ask them to change their proposed code in the CR!), and constantly challenge your own beliefs and assumptions. We all have different paths of experience and bring different knowledge to share.
* If nit-picking, make it explicit with a “nit:” prefix or something similar. Nit-picks are not considered blockers for the CR. Think of them as “hey, maybe try doing it this way next time”.
* All the workaround/tricky change should be marked as blocker in the comment, but we should be flexible for this change. For example, if the changes are safe and can fix a customer facing issue, let them go through and ask the implementer to add the TODO task. 
* Be timely; if you engage with a CR, don’t wait a couple days between revision feedback. If you have no more feedback on the CR and do feel comfortable being the only reviewer, “ship it”. If you have no more feedback on the CR and you don’t feel comfortable being the only approver, “ship it”, and indicate that you’d like someone else to approve as well before pushing.
    * SLA - one day. 
* Batch your comments together. Unless the implementer provides new information to explain existing code / strategy, the 2nd, 3rd, 4th, etc iteration shouldn't include comments about code that was available for review in the 1st revision. Review the entire CR before publishing your comments, and, if it’s a big CR that takes multiple sessions to review, indicate that when publishing each wave of comments.
* Reviewers should clearly state which of their comments are blockers and which are not
    * Only need to add [minor]/[nit]/[optional] when the comment is not a blocker.
* [Nice to have]Use the "Resolve Comment" button to sign off on addressed comments so requester can quickly find anything still pending

Check list

Please follow below check list to help our team speed up the CR process

### As a Publisher:

* Pass code style check
* Meaningful variable name
* Already add necessary comments for reader unfriendly part
* Without any unused change
* Pass UT Coverage check
* Provide enough information in the git commit message and CR description
    * Why and what changes were made along with possible testing, follow the team level CR template if applicable
* Produce CRs related to a single purpose, with formatting and refactoring in their own CR
* Pass Coverlay. If it doesn't, explain why.

*As a Reviewer:*

* Clarify the optional comment with a specific label
* Concise, friendly and actionable comments.
    * Explain what I suggest updating and why.  “Post”, “Wiki” whatever link could prove your point.

## Reference

* SQ Code review check list -- link (https://w.amazon.com/bin/view/SQ/CodeReviewChecklist) 
* Code Reviews in Agile -- link (https://medium.com/@satyajeetbhosale/code-reviews-in-agile-836b3908f7aa)
* AWS_IoT_ThingsGraph CR_Reviewing_Best_Practices — link (https://w.amazon.com/bin/view/AWS_IoT_ThingsGraph/CR_Reviewing_Best_Practices/)



