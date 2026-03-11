# Analytics Naming Conventions

## Overview

We use **two layers of analytics tracking**:

1. **Screen/page views**
2. **Custom interaction events**

These must stay separate.

---

## 1. Screen / Page View Naming

Screen names use this dot notation format:

`web.{section}.{page}` or `app.{section}.{screen}`

Examples:

- `web.home.landing`
- `web.faq.landing`
- `web.edu.landing`
- `web.edu.article`
- `app.onboarding.welcome`
- `app.profile.settings`

### Rules

- Always use lowercase
- Use dot-separated namespaces
- Use short, stable names
- Do not use spaces
- Do not use display labels
- Prefer product/domain structure over UI wording

### Notes

- Screen names describe **where the user is**
- They are used on the screen/page view event
- Do not create duplicate custom `*_viewed` events for pages that already have a screen view

### Example

**Good:**
- `web.edu.article`

**Bad:**
- `Education Hub Article`
- `education:article_viewed` as a duplicate of page view

---

## 2. Custom Event Naming

Custom events use this colon format:

`object:action`

Examples:

- `cta:clicked`
- `nav:clicked`
- `signup:submitted`
- `signup:succeeded`
- `signup:failed`
- `education:filter_selected`
- `education:search_used`
- `education:search_result_clicked`
- `education:article_scrolled`
- `support:contact_clicked`

### Rules

- Always use lowercase
- Format must be `noun:verb_past_tense` or equivalent action label
- Use snake_case after the colon if needed
- Keep names semantic, not UI-specific
- Track **user intent**, not raw UI mechanics unless specifically needed

### Good examples

- `education:filter_selected`
- `education:search_used`
- `support:contact_clicked`
- `profile:updated`
- `settings:saved`

### Bad examples

- `button_clicked`
- `search_icon_clicked`
- `blue_button_tapped`
- `user_clicked_submit_button`

---

## 3. When to Use Screen Views vs Custom Events

### Use a screen/page view when:

You are tracking that a user landed on a page/screen.

Example:
- `screen_view` with `screen_name = web.edu.article`

### Use a custom event when:

You are tracking a meaningful interaction on that page.

Example:
- `education:filter_selected`
- `cta:clicked`
- `signup:submitted`

### Important

Do **not** duplicate page views with custom `*_viewed` events.

Example:
- If screen view is `web.edu.article`, do **not** also send `education:article_viewed`

---

## 4. Parameter Naming

Parameters use `snake_case`.

Examples:

- `cta_name`
- `nav_item`
- `location`
- `destination`
- `filter_name`
- `filter_slug`
- `results_count`
- `search_query`
- `article_id`
- `article_name`
- `scroll_depth`
- `time_on_page_seconds`

### Rules

- Always use lowercase snake_case
- Prefer stable IDs/slugs over visible labels
- Parameters should describe context, not presentation details
- Reuse the same parameter names across events where possible

---

## 5. Standard Parameter Patterns

### Navigation events

Event:
- `nav:clicked`

Params:
- `nav_item` - The navigation item clicked
- `location` - Where the nav is located (header, footer, sidebar)
- `destination` - Where the user is going

Example:
```json
{
  "event": "nav:clicked",
  "nav_item": "education_hub",
  "location": "header",
  "destination": "/education"
}
```

### CTA events

Event:
- `cta:clicked`

Params:
- `cta_name` - Name/identifier of the CTA
- `location` - Where the CTA is located
- `destination` - Where the CTA leads

Example:
```json
{
  "event": "cta:clicked",
  "cta_name": "sign_up",
  "location": "hero",
  "destination": "/signup"
}
```

### Form events

Event:
- `form:submitted`
- `form:field_changed`
- `form:validation_failed`

Params:
- `form_name` - Name/identifier of the form
- `field_name` - Name of the field (for field-level events)
- `validation_errors` - List of validation errors (for validation events)

### Search events

Event:
- `search:performed`
- `search:result_clicked`
- `search:filter_applied`

Params:
- `search_query` - The search query
- `results_count` - Number of results returned
- `filter_name` - Name of filter applied
- `result_position` - Position of clicked result

---

## 6. Platform-Specific Patterns

### Web

- Screen views: `web.{section}.{page}`
- Events: `{section}:{action}`

Examples:
- `web.home.landing`
- `web.profile.settings`
- `home:cta_clicked`
- `profile:updated`

### Mobile App

- Screen views: `app.{section}.{screen}`
- Events: `{section}:{action}`

Examples:
- `app.onboarding.welcome`
- `app.profile.settings`
- `onboarding:completed`
- `profile:updated`

### Backend/API

- Events: `api:{action}` or `{service}:{action}`

Examples:
- `api:request_received`
- `api:error_occurred`
- `auth:login_succeeded`
- `auth:login_failed`

---

## 7. Best Practices

1. **Consistency:** Use the same naming patterns across similar features
2. **Semantic naming:** Name events based on user intent, not UI elements
3. **Avoid duplication:** Don't create `*_viewed` events if you already have screen views
4. **Stable identifiers:** Use IDs/slugs instead of display labels in parameters
5. **Documentation:** Always document new events in the Analytics Tracking sheet
6. **Review:** Review analytics events during PR review to ensure they follow conventions

---

## 8. Integration with PostHog

For platform, we use PostHog as the analytics platform. When implementing analytics:

1. Use PostHog's `capture()` method for custom events
2. Use PostHog's automatic screen view tracking or manually capture screen views
3. Ensure all events follow the naming conventions above
4. Update the Analytics Tracking sheet in Notion when adding new events

---

## References

- PostHog documentation: https://posthog.com/docs
- Analytics Tracking sheet: (link to Notion page)
- This file: `.claude/rules/product/analytics-naming.md`
