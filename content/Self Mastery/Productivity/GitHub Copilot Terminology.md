# Terminology
so much to keep track of and constantly changing. trying to keep this up to date

# The Term "Agent"

Used for five different things:

| Feature      | Location                    | Function                                                             | Use Case                                                                  |
| ------------ | --------------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Agent Mode   | VS Code chat pane           | Real time, synchronous                                               | Active coding and iterative building                                      |
| Custom Agent | `.github/agents/*.agent.md` | Personas. Specialist with unique tools and instructions              | Specialized roles. Can include specific handoffs. Called with `@` in chat |
| Cloud Agent  | context menus / TODOs       | Asynchronous. Sends a task to cloud agent for work in the background | Cleanup or refactors you dont want to watch                               |
| CLI Agent    | Terminal                    | CLI. Runs agents or prompts in the shell                             | Automating CI/CD or repo-wide audits via scripts                          |
| Coding Agent | github.com                  | Cloud-based. Builds pull requests from an issue.                     | High-level implementation from a web browser                              |
|              |                             |                                                                      |                                                                           |
# Skills
**Purpose:** Reusable "knowledge modules" loaded automatically when relevant. Best for domain logic and "gotchas."
- **Location:** `.github/skills/<skill-name>/`
- **Core File:** `SKILL.md`
- **Max Length**: 1,000-3,000 characters
	- concise description in YAML (max 1,024 characters) - **critical** because this is what copilot uses to decide if it should use the skill
- **Description**: ensure the description makes the skill "highly discoverable" so the agent will know to use it based on the user's prompt
- **Additional Files**:
	- stored in the `.github/skills/<skill-name>/` folder
	- each additional file should be <5MB
	- Copilot will pull what it deems relevant to the current query
	- Aim for 3-5 high-value reference files per skill
	- explicitly mention them in `SKILL.md` so the agent knows they exist and what they contain
- **Discovery:** Loaded via its `description` in YAML.
- **Example (`.github/skills/prediction-math/SKILL.md`):**
- other resources can be placed in the folder
    
```markdown
---
name: prediction-math
description: Logic for Yes/No contracts, log-odds, and fee calculations.
---
# Instructions
- Always verify YesPrice + NoPrice = 1.0.
- Log-odds transform: log(p / (1-p)).
```

# Custom Agents
**Purpose:** Specialized "personas" with specific tools and workflows
- **Location:** `.github/agents/`
- **Core File:** `<name>.agent.md`
- **Max Length**: 2,000-4,000 characters
- **Features:** Can define `tools` (terminal, edit) and `handoffs` (buttons to switch agents).
- **Example (`.github/agents/quant-reviewer.agent.md`):
- can define "handoffs" to other agents

```markdown
---
name: quant-reviewer
description: Evaluates code for vectorized efficiency.
tools: [tool1, tool2]
	- read      # to see the code
    - search    # to find things in the project
	- edit      # to suggest code changes
	- terminal  # to run tests or buids
	- fetch     # pull content from a URL
---
You are a strict Quant Reviewer. Reject any Python `for` loops in pandas logic. 
Enforce `numpy` vectorization.
```

# Prompts
**Purpose:** On-demand "templates" or complex instructions triggered by a slash command.
- **Location:** `.github/prompts/`
- **Core File:** `<command-name>.prompt.md`
- **Trigger:** Type `/command-name` in chat.
- **Example (`.github/prompts/api-test.prompt.md`):**

```markdown
---
name: api-test
description: Scaffolds a pytest for a new API endpoint.
---
Create a new test file in `tests/bin/` for the `#file` endpoint. 
Include mocks for authentication and a 400 error case.
```

# Handoffs
### Chaining & Handoffs (The Workflow Bridge)
- ensure that `prompt` is a specific mission statement so the next agent knows the context

To link your **Developer** to your **Quant Reviewer**, add this to `developer.agent.md`:
```yaml
handoffs:
  - label: "Request Quant Review"
    agent: quant-reviewer
    prompt: "Please audit the vectorization in these new functions."
```
