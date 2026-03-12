---
name: manual-testing
description: >
  Expert software manual QA testing assistant for agents and humans. Use this skill whenever
  the user wants to: write test cases, create test plans, design test scenarios, perform exploratory
  testing, report bugs, validate features, do regression testing, write acceptance criteria, create
  QA checklists, test APIs manually, test UI/UX flows, or review any software for quality issues.
  Trigger this skill for any request involving "test", "QA", "quality assurance", "bug report",
  "test case", "test plan", "acceptance criteria", "regression", "exploratory testing", "smoke test",
  "sanity check", or "defect". This skill is also essential when an agent needs to verify that
  a feature or system is working correctly before marking a task as complete.
---

# Manual Software Testing Skill

A comprehensive guide for conducting rigorous manual software testing — whether you're a QA agent
verifying your own output, a human tester documenting test cases, or an AI agent systematically
validating a feature before shipping.

---

## Quick Start: What Are You Doing?

| Task | Jump to |
|------|---------|
| Testing a feature I just built | [Agent Self-Testing Protocol](#agent-self-testing-protocol) |
| Writing a test plan | [Test Planning](#test-planning) |
| Writing test cases | [Test Case Design](#test-case-design) |
| Exploratory testing | [Exploratory Testing](#exploratory-testing) |
| Reporting a bug | [Bug Reporting](#bug-reporting) |
| API testing | [API Testing](#api-testing) |
| UI/UX testing | [UI-UX Testing](#ui-ux-testing) |
| Regression testing | [Regression Testing](#regression-testing) |

---

## Agent Self-Testing Protocol

When an agent completes a task and needs to verify it before reporting success, follow this protocol:

### Step 1: Define the Expected Behavior
Before testing, explicitly state:
- What the feature/code/output is supposed to do
- What inputs it accepts
- What outputs or side effects are expected
- What it should NOT do

### Step 2: Run the Happy Path First
Test the most common, "correct" usage:
1. Use valid, typical inputs
2. Verify the output matches the specification exactly
3. Check for any unintended side effects

### Step 3: Run Boundary & Edge Cases
```
For each input field or parameter:
  - Empty / null / undefined
  - Minimum allowed value
  - Maximum allowed value
  - One below minimum (should fail gracefully)
  - One above maximum (should fail gracefully)
  - Special characters: !@#$%^&*()'"<>/\
  - Very long strings (1000+ chars)
  - Whitespace only
  - Unicode / emoji: 你好 🎉 ñ
```

### Step 4: Run Negative Cases
- What happens with completely wrong input types?
- What happens when required dependencies are missing?
- What happens if the user has no permissions?
- What happens on network failure or timeout?

### Step 5: Verify Error Handling
- Are errors surfaced clearly to the user?
- Are error messages helpful (not stack traces exposed to users)?
- Does the system recover gracefully?

### Step 6: Document Results
Use the [Test Execution Log](#test-execution-log-template) template.

---

## Test Planning

### Test Plan Structure

**1. Scope**
- What IS being tested (features, modules, APIs)
- What is NOT being tested (and why)
- Test environment (OS, browser, device, version)

**2. Test Objectives**
- What quality criteria must be met?
- What are the acceptance criteria for release?

**3. Test Types to Execute**
- [ ] Functional testing
- [ ] Regression testing
- [ ] Boundary/edge case testing
- [ ] Negative testing
- [ ] UI/UX testing
- [ ] API testing
- [ ] Performance (basic manual checks)
- [ ] Security (basic manual checks)
- [ ] Accessibility

**4. Entry & Exit Criteria**
- Entry: "Testing begins when the build is deployed to staging and smoke tests pass"
- Exit: "Testing ends when all P1/P2 bugs are resolved and test coverage >= 90%"

**5. Risk Assessment**
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Authentication flows break | Medium | High | Test login first |
| Data loss on save | Low | Critical | Backup before testing |

---

## Test Case Design

### Test Case Template

```
ID: TC-[MODULE]-[NUMBER]
Title: [Short, descriptive name]
Priority: P1 (Critical) | P2 (High) | P3 (Medium) | P4 (Low)
Type: Functional | Regression | Edge Case | Negative | UI | API
Preconditions: [State the system must be in before this test]
Test Data: [Specific inputs to use]

Steps:
1. [Action]
2. [Action]
3. ...

Expected Result: [Exactly what should happen]
Actual Result: [Fill in during execution]
Status: Pass | Fail | Blocked | Skip
Notes: [Any observations]
```

### Test Case Design Techniques

**Equivalence Partitioning**
Group inputs into classes where all values behave the same way. Test one from each class.
- Example for age field (valid: 18-65): test 18, 40, 65 (valid); test 17, 66 (invalid)

**Boundary Value Analysis**
Always test the edges. If a field accepts 1-100 characters:
- Test: 0, 1, 2, 99, 100, 101

**Decision Table Testing**
For complex business rules with multiple conditions:
```
Condition A | Condition B | Expected Outcome
TRUE        | TRUE        | Result 1
TRUE        | FALSE       | Result 2
FALSE       | TRUE        | Result 3
FALSE       | FALSE       | Result 4
```

**State Transition Testing**
Map all states an object can be in and test each valid/invalid transition:
```
[Draft] --submit--> [Pending] --approve--> [Active] --deactivate--> [Inactive]
         --delete-->  [Deleted]
```

**Pairwise Testing**
When you have many parameters, test all pairs of values (not all combinations) to maximize coverage efficiently. Read `references/pairwise-testing.md` for details.

---

## Exploratory Testing

Exploratory testing is simultaneous learning, design, and execution. Use this when:
- Requirements are unclear or incomplete
- You're testing a new or unfamiliar feature
- You want to find bugs that scripted tests miss

### Charter Format
```
Explore [AREA/FEATURE]
With [RESOURCES/TOOLS]
To discover [INFORMATION/RISKS]
```

Example:
```
Explore the user registration flow
With a focus on form validation and error states
To discover any edge cases that allow invalid data to be submitted
```

### Exploratory Testing Heuristics (SFDIPOT)

| Heuristic | What to test |
|-----------|-------------|
| **S**tructure | Data the product contains (files, databases, fields) |
| **F**unctions | Things the product does (actions, operations) |
| **D**ata | Inputs and outputs (valid, invalid, boundary) |
| **I**nterface | External dependencies (APIs, other systems, integrations) |
| **P**latform | Environment (OS, browser, device, network conditions) |
| **O**perations | How users actually use it (workflows, sequences) |
| **T**ime | Time-related issues (timeouts, timestamps, scheduling, concurrency) |

### Session Notes Template
```
Session: [Date/Time]
Charter: [What you set out to test]
Duration: [Time spent]

Areas Covered:
- 

Bugs Found:
- 

Questions/Risks:
- 

Ideas for Next Session:
- 
```

---

## Bug Reporting

### Bug Report Template

```
Title: [Action] + [Expected] + [Actual] — e.g., "Login button does nothing when email contains spaces"

Priority: P1-Critical | P2-High | P3-Medium | P4-Low
Severity: Blocker | Major | Minor | Trivial
Status: New | In Progress | Fixed | Verified | Closed
Environment: [OS, Browser/App version, Test environment]
Found by: [Tester name/agent]
Date: [YYYY-MM-DD]

Steps to Reproduce:
1. Navigate to [URL/screen]
2. Enter "[specific value]" in [field]
3. Click [button/action]
4. Observe result

Expected Result:
[Exactly what should have happened]

Actual Result:
[Exactly what happened instead]

Attachments:
- Screenshot: [filename]
- Video: [filename]
- Logs: [relevant log snippet]

Additional Notes:
[Workaround, frequency, related issues]
```

### Priority Guidelines

| Priority | Definition | Example |
|----------|-----------|---------|
| P1 Critical | System unusable, data loss, security breach | Login broken, payments failing, data deleted |
| P2 High | Major feature broken, no workaround | Cannot save a form, search returns wrong results |
| P3 Medium | Feature broken but workaround exists | Sort order wrong, tooltip missing |
| P4 Low | Minor cosmetic or UX issue | Typo, misaligned button, wrong color |

---

## API Testing

### Manual API Testing Checklist

For each endpoint, verify:

**Request Validation**
- [ ] Valid request returns 200/201 with correct response body
- [ ] Missing required fields return 400 with clear error message
- [ ] Invalid field types return 400 with clear error message
- [ ] Extra/unexpected fields are handled (ignored or rejected)
- [ ] Correct HTTP method (GET/POST/PUT/PATCH/DELETE)

**Authentication & Authorization**
- [ ] Unauthenticated request returns 401
- [ ] Authenticated but unauthorized user returns 403
- [ ] Expired token returns 401 (not 500)
- [ ] Invalid token format returns 401

**Response Validation**
- [ ] Response schema matches documentation
- [ ] All documented fields are present
- [ ] Data types are correct (string, number, boolean, array)
- [ ] Timestamps are in correct format (ISO 8601)
- [ ] Pagination works correctly (page, limit, total)

**Edge Cases**
- [ ] Empty list returns `[]` not `null`
- [ ] Large payloads handled gracefully
- [ ] Special characters in string fields
- [ ] Maximum/minimum numeric values

**Error Handling**
- [ ] 404 for non-existent resources
- [ ] 409 for conflicts (duplicate creation)
- [ ] 422 for unprocessable entity
- [ ] 500 errors don't leak stack traces
- [ ] Rate limiting returns 429 (if applicable)

### API Test Case Example
```
Endpoint: POST /api/users
Test: Create user with valid data

Request:
POST /api/users
Content-Type: application/json
Authorization: Bearer {valid_token}

{
  "email": "test@example.com",
  "name": "Test User",
  "role": "viewer"
}

Expected Response:
Status: 201 Created
Body: {
  "id": "[any UUID]",
  "email": "test@example.com",
  "name": "Test User",
  "role": "viewer",
  "createdAt": "[valid ISO timestamp]"
}

Assertions:
- Status code is 201
- Body.id matches UUID format
- Body.email matches request email
- Body.createdAt is a valid date
```

---

## UI-UX Testing

### Functional UI Checklist

**Forms**
- [ ] All required fields are marked (asterisk or label)
- [ ] Submitting empty required fields shows inline errors
- [ ] Errors appear next to the relevant field (not just at top)
- [ ] Successful submission shows confirmation
- [ ] Form resets or navigates appropriately after submit
- [ ] Tab order is logical

**Navigation**
- [ ] All links go to correct destination
- [ ] Back button works as expected
- [ ] Breadcrumbs are accurate
- [ ] Active page is highlighted in nav
- [ ] Deep links work (direct URL access)

**Data Display**
- [ ] Long text truncates or wraps gracefully
- [ ] Empty states have helpful messages (not blank screens)
- [ ] Loading states are shown for async operations
- [ ] Lists handle 0 items, 1 item, many items
- [ ] Numbers are formatted correctly (commas, decimals, currency)
- [ ] Dates are formatted in the user's locale

**Responsive Design**
- [ ] Mobile (375px width)
- [ ] Tablet (768px width)
- [ ] Desktop (1280px+ width)
- [ ] Text doesn't overflow containers
- [ ] Buttons are tappable on mobile (44px minimum)

**Accessibility (Basic)**
- [ ] All images have alt text
- [ ] Form fields have labels
- [ ] Color alone is not used to convey meaning
- [ ] Focus indicators are visible
- [ ] Page has a logical heading structure

---

## Regression Testing

### Regression Strategy

After any code change, test:

1. **Smoke Test** (5-10 min): Verify the most critical paths still work
   - Can users log in?
   - Can users complete the core workflow?
   - Is the app loading without errors?

2. **Targeted Regression** (test areas affected by the change):
   - Map the change to affected modules
   - Test all features in those modules
   - Test integration points with other modules

3. **Full Regression** (for major releases):
   - Execute all P1 and P2 test cases
   - Cover all user-facing features

### Regression Test Selection (Change-Impact Matrix)
```
Changed: [authentication module]
Impacts:
  - Login flow           → Test all login scenarios
  - Password reset       → Test password reset flow
  - Session management   → Test timeout, refresh, multi-tab
  - API auth headers     → Test authenticated API calls
  - "Remember me"        → Test persistence across sessions
```

---

## Test Execution Log Template

```
Test Run: [Feature/Build version]
Environment: [Staging/Production] [URL]
Tester: [Name/Agent ID]
Date: [YYYY-MM-DD]
Duration: [Time spent]

Summary:
  Total Tests: __
  Passed:      __
  Failed:      __
  Blocked:     __
  Skipped:     __
  Pass Rate:   __%

Results:
| ID        | Title                   | Status  | Notes |
|-----------|-------------------------|---------|-------|
| TC-AUTH-01| Login with valid creds  | PASS    |       |
| TC-AUTH-02| Login with wrong password| FAIL   | See bug BUG-042 |
| TC-AUTH-03| Login with empty fields | BLOCKED | Depends on TC-AUTH-01 |

Bugs Found:
- BUG-042: [Short description]

Overall Assessment:
[ ] Ready to release
[ ] Release with known issues (list them)
[ ] NOT ready — blockers present
```

---

## Quality Heuristics (Quick Reference)

When you're not sure what to test next, ask:

- **CORRECT**: Is the result **C**onforming, **O**rdered, **R**ange-bound, **C**ross-checked, **R**eferenced, **E**xistence-checked, **T**imely?
- **FALLIBLE**: What could go wrong? (network, disk, memory, permissions, time)
- **CONSISTENT**: Does the behavior match across browsers, devices, users, and time?
- **GOLDILOCKS**: Is the response too slow, too fast, or just right?
- **PERSONA**: Would a new user understand this? An expert? A user with disabilities?

---

## Reference Files

- `references/pairwise-testing.md` — Efficient multi-parameter test design
- `references/common-bug-patterns.md` — Frequently occurring bug categories to look for
- `references/accessibility-checklist.md` — Detailed WCAG 2.1 accessibility checks
- `references/security-checklist.md` — Basic manual security checks (OWASP top 10)
