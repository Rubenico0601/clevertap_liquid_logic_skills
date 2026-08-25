# CleverTap Liquid Compatibility Reference

This document is the technical compatibility knowledge base for the CleverTap Liquid Logic Generator & Validator skill.

`SKILL.md` defines how the skill behaves.

This file defines what Liquid functionality the skill is permitted to generate.

The skill must treat this document as the authoritative compatibility layer for the target CleverTap Liquid implementation, subject to verification against the current official CleverTap documentation.

When this document conflicts with generic Shopify Liquid knowledge, this document takes precedence only after the current official CleverTap documentation has been checked.

When this document conflicts with current official CleverTap documentation, the official CleverTap documentation takes precedence and this reference must be updated.

Official CleverTap Liquid documentation:

[https://docs.clevertap.com/docs/liquid-tags](https://docs.clevertap.com/docs/liquid-tags)

Official CleverTap nested-object documentation:

[https://docs.clevertap.com/docs/nested-objects-in-user-properties](https://docs.clevertap.com/docs/nested-objects-in-user-properties)

---

## 1. Compatibility target

Target engine:

**LiqP 0.7.9**

The skill must generate only Liquid that is compatible with the CleverTap implementation represented by this compatibility reference.

Do not assume that all Shopify Liquid functionality is available.

CleverTap explicitly states that its Liquid terms are adapted from Shopify and that only the terms identified in CleverTap's documentation are supported. ([CleverTap User Docs][1])

Generic Shopify Liquid behavior must never override documented CleverTap behavior.

---

## 2. Compatibility classification

Every Liquid construct should be classified internally as one of:

### Confirmed supported

The construct is explicitly documented by CleverTap and the syntax is known.

### Conditionally supported

The construct is documented but depends on:

* Campaign channel.
* Segment type.
* Account configuration.
* Property structure.
* Data type.
* Other documented limitations.

The skill may generate it only when the relevant conditions are satisfied or clearly stated.

### Unsupported

The construct is explicitly unsupported or conflicts with documented CleverTap behavior.

The skill must not generate it.

### Unknown / not documented

The skill cannot establish support from current CleverTap documentation or verified compatibility evidence.

Treat this as **unknown**, not supported.

The skill must not present unknown functionality as production-ready.

---

## 3. Source-of-truth hierarchy

Use this order when determining compatibility:

1. Current official CleverTap documentation.
2. This compatibility reference.
3. Verified CleverTap examples.
4. Verified LiqP 0.7.9 implementation evidence.
5. Generic Liquid knowledge.

Generic Shopify documentation must never override current CleverTap documentation.

If current official CleverTap documentation changes, this reference should be updated.

---

# 4. Engagement channels

CleverTap currently documents Liquid Tags for:

* Email
* Mobile Push
* SMS
* Webhooks
* App Inbox
* In-App
* Native Display
* WhatsApp

Liquid availability alone does not mean that every Liquid feature is available in every channel. ([CleverTap User Docs][1])

Always evaluate feature-specific channel restrictions separately.

---

# 5. Variable namespaces

## Profile

Profile properties use:

```liquid
{{ Profile.PropertyName }}
```

The `P` in `Profile` must be capitalized and must be followed by a period.

Examples:

```liquid
{{ Profile.FirstName }}
{{ Profile.Language }}
{{ Profile.Membership }}
```

Profile properties are available for all segments according to CleverTap documentation. ([CleverTap User Docs][1])

Never invent a Profile property.

The exact property name must be:

* Supplied by the user.
* Confirmed by the user.
* Or verified from available CleverTap data/context.

---

## Event

Event properties use:

```liquid
{{ Event.PropertyName }}
```

The `E` in `Event` must be capitalized and must be followed by a period.

Examples:

```liquid
{{ Event.Amount }}
{{ Event.Product }}
```

For event properties containing spaces or special characters, bracket notation may be used:

```liquid
{{ Event["Requested Product"] }}
```

CleverTap documents Event-property Liquid as being available for live user segments. Event properties therefore must not be assumed to work universally across every campaign context. ([CleverTap User Docs][1])

Never invent an Event property.

---

## Other CleverTap variables

Do not assume that Profile and Event are the only possible Liquid variables.

If the user requests another CleverTap-specific personalization variable:

1. Check this reference.
2. Check current official CleverTap documentation.
3. Verify the relevant campaign context.
4. Use the exact documented syntax.

Never invent context-specific variables.

---

# 6. Output tags

Supported output syntax:

```liquid
{{ expression }}
```

Examples:

```liquid
{{ Profile.FirstName }}
{{ Profile.Membership }}
{{ Event.Amount }}
```

Output tags may contain supported filters.

Example:

```liquid
{{ Profile.Name | default: "Customer" }}
```

Only use filters explicitly documented in this reference or current CleverTap documentation.

---

# 7. Conditional tags

CleverTap currently documents the following conditional/control functionality:

* `if`
* `else`
* `elsif`
* `unless`
* `case`
* `when`
* `for`
* `break`
* `split`
* `continue`
* `abort`
* `limit`
* `offset`
* `tablerow`
* `now`

CleverTap's documentation describes the conditional feature as "Else/Elseif", but the actual documented syntax is:

```liquid
{% elsif condition %}
```

Therefore use `elsif`, not `elseif`. ([CleverTap User Docs][1])

---

# 8. if

Supported:

```liquid
{% if condition %}
...
{% endif %}
```

Example:

```liquid
{% if Profile.Language == "French" %}
Bonjour!
{% endif %}
```

---

# 9. elsif

Supported:

```liquid
{% if condition %}
...
{% elsif condition %}
...
{% else %}
...
{% endif %}
```

Example:

```liquid
{% if Profile.Language == "French" %}
Bonjour!
{% elsif Profile.Language == "Spanish" %}
Hola!
{% else %}
Hello!
{% endif %}
```

Use `elsif`.

Do not generate `elseif` unless future official CleverTap documentation explicitly documents that syntax.

---

# 10. else

Supported:

```liquid
{% else %}
```

Example:

```liquid
{% if Profile.Membership == "Gold" %}
20%
{% else %}
5%
{% endif %}
```

---

# 11. unless

Supported:

```liquid
{% unless condition %}
...
{% endunless %}
```

Example:

```liquid
{% unless Profile.Language == "French" %}
Hello!
{% endunless %}
```

---

# 12. case / when

CleverTap documents switch-case functionality using:

```liquid
{% case variable %}
{% when "value" %}
...
{% when "another value" %}
...
{% else %}
...
{% endcase %}
```

Example:

```liquid
{% case Profile.Membership %}
{% when "Gold" %}
20%
{% when "Silver" %}
10%
{% else %}
5%
{% endcase %}
```

Use `case/when` when multiple branches are primarily based on the value of the same variable.

Do not use `case/when` when the business logic requires unrelated compound conditions unless the resulting logic remains supported and clear.

---

# 13. for

Supported:

```liquid
{% for item in collection %}
...
{% endfor %}
```

Example:

```liquid
{% for item in Profile.Items %}
{{ item }}
{% endfor %}
```

CleverTap documents looping over collections. ([CleverTap User Docs][1])

Do not assume that every object or property can be iterated.

Verify:

* The property exists.
* The property is a collection/array suitable for iteration.
* The relevant structure is supported.

---

# 14. break

Supported inside loops:

```liquid
{% break %}
```

Example:

```liquid
{% for i in (1..10) %}
  {% if i == 7 %}
    {% break %}
  {% endif %}
{% endfor %}
```

CleverTap documents `break` for stopping loop execution. ([CleverTap User Docs][1])

---

# 15. continue

Supported inside loops:

```liquid
{% continue %}
```

Example:

```liquid
{% for i in (1..10) %}
  {% if i == 7 %}
    {% continue %}
  {% endif %}
  {{ i }}
{% endfor %}
```

CleverTap documents `continue` for skipping the current loop iteration. ([CleverTap User Docs][1])

---

# 16. split

Supported:

```liquid
{% assign items = "A, B, C" | split: ", " %}
```

`split` converts a delimited string into an array.

Example:

```liquid
{% assign items = "A, B, C" | split: ", " %}

{% for item in items %}
{{ item }}
{% endfor %}
```

CleverTap documents this behavior. ([CleverTap User Docs][1])

---

# 17. abort

CleverTap supports:

```liquid
{% abort %}
```

`abort` can suppress message delivery when a condition is not fulfilled. CleverTap reports such cases as `LiquidLogicAborted`. ([CleverTap User Docs][1])

Example:

```liquid
{% if Event.price > 100 %}
Coupon
{% else %}
{% abort %}
{% endif %}
```

Use `abort` only when the business requirement is to suppress the message.

Do not use `abort` as generic error handling.

---

# 18. limit

Supported as a `for` loop parameter:

```liquid
{% for item in array limit:2 %}
...
{% endfor %}
```

Example:

```liquid
{% for item in array limit:2 %}
{{ item }}
{% endfor %}
```

CleverTap documents `limit` as a loop parameter. ([CleverTap User Docs][1])

---

# 19. offset

Supported as a `for` loop parameter:

```liquid
{% for item in array offset:2 %}
...
{% endfor %}
```

Example:

```liquid
{% for item in array offset:2 %}
{{ item }}
{% endfor %}
```

CleverTap documents `offset` as a loop parameter. ([CleverTap User Docs][1])

---

# 20. tablerow

CleverTap documents:

```liquid
{% tablerow item in collection %}
...
{% endtablerow %}
```

It must be wrapped in an HTML `<table>` structure.

Example:

```liquid
<table>
{% tablerow product in Profile.multi %}
{{ product | default: "def" }}
{% endtablerow %}
</table>
```

CleverTap documents this behavior specifically for generating HTML tables. ([CleverTap User Docs][1])

---

# 21. now

CleverTap supports the `now` value through the date filter.

Example:

```liquid
{{ "now" | date: "%Y-%m-%d %H:%M" }}
```

CleverTap documents that `now` returns the current date/time in IST (UTC+5:30) by default. ([CleverTap User Docs][1])

Important:

`now` must **not** be interpreted as the recipient's local timezone.

If the requirement explicitly needs a particular timezone, use `date_tz` when supported.

---

# 22. date

CleverTap documents the `date` filter.

Example:

```liquid
{{ "now" | date: "%Y-%m-%d %H:%M" }}
```

Use only documented date formatting syntax.

Do not invent date functions such as:

```text
date_diff
to_epoch
days_between
```

unless current CleverTap documentation explicitly confirms them.

Date formatting is not the same as date arithmetic.

---

# 23. date_tz

CleverTap documents the `date_tz` filter:

```liquid
{{ "now" | date_tz: "%Y-%m-%d %H:%M:%S", "Asia/Singapore" }}
```

Syntax:

```liquid
{{ "now" | date_tz: "<format>", "<timezone>" }}
```

The timezone must be a valid IANA timezone.

Examples:

```text
Asia/Singapore
Europe/London
America/New_York
Australia/Sydney
Pacific/Auckland
```

CleverTap states that DST adjustments are handled automatically for applicable timezones. The current documentation states that `date_tz` is supported across:

* Email
* Mobile Push
* SMS
* Webhooks
* App Inbox
* In-App
* WhatsApp

([CleverTap User Docs][1])

Use `date_tz` when the requirement requires a specific timezone.

Do not assume that `now` automatically represents recipient-local time.

---

# 24. assign

Supported:

```liquid
{% assign variable = expression %}
```

Example:

```liquid
{% assign membership = Profile.Membership %}
```

Assignments can improve readability and allow intermediate values to be reused.

CleverTap documents `assign` as a variable tag. ([CleverTap User Docs][1])

Do not create unnecessary assignments for trivial expressions.

---

# 25. raw

Supported:

```liquid
{% raw %}
...
{% endraw %}
```

Use `raw` when Liquid syntax itself needs to be displayed as text.

Example:

```liquid
{% raw %}{{ 5 | plus: 6 }}{% endraw %}
```

CleverTap documents this behavior. ([CleverTap User Docs][1])

---

# 26. comment

Supported:

```liquid
{% comment %}
...
{% endcomment %}
```

Use comments for explanatory notes.

Do not use comments to conceal unsupported functionality.

---

# 27. Operators

CleverTap currently documents the following operators:

| Operator   | Meaning                             |
| ---------- | ----------------------------------- |
| `==`       | equals                              |
| `!=`       | does not equal                      |
| `>`        | greater than                        |
| `<`        | less than                           |
| `>=`       | greater than or equal to            |
| `<=`       | less than or equal to               |
| `and`      | logical AND                         |
| `or`       | logical OR                          |
| `contains` | checks for a string within a string |

([CleverTap User Docs][1])

Do not generate programming-language operators such as:

```text
===
!==
&&
||
!
```

unless current official CleverTap documentation explicitly confirms them.

---

# 28. String comparisons

Use:

```liquid
{% if Profile.Membership == "Gold" %}
```

Do not use JavaScript-style equality operators.

---

# 29. Numeric comparisons

Supported comparison operators include:

```text
>
<
>=
<=
==
!=
```

Example:

```liquid
{% if Profile.PurchaseCount > 5 %}
...
{% endif %}
```

The underlying property must actually contain an appropriate numeric value when numeric comparison is required.

Do not assume a string containing a number will behave identically to a numeric property.

---

# 30. and / or

Supported:

```liquid
{% if Profile.Membership == "Gold" and Profile.PurchaseCount > 5 %}
...
{% endif %}
```

Supported:

```liquid
{% if Profile.Membership == "Gold" or Profile.Membership == "Silver" %}
...
{% endif %}
```

Do not replace these with:

```text
&&
||
```

---

# 31. contains

Supported for string containment:

```liquid
{% if Profile.Country contains "India" %}
...
{% endif %}
```

CleverTap documents `contains` as checking for a string within a string. ([CleverTap User Docs][1])

Do not assume `contains` provides arbitrary array/object membership functionality unless current CleverTap documentation explicitly establishes that behavior.

---

# 32. Nested Object Personalization

CleverTap supports nested Profile and Event personalization.

Nested personalization must be enabled for the account. ([CleverTap User Docs][1])

Supported syntax:

```liquid
{{ Profile.<property_name>.<level_1>.<level_2>.<level_3> }}
```

and:

```liquid
{{ Event.<property_name>.<level_1>.<level_2>.<level_3> }}
```

CleverTap currently documents nested personalization up to **3 levels**. ([CleverTap User Docs][1])

Example:

```liquid
{{ Profile.subscription.plan.name | default: "valued" }}
```

The skill must not generate nested personalization beyond the documented depth.

---

# 33. Nested Object Supported Channels

CleverTap's current Liquid documentation lists Nested Personalization support for:

* Email
* Mobile Push
* SMS
* Webhooks
* WhatsApp

([CleverTap User Docs][1])

A separate current CleverTap nested-user-properties page also describes nested Liquid personalization across multiple channels, including Push Notifications, Emails, In-App messages, SMS, WhatsApp, and Webhooks. ([CleverTap User Docs][2])

Because the dedicated Liquid Tags page currently gives the more restrictive explicit channel list, the skill should treat channel-specific support as requiring verification when the requested channel is not explicitly established.

In particular, do not blindly claim that every nested-object Liquid feature is universally available across every CleverTap surface.

When there is a channel discrepancy between CleverTap documentation pages:

1. Check the current documentation.
2. Prefer the most specific documentation for the feature.
3. State the uncertainty if it cannot be resolved.
4. Do not present the feature as universally supported.

---

# 34. Nested Object System Limits

CleverTap currently documents the following nested-object ingestion limits:

| Limit                                      |  Maximum |
| ------------------------------------------ | -------: |
| Nesting depth                              | 3 levels |
| Root-level object/array keys               |        5 |
| Nested object/array keys per nesting depth |        5 |
| Nested elements inside an array            |      100 |
| Nested keys inside an object               |      100 |

([CleverTap User Docs][2])

The skill should consider these limits when troubleshooting nested-property issues.

A Liquid expression can be syntactically valid while the underlying nested data structure is incompatible with CleverTap's documented limits.

Do not confuse:

**Liquid syntax support**

with:

**Nested-property ingestion limits.**

---

# 35. Nested Object account configuration

Nested Object Personalization must be enabled for the CleverTap account before nested personalization can be used. ([CleverTap User Docs][1])

If nested personalization is not resolving:

1. Verify the property exists.
2. Verify the nested structure.
3. Verify account capability/configuration.
4. Verify nesting depth.
5. Verify the campaign context.
6. Use Preview & Test where applicable.

CleverTap specifically instructs users to use Preview & Test to validate nested personalization before publishing. ([CleverTap User Docs][1])

---

# 36. Nested Profile properties

Example:

```liquid
{{ Profile.subscription.plan.name | default: "valued" }}
```

Before generating nested Profile Liquid:

* Confirm the root property.
* Confirm the nested path.
* Confirm the property structure.
* Confirm account support.
* Confirm nesting depth.
* Confirm channel/context.

Never invent nested paths.

---

# 37. Nested Event properties

Nested Event properties follow:

```liquid
{{ Event.<property_name>.<level_1>.<level_2>.<level_3> }}
```

Event-property Liquid remains subject to Event-property context restrictions.

Never assume nested Event properties can be used in every campaign context.

---

# 38. Arrays and indexing

CleverTap documents array access in nested personalization examples.

Example:

```liquid
{{ Event.Trip[0].Travelers[1].Email | default: "Hey there" }}
```

Indexes start at `0`.

Array indexing must be treated as potentially unsafe.

Before generating an index:

1. Confirm the property is actually an array.
2. Confirm the expected structure.
3. Confirm the required index.
4. Consider whether the index can be absent.
5. Consider whether an empty or shorter array is possible.

An invalid array index can prevent campaign delivery according to CleverTap documentation. Therefore, the skill must be conservative when generating array indexes.

Never invent an array index merely because the syntax is valid.

---

# 39. Default values

The `default` filter is documented by CleverTap.

Example:

```liquid
{{ Profile.Name | default: "Customer" }}
```

Use `default` when a fallback output is appropriate.

Do not use `default` to conceal a missing or incorrectly named property when the user needs to diagnose the underlying data issue.

A fallback value does not prove that the referenced property exists.

---

# 40. Date property behavior

CleverTap transforms date properties for Liquid personalization.

The skill must not assume that a date property arrives in exactly the same representation as its underlying stored profile/event data.

CleverTap documentation provides examples where date properties are rendered for Liquid as:

```text
YYYY-MM-DD HH:MM:SS
```

Therefore, before implementing date arithmetic or comparisons, establish the actual representation relevant to the property and campaign.

Do not infer epoch seconds or milliseconds merely from the fact that a property represents a date.

---

# 41. Epoch handling

Do not assume CleverTap Liquid provides generic epoch conversion.

Never generate:

```liquid
{{ Profile.DueDate | to_epoch }}
```

unless current official CleverTap documentation explicitly confirms it.

Never generate:

```liquid
{{ Profile.DueDate | date_diff: "now" }}
```

unless explicitly documented.

For an epoch-related requirement, determine:

1. What the source property contains.
2. Whether the source is seconds or milliseconds.
3. Whether CleverTap transforms the value before Liquid processing.
4. Whether the required arithmetic is supported.
5. Whether the desired output can be produced with documented functionality.

If the calculation cannot be reliably performed in CleverTap Liquid, recommend precomputing the value upstream.

---

# 42. Date difference

No undocumented `date_diff` capability should be assumed.

For requirements such as:

```text
days until due date
```

the skill must determine whether the required calculation can actually be performed using documented CleverTap functionality.

If it cannot:

* Do not fabricate a date-difference filter.
* Explain the limitation.
* Provide the closest supported implementation where possible.
* Suggest upstream precomputation when appropriate.

---

# 43. Date/timezone rules

For any date/time requirement, determine when materially relevant:

* Source property.
* Profile/Event/other source.
* Data type.
* Date representation.
* Epoch unit.
* Timezone.
* Date-only vs timestamp comparison.
* Rounding behavior.
* Future-date behavior.
* Today behavior.
* Past-date behavior.
* Missing-value behavior.

`now` uses IST by default.

`date_tz` should be used when a specific IANA timezone is required.

Do not assume recipient-local timezone without an explicit requirement and supported implementation.

---

# 44. Arithmetic

Only use arithmetic functionality that is confirmed by current CleverTap documentation or this compatibility reference.

Do not assume every arithmetic filter/function available in generic Shopify Liquid is supported.

For example, do not introduce undocumented date arithmetic merely because it is available in another Liquid implementation.

If an arithmetic operation cannot be verified:

* Treat it as unknown.
* Do not present it as production-ready.
* Seek current official CleverTap documentation.
* Recommend upstream computation if necessary.

---

# 45. Unsupported Shopify functionality

The following principle is mandatory:

**Shopify Liquid support does not automatically mean CleverTap support.**

Do not generate Shopify-only or undocumented constructs.

Examples of functionality that must not be assumed:

* Generic Shopify-specific filters.
* Undocumented date arithmetic.
* Undocumented array functions.
* Undocumented mathematical functions.
* Undocumented string functions.
* Undocumented object functions.
* Undocumented custom filters.
* `to_epoch`.
* `date_diff`.
* `days_between`.
* Any other undocumented custom function.

If a construct is not documented by CleverTap or otherwise verified against the compatibility target, treat it as unsupported or unknown.

---

# 46. Unsupported programming syntax

Never generate:

```text
===
!==
&&
||
!
```

unless current official CleverTap documentation explicitly confirms support.

Use the documented CleverTap operators instead.

---

# 47. Campaign-context limitations

Liquid Tags themselves are documented across multiple CleverTap engagement channels, but individual Liquid capabilities may have narrower restrictions. ([CleverTap User Docs][1])

The skill must consider whether Liquid is being used in:

* Email
* Mobile Push
* SMS
* Webhooks
* App Inbox
* In-App
* Native Display
* WhatsApp

Special attention must be given to:

* Event properties.
* Nested Profile properties.
* Nested Event properties.
* HTML.
* `date_tz`.
* Channel-specific editor behavior.

Never claim universal support for a feature when the documentation does not establish it.

---

# 48. Event-property context

CleverTap states that Profile properties are available for all segments, while Liquid Tags for Event properties are available for live user segments. ([CleverTap User Docs][1])

Therefore, if Event properties are used:

1. Identify the Event property.
2. Confirm the campaign/segment context.
3. Confirm that Event-property Liquid is supported there.
4. Do not treat Event properties as universally available personalization variables.

---

# 49. HTML + Liquid

The current CleverTap documentation explicitly states that the HTML + Liquid workflow described in the Liquid Tags documentation applies **only to the Email channel**. ([CleverTap User Docs][1])

Therefore:

* Do not claim generic HTML/Liquid support across all channels.
* For Email, validate both HTML and Liquid.
* Preserve valid customer HTML wherever possible.
* Validate Liquid inside HTML attributes.
* Validate quotes and delimiters.
* Ensure Liquid does not break the HTML structure.

Example:

```html
<a href="{{ Profile.DeepLink }}">Open</a>
```

should not be rewritten unnecessarily if the Liquid is already correct.

---

# 50. HTML validation

When validating Email HTML containing Liquid:

Check:

* HTML structure.
* Liquid delimiters.
* Quotes.
* Attribute boundaries.
* Liquid expressions.
* Conditional blocks.
* Final rendered structure.

Do not make unrelated design changes.

The skill should make the smallest necessary correction.

---

# 51. Validation

CleverTap provides syntax validation in the campaign editor and Preview & Test functionality. Current documentation states that Liquid syntax errors can surface during campaign creation and that Preview & Test can be used to validate personalized values and send test campaigns. ([CleverTap User Docs][1])

The skill must distinguish:

**Static compatibility analysis**

from:

**Actual CleverTap execution.**

Never claim that generated Liquid was executed in CleverTap unless actual execution evidence is available.

---

# 52. Logical dry runs

The skill may perform a logical dry run using sample values supplied by the user.

Example:

```text
Profile.Membership = Gold
Profile.PurchaseCount = 12
```

The skill can determine which condition should execute.

This must be described as:

**Logical dry run**

Never describe it as:

**Tested in CleverTap**

unless it was actually executed there.

---

# 53. Compatibility decision rules

When evaluating a construct:

## Supported

Generate it when:

* It is explicitly documented by CleverTap.
* The syntax matches documented CleverTap syntax.
* The campaign context is appropriate.
* Required account capabilities are available.

## Conditionally supported

Generate it only when:

* The required channel/context is established.
* The required account configuration is established.
* The relevant property structure is valid.
* Other documented conditions are satisfied.

## Unsupported

Do not generate it when:

* It exists only in generic Shopify Liquid.
* CleverTap explicitly excludes it.
* It requires an undocumented filter/function/operator.
* It conflicts with a documented CleverTap limitation.

## Unknown

If support cannot be established:

1. Check current official CleverTap documentation.
2. Check this reference.
3. Check verified compatibility evidence if available.
4. Do not guess.
5. Tell the user that compatibility could not be verified.

---

# 54. No invented functionality

The skill must never invent:

* CleverTap Liquid tags.
* CleverTap filters.
* CleverTap operators.
* CleverTap functions.
* CleverTap syntax.
* Profile properties.
* Event properties.
* Context-specific variables.
* Campaign capabilities.
* Date functions.
* Epoch functions.
* Array functions.
* Object functions.

If a feature is requested but cannot be verified, explicitly state the limitation.

---

# 55. No invented data

A syntactically valid Liquid expression can still be factually incorrect if its referenced property does not exist.

Therefore:

```liquid
{{ Profile.DueDate }}
```

must not be generated merely because `DueDate` is a reasonable property name.

The skill must require that the property be:

* Provided by the user.
* Confirmed by the user.
* Or verified from available data/context.

The same applies to Event properties.

---

# 56. Property names and capitalization

Respect the exact property names supplied by the user.

For standard CleverTap namespaces:

```liquid
Profile.PropertyName
Event.PropertyName
```

The namespace capitalization is significant.

Do not silently rename:

```text
Profile.Membership
```

to:

```text
Profile.membership
```

or:

```text
Event.Amount
```

to:

```text
Event.amount
```

unless the user confirms the actual property name.

---

# 57. Data types

When the data type materially affects correctness, establish it.

Relevant types include:

* String
* Number
* Boolean
* Array
* Object
* Date
* Datetime
* Epoch seconds
* Epoch milliseconds

Examples:

For numeric comparison:

```liquid
{% if Profile.PurchaseCount > 5 %}
```

the underlying value must behave as a number.

For date calculations, establish the representation before calculating.

For arrays, establish the array structure before indexing or iterating.

---

# 58. Array safety

Arrays require additional caution because incorrect assumptions can affect campaign delivery.

Before using:

```liquid
array[0]
```

or a nested index:

```liquid
array[0].items[1]
```

verify:

* Array existence.
* Expected length.
* Index validity.
* Nested structure.
* Campaign context.
* Fallback behavior.

Do not assume `default` automatically makes every invalid array index safe.

---

# 59. Nested-object safety

Before generating nested personalization:

Verify:

* Nested Object Personalization is enabled.
* Root property exists.
* Nested path exists.
* Nesting depth is within documented limits.
* Data structure is valid.
* Channel/context is supported.
* Preview & Test can be used where appropriate.

---

# 60. Existing Liquid

If existing Liquid is already correct:

Do not rewrite it unnecessarily.

Preserve:

* Existing variable names.
* Existing conditions.
* Existing formatting.
* Existing output.
* Existing HTML.

Only make changes required by the user's request.

---

# 61. Minimal-change repair

When repairing existing Liquid:

1. Identify the actual defect.
2. Preserve valid logic.
3. Correct the smallest necessary portion.
4. Verify CleverTap compatibility.
5. Avoid unrelated refactoring.

Do not rewrite working code merely because a shorter implementation exists.

---

# 62. Unsupported functionality workaround

If a requested feature is not supported by CleverTap Liquid:

### Requirement

Explain what the customer wants.

### Limitation

Explain which required functionality is unavailable.

### Supported alternative

Explain what can be achieved using supported CleverTap functionality.

### Data-model workaround

When appropriate, recommend precomputing the required value before it reaches CleverTap.

Example:

If complex date arithmetic cannot be reliably performed in CleverTap Liquid, an upstream system could provide:

```text
Profile.DaysUntilDue
```

The campaign can then use supported conditional logic.

The workaround must clearly be identified as a workaround.

Never represent a workaround as native CleverTap Liquid functionality.

---

# 63. Documentation freshness

CleverTap's Liquid functionality evolves.

The compatibility reference must therefore be treated as a maintained knowledge base rather than a permanent snapshot.

When a new CleverTap Liquid feature is discovered:

1. Verify it against current official documentation.
2. Determine supported syntax.
3. Determine supported channels.
4. Determine account/configuration requirements.
5. Determine data-type limitations.
6. Determine relevant restrictions.
7. Update this reference.
8. Do not automatically assume universal support.

When a feature is removed or restricted:

1. Update the reference.
2. Mark the feature appropriately.
3. Prevent the skill from generating obsolete functionality.

---

# 64. Final production-readiness rule

A Liquid expression should be considered production-ready by this skill only when it is:

* Syntactically valid.
* Logically correct.
* Compatible with documented CleverTap Liquid behavior.
* Appropriate for the campaign context.
* Based on real/confirmed Profile or Event properties.
* Based on supported data structures.
* Free of invented CleverTap functionality.
* Free of unsupported Shopify-only functionality.
* Safe against known array/nested-object limitations.
* Consistent with applicable account configuration requirements.

If any of these cannot be established, the skill must clearly identify the uncertainty rather than presenting the Liquid as production-ready.

---

# 65. Final hard rules

The skill must:

* Treat current official CleverTap documentation as the primary source of truth.
* Treat this file as the compatibility knowledge base.
* Treat undocumented functionality as unknown, not supported.
* Never assume Shopify Liquid functionality is supported by CleverTap.
* Never invent CleverTap tags, filters, operators, functions, or syntax.
* Never invent Profile properties.
* Never invent Event properties.
* Never invent context-specific CleverTap variables.
* Preserve exact customer-provided property names.
* Respect `Profile.` and `Event.` capitalization.
* Confirm data type when it materially affects correctness.
* Confirm Profile vs Event when source ambiguity materially affects correctness.
* Confirm epoch seconds vs milliseconds when relevant and unknown.
* Consider timezone and date-vs-timestamp semantics for date logic.
* Never invent `date_diff`, `to_epoch`, `days_between`, or equivalent undocumented functionality.
* Use `date_tz` for explicitly required timezone-aware output.
* Treat `now` as IST by default.
* Consider channel restrictions for feature-specific functionality.
* Treat nested personalization as requiring the appropriate account capability.
* Respect documented nested-object limits.
* Be conservative with array indexing.
* Recognize that invalid array indexing can affect campaign delivery.
* Preserve valid Liquid and HTML wherever possible.
* Treat HTML + Liquid workflow as Email-specific unless current CleverTap documentation establishes broader support.
* Distinguish syntax validity from logical correctness.
* Distinguish logical correctness from CleverTap compatibility.
* Never claim CleverTap execution unless actual execution occurred.
* Use logical dry runs only when clearly identified as such.
* Prefer a supported, slightly more verbose implementation over an unsupported shorter implementation.
* If a requirement cannot be implemented with supported CleverTap Liquid, explicitly explain the limitation.
* Provide a genuine supported workaround where one exists.
* Do not manufacture a workaround where none exists.
* Update this reference when official CleverTap functionality changes.
* Never sacrifice CleverTap compatibility merely to satisfy requested syntax.
* The final implementation must solve the customer's actual business requirement rather than merely produce Liquid that looks syntactically correct.

[1]: https://docs.clevertap.com/docs/liquid-tags "Liquid Tags"
[2]: https://docs.clevertap.com/docs/nested-objects-in-user-properties "Nested Objects in User Properties"
