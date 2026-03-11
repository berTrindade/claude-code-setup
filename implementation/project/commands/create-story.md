---
description: Create a user story following the project template. Reads analytics tracking, references existing tickets, generates story, waits for review before creating.
argument-hint: [story-description]
allowed-tools: Read, Edit, Write, Bash, Grep
model: sonnet
---

Create a user story following the project's user story template. This command reads existing analytics tracking sheets, references existing tickets as templates, and generates a new story following the standard format.

## Step 1 — Understand the request

Clarify:
- What platform is this for? (web, app, backend)
- What is the user story about?
- What is the business context?
- Are there any dependencies or related tickets?

## Step 2 — Read analytics rules

Read the analytics naming conventions from `.claude/rules/product/analytics-naming.md` to understand:
- Screen/page view naming format
- Custom event naming format
- Parameter naming conventions
- When to use screen views vs custom events

## Step 3 — Read existing analytics tracking

If Notion MCP is available, read the Analytics Tracking sheet to understand:
- What analytics events already exist
- What naming patterns are used
- What parameters are standard

**Note:** If Notion MCP is not available, skip this step and proceed with analytics rules only.

## Step 4 — Reference existing tickets

Search GitHub or Linear for similar tickets to use as templates:

```bash
# GitHub
gh issue list --label "feature" --limit 10 --json number,title,body

# Or search for specific examples
gh issue list --search "app | Log in" --limit 5
```

Look for tickets that:
- Are on the same platform (web/app/backend)
- Have similar functionality
- Have well-structured acceptance criteria
- Include analytics definitions

**Good template examples to look for:**
- Login/authentication flows
- Feature implementations
- User-facing features

## Step 5 — Generate the story

Create a user story following this template:

```markdown
# Summary

As a <user type>,
I want to <perform some action>,
So that I <get some value/achieve something>

## Business context

<If provided, include business context here>

## Acceptance criteria

**AC1 - <Title of acceptance criteria>**

Given that <precondition>
When <action>
Then <expected outcome>

**AC2 - <Title of acceptance criteria>**

Given that <precondition>
When <action>
Then <expected outcome>

## Analytics

### Screen/Page Views
- `web.{section}.{page}` or `app.{section}.{screen}`

### Custom Events
- `object:action` with parameters:
  - `parameter_name`: <description>

**Note:** Follow analytics naming conventions from `.claude/rules/product/analytics-naming.md`

## Not included

<Anything that is not a focus for this ticket (e.g., has been split into separate tickets, or not in scope)>

---

## Discuss in kick off

- [ ] A/C review
- [ ] Accessibility criteria
- [ ] How we will prevent dependency chains
- [ ] Meaningful unit tests
- [ ] Animations/page transitions
- [ ] Does the effort estimate feel right?

## FIGMA Links

<Add any relevant FIGMA links here>

### Dependencies

<List any dependencies that this task has, such as tasks that need to be completed first>

-

### PR Links

<Links to related Pull Requests>

- 

### Definition of Done

_A checklist of tasks that must be completed and ticked off for this ticket to be considered 'done'_

- [ ] Manually tested across supported browsers/devices
- [ ] Relevant e2e/unit snapshot tests written
- [ ] Reviewed and approved by design and product
- [ ] Smoke tested on Android by engineer (not QA) [limit to 20 mins fixes or crashes only]
- [ ] Analytics requirements met
- [ ] Accessibility standards met
- [ ] PR reviewed and approved by one other engineer and merged
- [ ] Appropriate documentation completed (e.g., ADRs/read me's/how to test)
```

**Title format:**
- `web | <short description>` for web features
- `app | <short description>` for mobile app features
- `backend | <short description>` for backend features

**Style guidelines:**
- Keep descriptions simple and focused
- No em dashes or semicolons in descriptions
- Use casual tone
- Be concise and clear

## Step 6 — Review and confirm

**IMPORTANT:** Display the generated story and wait for user review and approval before creating the ticket.

Ask:
- Does this capture the requirements correctly?
- Are the acceptance criteria complete?
- Are the analytics events appropriate?
- Should anything be moved to "Not included"?

## Step 7 — Create the ticket

Once approved, create the ticket:

**For GitHub:**
```bash
gh issue create \
  --title "<platform> | <description>" \
  --body-file <generated-story-file> \
  --label "feature"
```

**For Linear:**
Use Linear MCP to create the issue with the generated content.

**Important:** 
- Set project/label as appropriate (e.g., "platform V1 Build")
- Set type as "Feature"
- Link to any dependencies mentioned

## Step 8 — Update analytics tracking

If new analytics events were added and Notion MCP is available, update the Analytics Tracking sheet in Notion with the new events.
