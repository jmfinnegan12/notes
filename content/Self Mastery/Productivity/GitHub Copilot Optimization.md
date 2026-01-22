# GitHub Copilot Optimization
big workflow upgrade -- code produced from copilot is consistently now much higher quality than before. agents now follow my instructions more accurately with fewer necessary corrections -- i am more productive and less frustrated.

Setup
- got rid of monorepo. projects are now organized into their own repos
- explicit and concise instructions
	- define persona
	- preference detail (eg google docstring format, make sure code is readable, etc)
	- specify type check, linting, and run testing before returning task as completing -- this was huge -- i used to spend lots of time and tokens fixing linting errors and debugging silly mistakes. this is now all explicitly stated in instructions and taken care of automatically--no need for me to restate it
- plan project before it starts with a highly intelligent model
- then execute workflow -- busy work tasks done with less intelligent (cheaper) models, while debugging, problem solving, reasoning done with more intelligent models

Workflow
- design the project
	- start with a concept. flesh it out into a detailed description
	- iterate with highly intelligent model (Claude Opus 4.5) until i have a detailed plan of the following:
		- list of components and how they interact
		- big picture phases starting with most basic, bare-bones prototype. working up to full idea
		- detailed list of step by step instructions for each phase
- build the project
	- new agent (chat) for each step -- prevents cluttered chats and unnecessary context
	- task the chat with completing the step
	- manual review