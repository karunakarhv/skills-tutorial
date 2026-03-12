# Common Bug Patterns Reference

A curated list of bug categories that appear frequently across software systems.
Use this as a checklist during exploratory testing to make sure you're looking in the right places.

---

## 1. Input & Validation Bugs

- **Missing validation**: Field accepts values it shouldn't (negative quantities, future birthdates, SQL in text boxes)
- **Client-only validation**: Server doesn't validate — bypass the UI and send bad data directly to the API
- **Inconsistent validation**: Same field validated differently on create vs. update, or in different parts of the app
- **Trimming not applied**: " admin" (with leading space) treated as different from "admin"
- **Case sensitivity**: "Admin" != "admin" in login but treated the same in search
- **Encoding issues**: HTML entities (`&amp;`, `&lt;`) displayed literally instead of rendered

## 2. Boundary & Off-By-One Bugs

- Off-by-one in loops: iterating 1 too many or too few times
- Inclusive vs. exclusive ranges: "up to 100" — does that include 100?
- Zero-based vs. one-based indexing confusion
- Maximum length: field shows 255 chars allowed, but database column is VARCHAR(250)
- Page boundaries: last item on page 1 also appears as first item on page 2

## 3. State & Timing Bugs

- **Race conditions**: Two users edit same record simultaneously, one overwrites the other
- **Stale data**: Page shows cached data after underlying data changes
- **Session state**: Actions after logout still succeed; logged-in state persists longer than expected
- **Back button**: User navigates back after form submit, re-submit occurs
- **Concurrent requests**: Clicking a button twice submits two requests

## 4. Error Handling Bugs

- **Silent failures**: Operation fails but user sees success message (or nothing)
- **Unhelpful errors**: "Something went wrong" with no recovery path
- **Stack traces exposed**: Raw exception details shown to end users
- **Retry not available**: Failed operation requires starting over from scratch
- **Partial success**: Some steps succeed before failure — system left in inconsistent state

## 5. Authentication & Authorization Bugs

- **Broken access control**: User A can access User B's data by changing an ID in the URL
- **Missing auth check**: API endpoint works without authentication token
- **Privilege escalation**: Regular user can perform admin actions
- **Insecure direct object reference (IDOR)**: `/invoices/1042` accessible by anyone who guesses the ID
- **Token issues**: Token never expires; invalidated token still works; token contents trusted without verification

## 6. Data Persistence Bugs

- **Data not saved**: Changes appear in UI but not persisted to database
- **Data not loaded**: Saved data doesn't appear on next page load
- **Cascade failures**: Deleting a parent record should delete children — doesn't (or vice versa)
- **Duplicate records**: Submitting form twice creates two records instead of one
- **Truncation**: Long strings silently truncated without error

## 7. Calculation & Logic Bugs

- **Rounding errors**: `0.1 + 0.2 = 0.30000000000000004` — floating point precision issues
- **Currency**: Calculations done in floating point instead of integer cents
- **Timezone bugs**: Event at "5pm" shows as different time in different timezones
- **DST transitions**: Clocks going forward/back cause 1-hour errors or duplicate/missing events
- **Date math**: Adding 1 month to January 31st — what's the result?

## 8. UI & Display Bugs

- **Layout breaks**: UI works at 1280px but breaks at 1024px or mobile
- **Long content**: Long name overflows container, breaks layout, or hides other elements
- **Empty states**: No message shown when a list is empty — just blank space
- **Loading states**: No indicator shown while async operations run — user doesn't know to wait
- **Z-index issues**: Dropdown menus hidden behind other elements
- **Focus trapping**: After modal closes, focus doesn't return to the triggering element

## 9. Integration & API Bugs

- **Dependency not checked**: Feature works in isolation but fails when dependent service is down
- **Version mismatch**: Frontend expects API v2 response shape, but calls v1 endpoint
- **Timeout handling**: Long API call times out — no feedback to user, or app crashes
- **Webhook not retried**: External webhook fails silently, data never synced
- **Encoding mismatch**: API returns UTF-8, consumer interprets as Latin-1

## 10. Configuration & Environment Bugs

- **Works on my machine**: Feature works in development but fails in staging/production
- **Missing env variable**: App silently uses `undefined` when environment variable not set
- **Feature flags**: Feature enabled in the wrong environment
- **Hardcoded values**: Dev/test URLs or credentials hardcoded and shipped to production
