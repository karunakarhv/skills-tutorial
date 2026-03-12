# Manual Testing Skill — README

A skill that gives Claude deep expertise in software manual testing. Once installed, Claude can write test plans, design test cases, run structured testing protocols, report bugs, and — critically — help AI agents verify their own work before marking tasks complete.

---

## File Structure

```
manual-testing-skill/
├── SKILL.md                              ← Main skill (always loaded)
└── references/
    ├── common-bug-patterns.md            ← 10 bug categories to hunt for
    ├── security-checklist.md             ← OWASP Top 10 manual checks
    └── accessibility-checklist.md        ← WCAG 2.1 manual checks
```

`SKILL.md` is always loaded into context when the skill triggers. The reference files are consulted on demand when a specific testing type is requested (e.g. "test this for security issues" will cause Claude to read `security-checklist.md`).

---

## Installation

### In Claude.ai
1. Go to **Settings → Skills**
2. Click **Add Skill**
3. Upload the `manual-testing-skill/` folder (or the `.skill` package if provided)
4. The skill will appear in your skill list as `manual-testing`

### In Claude Code / Cowork
Place the skill folder in your skills directory:
```bash
cp -r manual-testing-skill/ ~/.claude/skills/manual-testing/
```

---

## What It Does

This skill teaches Claude to act as a senior QA engineer. It covers:

| Capability | What Claude will produce |
|---|---|
| **Test Planning** | Full test plan with scope, objectives, risk matrix, entry/exit criteria |
| **Test Case Design** | Structured test cases using equivalence partitioning, boundary analysis, decision tables |
| **Exploratory Testing** | Session charters, SFDIPOT heuristics, session notes |
| **Bug Reporting** | Detailed bug reports with steps to reproduce, priority, severity |
| **API Testing** | Endpoint-by-endpoint checklist covering auth, validation, error handling |
| **UI/UX Testing** | Functional, responsive, and accessibility checks |
| **Regression Testing** | Smoke test, targeted regression, and full regression strategies |
| **Agent Self-Testing** | Structured protocol for AI agents to verify their own output |
| **Security Testing** | Basic OWASP Top 10 manual checks (no special tools needed) |
| **Accessibility Testing** | WCAG 2.1 keyboard, screen reader, and contrast checks |

---

## How to Use It

The skill triggers automatically based on what you ask. You don't need to mention "use the testing skill" — just describe your testing need naturally.

### Example Prompts

**Writing test cases:**
> "Write test cases for a user login form that accepts email and password"

> "I have a checkout flow with 3 steps. Write test cases covering the happy path and edge cases."

**Creating a test plan:**
> "Create a test plan for the new notifications feature we're shipping next week"

**Exploratory testing:**
> "Give me an exploratory testing charter for the search functionality"

**Reporting a bug:**
> "Help me write a bug report — the save button doesn't work when the title field has more than 100 characters"

**API testing:**
> "Write a test case for a POST /api/orders endpoint that creates a new order"

> "Give me a checklist to manually test our REST API before launch"

**Agent self-verification:**
> "I just built a form validation function. Run through the self-testing protocol on it."

> "Before I mark this task done, help me verify the feature is working correctly"

**Regression testing:**
> "We just changed the authentication module. What should I regression test?"

**Security checks:**
> "Do a basic security check on our login page"

**Accessibility:**
> "Check this form for accessibility issues"

---

## Trigger Keywords

The skill activates automatically when your message contains words like:

`test`, `testing`, `test case`, `test plan`, `QA`, `quality assurance`, `bug`, `bug report`, `defect`, `regression`, `exploratory`, `smoke test`, `sanity check`, `acceptance criteria`, `checklist`, `validate`, `verify`, `API test`, `UI test`, `accessibility`, `security check`

---

## For AI Agents

This skill is especially valuable in agentic workflows where Claude is building and then needs to verify its work. The **Agent Self-Testing Protocol** (in `SKILL.md`) defines a structured 6-step process:

1. Define expected behavior before testing
2. Run the happy path
3. Run boundary and edge cases
4. Run negative / failure cases
5. Verify error handling
6. Log results using the execution log template

You can instruct your agent system prompt to use this skill like so:

```
Before marking any task complete, use the manual-testing skill to run the
Agent Self-Testing Protocol on your output. Document the results in the
Test Execution Log format.
```

---

## Customising the Skill

You can extend the skill by adding your own reference files to the `references/` folder and linking to them from `SKILL.md`. For example:

- `references/your-app-test-data.md` — Standard test accounts, environments, URLs
- `references/your-regression-suite.md` — Your team's core regression test cases
- `references/your-bug-template.md` — Bug report format customised for your issue tracker (Jira, Linear, GitHub Issues, etc.)

To link a new reference file, add a line to the bottom of `SKILL.md`:
```markdown
- `references/your-app-test-data.md` — Standard test data and environment details
```

---

## Reference Files

Load these by asking Claude about a specific area:

| File | When Claude reads it |
|------|----------------------|
| `common-bug-patterns.md` | "What should I look for?" / exploratory testing |
| `security-checklist.md` | "Test this for security" / "basic security checks" |
| `accessibility-checklist.md` | "Check accessibility" / "WCAG" / "screen reader" |

---

## Limitations

- This skill covers **manual** testing only. It does not write automated test scripts (unit tests, Playwright, Cypress, etc.). For automated testing, a separate skill would be needed.
- Security checks are **basic manual checks** only — not a substitute for a dedicated security audit or automated scanner (Burp Suite, OWASP ZAP, etc.).
- Performance testing (load testing, stress testing) is out of scope.