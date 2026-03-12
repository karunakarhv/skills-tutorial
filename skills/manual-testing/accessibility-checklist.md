# Accessibility Testing Checklist (WCAG 2.1 — Manual Checks)

## Keyboard Navigation
- [ ] All interactive elements reachable by Tab key
- [ ] Tab order follows logical visual order
- [ ] Focus indicator visible on all interactive elements (not just outline: none)
- [ ] Modal dialogs trap focus inside; focus returns to trigger on close
- [ ] No keyboard traps (can always Tab away from any element)
- [ ] Dropdowns and menus operable with arrow keys
- [ ] Forms submittable with Enter key

## Screen Reader (Semantic HTML)
- [ ] Page has a single `<h1>` and logical heading hierarchy (h1 → h2 → h3)
- [ ] All images have descriptive `alt` text (decorative images have `alt=""`)
- [ ] Form inputs have associated `<label>` elements
- [ ] Buttons have descriptive text (not "Click here" or "Submit")
- [ ] Links have descriptive text (not "Read more" or "Click here")
- [ ] ARIA labels used where native HTML semantics aren't sufficient
- [ ] Dynamic content changes announced to screen readers (`aria-live`)

## Color & Contrast
- [ ] Text contrast ratio is at least 4.5:1 (normal text) or 3:1 (large text)
- [ ] Information not conveyed by color alone (error states use icon + color, not color only)
- [ ] Focus indicators have sufficient contrast against background

## Forms
- [ ] Required fields indicated (not by color alone)
- [ ] Error messages associated with their fields (`aria-describedby`)
- [ ] Error messages are descriptive ("Email must include @" not "Invalid input")
- [ ] Success/completion states announced

## Multimedia
- [ ] Videos have captions
- [ ] Audio has transcripts
- [ ] No auto-playing audio/video (or there's a way to stop it)
- [ ] Animations can be paused or disabled

## Structure & Navigation
- [ ] Page has a descriptive `<title>`
- [ ] Skip navigation link available (or landmark regions used)
- [ ] Language set on `<html lang="en">`
- [ ] Consistent navigation across pages

## Quick Manual Test Process
1. Navigate entire page using only Tab/Shift-Tab/Enter/Space
2. Check all images have alt text (right-click → Inspect, look for alt attribute)
3. Run the page through Chrome's built-in Lighthouse Accessibility audit
4. Check color contrast using browser DevTools or a contrast checker
