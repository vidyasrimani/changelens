# ChangeLens — Product Requirements

**Status:** Draft (product discovery)  
**Version:** 0.1  
**Last updated:** 2026-10-07

## 1. Problem

Users repeatedly check webpages while waiting for specific changes that matter to them. This requires ongoing attention and becomes increasingly time-consuming when monitoring multiple pages. Many page updates are unrelated to the user's interests; manually distinguishing relevant changes from noise adds to the burden.

## 2. Users and use cases

ChangeLens is not limited to one domain. Examples include:

- **Job seekers:** new relevant positions on a company's careers page.
- **Shoppers:** product price drops or availability.
- **Event attendees:** ticket availability or sale opening.
- **Researchers and citizens:** relevant changes to public government or regulatory webpages.
- **Developers:** relevant documentation and release-note changes.

These cases represent item additions, threshold crossings, state transitions, and text updates. Full support for every category in the first release is not yet a commitment.

## 3. Product goals

- Let users specify a publicly accessible URL and describe what matters in natural language without HTML or CSS expertise.
- Observe configured pages over time and identify changes relevant to the user's intent.
- Reduce irrelevant notifications and make relevant changes understandable.
- Notify users and retain user-visible change history.
- Distinguish a successful check with no relevant change from an unsuccessful or uninterpretable check.

## 4. Functional requirements

### Monitor management

- **FR-01:** Users can create a monitor for one public URL and describe their intent in natural language.
- **FR-02:** Users can view their monitors.
- **FR-03:** Users can edit monitors; edits apply prospectively and do not reinterpret earlier change records.
- **FR-04:** Users can pause, resume, and delete monitors.
- **FR-05:** The initial free-user limit is 20 active monitors.

### Observation and relevance

- **FR-06:** The system repeatedly observes the configured webpage.
- **FR-07:** It evaluates observed changes against the user's intent rather than reporting every raw webpage change.
- **FR-08:** Intended change categories include new items, changed values, thresholds, availability transitions, and relevant text updates. We will prioritize which of these ship in v1.
- **FR-09:** For condition-based monitoring, notify when a condition transitions from false to true, not repeatedly while it remains true. A later false-to-true transition may create a new notification.
- **FR-10:** When content interpretation is unreliable (for instance, after a substantial redesign), communicate uncertainty instead of falsely claiming a relevant change.

### History and notifications

- **FR-11:** Users can inspect detected relevant changes, observation times, and sufficient before/after context to understand what changed.
- **FR-12:** User-visible detected change history is retained for 90 days.
- **FR-13:** Initial notification channels are email and in-app history.
- **FR-14:** One logical detected change should not result in duplicate user-facing notifications.

### Monitoring failures

- **FR-15:** Clearly distinguish no relevant change from an unsuccessful/uninterpretable observation.
- **FR-16:** Make persistent monitoring failures visible to users.
- **FR-17:** Handle timeouts, access errors, redirects, and rate limits as monitoring outcomes, not automatically as relevant content changes.
- **FR-18:** Do not bypass authentication, CAPTCHAs, explicit restrictions, or anti-bot protections.

## 5. Preliminary non-functional requirements

These are assumptions and targets to refine, not verified system capabilities.

- **Initial scale assumption:** approximately 10,000 registered users; define a credible growth path.
- **Limit:** 20 active monitors per free user.
- **Timeliness:** non-real-time. Initial target: 95% of successfully accessible monitors checked within their scheduled 15-minute window. Measurement boundaries and exclusions require further definition.
- **Observation limits:** changes appearing and disappearing between observations may be missed; no guarantee when a site blocks or prevents access.
- **Reliability:** avoid silently discarding known relevant changes under recoverable failures, and prevent duplicate user-facing notifications.
- **Security:** treat submitted URLs and retrieved webpage content as untrusted. Do not allow unauthorized access to internal or infrastructure resources.
- **Responsible access:** respect site access controls, applicable policies, and rate limits; do not circumvent blocking.

## 6. Scope and non-goals

For the initial version:

- One configured URL per monitor, not full-domain crawling.
- Public, non-authenticated webpages only.
- No real-time sports-score style monitoring.
- No CAPTCHA or anti-bot bypass.
- No SMS, mobile push, Slack, or Teams integration initially.
- No automatic purchasing or other consequential actions triggered by detection.
- No promise to detect every transient change.

## 7. Product open questions

1. Which two or three change types/use cases should be in the **first usable release**?
2. Should JavaScript-rendered public pages be supported in v1? What should users see if a URL is unsupported?
3. Can users configure monitoring frequency, or is it fixed initially?
4. What email-delivery expectations should users have when delivery fails or is delayed?
5. How should uncertain results and prolonged unhealthy monitors be represented?
6. On the first observation, should a condition that is already satisfied generate an alert?
7. How should we communicate sites that cannot be monitored because of access restrictions?

## 8. Initial success criteria

An early usable version lets a user register a public URL and natural-language intent, receive understandable notifications when relevant observed changes occur, inspect change history, and distinguish healthy monitors from ones that cannot be checked.

Product quality measures (including precision/recall of relevant changes and notification usefulness) will be defined and baselined during evaluation planning.

---

**Boundary:** This document states product behavior and constraints. Snapshot/storage formats, fetching strategy, model/rule selection, scheduling architecture, and other implementation decisions belong in engineering designs and architecture decision records (ADRs).
