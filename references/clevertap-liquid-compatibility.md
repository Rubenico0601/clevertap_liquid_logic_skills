# CleverTap Liquid Compatibility Reference

This document is the technical compatibility knowledge base for the CleverTap Liquid Logic Generator & Validator skill.

The SKILL.md file defines how the skill behaves.

This file defines what Liquid functionality the skill is permitted to generate.

The skill must treat this document as the authoritative compatibility layer for the target CleverTap Liquid implementation.

When this document conflicts with generic Shopify Liquid knowledge, this document takes precedence.

When this document conflicts with current official CleverTap documentation, the official CleverTap documentation must be checked and this reference should be updated accordingly.

Official CleverTap documentation:
https://docs.clevertap.com/docs/liquid-tags

---

## 1. Compatibility target

Target engine:

LiqP 0.7.9

The skill must generate only Liquid that is compatible with the CleverTap implementation represented by this compatibility reference.

Do not assume that all Shopify Liquid functionality is available.

CleverTap explicitly states that its Liquid terms are adapted from Shopify and that only the terms identified in CleverTap's documentation are supported.

---

## 2. Variable namespaces

### Profile

Profile properties use:

{{ Profile.PropertyName }}

The `P` in `Profile` must be capitalized and must be followed by a period.

Examples:

{{ Profile.FirstName }}

{{ Profile.Language }}

{{ Profile.Membership }}

Never invent a Profile property.

The exact property name must be supplied by the user, confirmed by the user, or verified from available CleverTap data/context.

---

### Event

Event properties use:

{{ Event.PropertyName }}

The `E` in `Event` must be capitalized and must be followed by a period.

Examples:

{{ Event.Amount }}

{{ Event.Product }}

For event properties containing spaces or special characters, bracket notation may be used:

{{ Event["Requested Product"] }}

Event-property Liquid is context-dependent. CleverTap documentation states that Liquid tags for Event properties are available for live user segments.

Do not assume an Event property can be used in every campaign context.

---

## 3. Output tags

Supported output syntax:

{{ expression }}

Examples:

{{ Profile.FirstName }}

{{ Profile.Membership }}

{{ Event.Amount }}

Output tags may contain supported filters.

Example:

{{ Profile.Name | default: "Customer" }}

Only use filters confirmed elsewhere in this reference.

---

## 4. Conditional tags

The following conditional/control tags are documented by CleverTap:

- if
- else
- elsif
- unless
- case
- when
- for
- break
- split
- continue
- abort
- limit
- offset
- tablerow
- now

Use the exact CleverTap syntax.

---

## 5. if

Supported:

{% if condition %}
...
{% endif %}

Example:

{% if Profile.Language == "French" %}
Bonjour!
{% endif %}

---

## 6. elsif

Supported:

{% if condition %}
...
{% elsif condition %}
...
{% else %}
...
{% endif %}

Use `elsif`, not `elseif`, unless current CleverTap documentation explicitly documents otherwise.

---

## 7. else

Supported:

{% else %}

Example:

{% if Profile.Membership == "Gold" %}
20%
{% else %}
5%
{% endif %}

---

## 8. unless

Supported:

{% unless condition %}
...
{% endunless %}

Example:

{% unless Profile.Language == "French" %}
Hello!
{% endunless %}

---

## 9. case / when

Supported:

{% case variable %}
{% when "value" %}
...
{% when "another value" %}
...
{% else %}
...
{% endcase %}

Example:

{% case Profile.Membership %}
{% when "Gold" %}
20%
{% when "Silver" %}
10%
{% else %}
5%
{% endcase %}

---

## 10. for

Supported:

{% for item in collection %}
...
{% endfor %}

Example:

{% for item in Profile.Items %}
{{ item }}
{% endfor %}

Do not assume that every collection or object can be iterated. Verify the property structure and CleverTap support.

---

## 11. break

Supported inside loops:

{% break %}

Example:

{% for i in (1..10) %}
{% if i == 7 %}
{% break %}
{% endif %}
{% endfor %}

---

## 12. continue

Supported inside loops:

{% continue %}

Example:

{% for i in (1..10) %}
{% if i == 7 %}
{% continue %}
{% endif %}
{{ i }}
{% endfor %}

---

## 13. split

Supported:

{% assign items = "A, B, C" | split: ", " %}

This converts a delimited string into an array.

---

## 14. abort

CleverTap supports:

{% abort %}

This can abort sending a message when a condition is not fulfilled.

Example:

{% if Event.price > 100 %}
Coupon
{% else %}
{% abort %}
{% endif %}

The skill must not use `abort` as generic error handling.

Use it only when the business requirement actually calls for suppressing the message.

---

## 15. limit

Supported as a `for` loop parameter:

{% for item in array limit:2 %}
...
{% endfor %}

---

## 16. offset

Supported as a `for` loop parameter:

{% for item in array offset:2 %}
...
{% endfor %}

---

## 17. tablerow

CleverTap documents:

{% tablerow item in collection %}
...
{% endtablerow %}

This must be used within an HTML `<table>` structure.

---

## 18. now

CleverTap supports the `now` value through the date filter.

Example:

{{ "now" | date: "%Y-%m-%d %H:%M" }}

CleverTap documentation states that `now` returns the current date/time in IST (UTC+5:30) by default.

Do not assume `now` represents the recipient's local timezone.

---

## 19. date

CleverTap documents the `date` filter.

Example:

{{ "now" | date: "%Y-%m-%d %H:%M" }}

The format string follows the documented date formatting syntax.

Do not invent date functions such as:

date_diff

to_epoch

days_between

unless explicitly confirmed by CleverTap documentation.

---

## 20. date_tz

CleverTap documents:

{{ "now" | date_tz: "%Y-%m-%d %H:%M:%S", "Asia/Singapore" }}

The syntax is:

{{ "now" | date_tz: "<format>", "<timezone>" }}

The timezone must be a valid IANA timezone.

Examples:

Asia/Singapore

Europe/London

America/New_York

Australia/Sydney

Pacific/Auckland

Use `date_tz` when the requirement specifically requires a timezone other than the default server/IST behavior.

---

## 21. assign

Supported:

{% assign variable = expression %}

Example:

{% assign membership = Profile.Membership %}

Assignments may be used to make complex logic more readable.

Do not create unnecessary assignments.

---

## 22. raw

Supported:

{% raw %}
...
{% endraw %}

Use this when Liquid syntax itself needs to be displayed as text.

---

## 23. comment

Supported:

{% comment %}
...
{% endcomment %}

Comments should not be used to hide unsupported functionality.

---

## 24. Operators

The following operators are documented by CleverTap:

| Operator | Meaning |
|---|---|
| == | equals |
| != | does not equal |
| > | greater than |
| < | less than |
| >= | greater than or equal to |
| <= | less than or equal to |
| and | logical AND |
| or | logical OR |
| contains | checks whether a string contains another string |

Do not generate programming-language operators such as:

===

&&

||

!

unless explicitly confirmed by current CleverTap documentation.

---

## 25. String comparisons

Use:

{% if Profile.Membership == "Gold" %}

Do not use JavaScript-style equality operators.

---

## 26. Numeric comparisons

Supported comparison operators include:

>

<

>=

<=

==

!=

Example:

{% if Profile.PurchaseCount > 5 %}
...
{% endif %}

The skill must confirm that the underlying property contains a numeric value when numeric comparison is required.

---

## 27. and / or

Supported:

{% if Profile.Membership == "Gold" and Profile.PurchaseCount > 5 %}
...
{% endif %}

Supported:

{% if Profile.Membership == "Gold" or Profile.Membership == "Silver" %}
...
{% endif %}

Do not replace these with `&&` or `||`.

---

## 28. contains

Supported:

{% if Profile.Country contains "India" %}
...
{% endif %}

Use only for the documented string-containment behavior.

Do not assume `contains` performs arbitrary array/object membership checks without verifying current CleverTap documentation.

---

## 29. Nested objects

CleverTap supports nested Profile and Event personalization.

The documented syntax includes:

{{ Profile.<property_name>.<level_1>.<level_2>.<level_3> }}

and:

{{ Event.<property_name>.<level_1>.<level_2>.<level_3> }}

CleverTap currently documents nested personalization up to 3 levels.

Example:

{{ Profile.subscription.plan.name | default: "valued" }}

Nested personalization requires the relevant CleverTap account capability/configuration.

The skill must not assume nested object personalization is enabled for every account.

---

## 30. Nested Event properties

Nested Event properties follow the same general nested-property syntax:

{{ Event.<property_name>.<level_1>.<level_2>.<level_3> }}

However, Event-property Liquid remains subject to campaign/segment context restrictions.

---

## 31. Arrays and indexing

CleverTap documents array access in nested/event personalization examples.

Example:

{{ Event.Trip[0].Travelers[1].Email | default: "Hey there" }}

Indexes start at 0.

An invalid array index can prevent campaign delivery.

Therefore, the skill must be conservative when generating array indexes.

Never invent an index without understanding the expected array structure.

---

## 32. Default values

The `default` filter is documented in CleverTap examples.

Example:

{{ Profile.Name | default: "Customer" }}

Use `default` where a fallback value is appropriate.

Do not use `default` to conceal a missing property when the customer needs to know that the property itself is incorrect or unavailable.

---

## 33. Date property behavior

CleverTap transforms date properties for Liquid personalization.

The skill must not assume that a date property arrives in exactly the same representation as the underlying stored profile/event data.

CleverTap documentation provides examples where a date property is rendered for Liquid as:

YYYY-MM-DD HH:MM:SS

Therefore, before implementing date arithmetic, confirm the actual behavior and data representation relevant to the customer's property and campaign.

---

## 34. Epoch handling

Do not assume CleverTap Liquid provides a generic epoch conversion function.

Never generate:

{{ Profile.DueDate | to_epoch }}

unless current CleverTap documentation explicitly confirms it.

Never generate:

{{ Profile.DueDate | date_diff: "now" }}

unless explicitly documented.

For an epoch requirement, first determine:

1. What the source property contains.
2. Whether the source is seconds or milliseconds.
3. Whether CleverTap transforms the value before Liquid processing.
4. Whether the required arithmetic is supported.
5. Whether the desired result can be produced with supported operations.

If the calculation cannot be reliably performed in CleverTap Liquid, recommend precomputing the value upstream.

---

## 35. Date difference

No undocumented `date_diff` capability should be assumed.

For requirements such as:

"days until due date"

the skill must establish whether the calculation can actually be performed with documented CleverTap functionality.

If it cannot, do not fabricate a date-difference filter.

---

## 36. Unsupported Shopify functionality

The following principle is mandatory:

Shopify Liquid support does not automatically mean CleverTap support.

Do not generate Shopify-only or undocumented constructs.

Examples of functionality that must NOT be assumed:

- Generic Shopify-specific filters
- Undocumented date arithmetic
- Undocumented array functions
- Undocumented mathematical functions
- Undocumented string functions
- Undocumented object functions
- Undocumented custom filters
- Undocumented functions such as `to_epoch`
- Undocumented functions such as `date_diff`

If a construct is not in this compatibility reference or current CleverTap documentation, treat it as unsupported until verified.

---

## 37. Unsupported programming syntax

Never generate:

===

!==

&&

||

!

unless current CleverTap documentation explicitly confirms support.

Prefer CleverTap-supported Liquid operators.

---

## 38. Campaign-context limitations

Liquid functionality is not necessarily universal across all CleverTap campaign surfaces.

The skill must consider whether the requested Liquid is being used in:

- Email
- Mobile Push
- SMS
- Webhook
- App Inbox
- In-App
- Native Display
- WhatsApp
- Other CleverTap-supported surfaces

Event-property personalization has documented context restrictions.

Nested personalization also has documented channel/configuration restrictions.

Do not claim universal support when the documentation does not establish it.

---

## 39. HTML + Liquid

CleverTap supports combining HTML with Liquid, particularly in the documented email HTML workflow.

Example:

<a href="{{ Profile.DeepLink }}">Open</a>

When validating HTML containing Liquid:

- Validate HTML syntax.
- Validate Liquid syntax.
- Validate quotes.
- Validate Liquid delimiters.
- Preserve valid customer HTML.

---

## 40. Validation

CleverTap provides validation of Liquid syntax in the campaign editor and Preview & Test functionality.

The skill should distinguish:

Static compatibility analysis

from:

Actual CleverTap execution.

The skill must never claim that generated Liquid was executed in CleverTap unless the user actually provides evidence of execution.

---

## 41. Compatibility decision rules

When evaluating a construct:

### Supported

Generate it when:

- It is explicitly documented by CleverTap.
- It matches the syntax documented by CleverTap.
- Its campaign context is appropriate.

### Unsupported

Do not generate it when:

- It exists only in generic Shopify Liquid.
- It is undocumented by CleverTap.
- It requires an undocumented filter/function/operator.
- It conflicts with a documented CleverTap limitation.

### Uncertain

If support cannot be established:

1. Check the current CleverTap documentation.
2. Check this compatibility reference.
3. Do not guess.
4. Tell the user that compatibility could not be verified.

---

## 42. Source of truth hierarchy

Use this order:

1. Current official CleverTap documentation
2. This compatibility reference
3. Verified CleverTap examples
4. Generic Liquid knowledge

Generic Shopify documentation must never override current CleverTap documentation.

---

## 43. Update policy

When CleverTap introduces or removes Liquid functionality:

1. Verify the change against official CleverTap documentation.
2. Update this compatibility reference.
3. Record important limitations or channel restrictions.
4. Do not blindly assume the new functionality is supported across every campaign surface.
5. Keep SKILL.md focused on behavior rather than duplicating the complete compatibility matrix.

---

## 44. Final compatibility rule

A Liquid expression should be considered production-ready by this skill only when it is:

- Syntactically valid.
- Logically correct.
- Compatible with documented CleverTap Liquid behavior.
- Appropriate for the campaign context.
- Based on real/confirmed Profile or Event properties.
- Free of invented CleverTap functionality.
