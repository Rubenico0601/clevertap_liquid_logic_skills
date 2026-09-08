# CleverTap Liquid Compatibility Reference

## 1. Compatibility Target

This reference is for Liquid logic intended to run in CleverTap.

Target Liquid engine:

```text
LiqP 0.7.9
````

The primary compatibility source is the current CleverTap Liquid documentation.

Official CleverTap Liquid documentation:

[https://docs.clevertap.com/docs/liquid-tags](https://docs.clevertap.com/docs/liquid-tags)

Nested object documentation:

[https://docs.clevertap.com/docs/nested-objects-in-user-properties](https://docs.clevertap.com/docs/nested-objects-in-user-properties)

This reference must be treated as a compatibility guide, not as a generic Shopify Liquid reference.

---

# 2. Compatibility Classification

Every Liquid construct should be classified into one of the following categories.

## 2.1 Officially documented

The construct is explicitly documented by CleverTap.

Use it for new Liquid generation.

---

## 2.2 Verified implementation behavior

The construct is known to work in the relevant CleverTap environment through reliable implementation evidence, testing, or confirmed internal behavior.

It may be used when appropriate, but do not describe it as officially documented unless the documentation explicitly supports it.

---

## 2.3 Existing known-working customer code

The construct appears in Liquid supplied by the customer and the customer indicates that it currently works.

The skill may:

* Analyze it.
* Explain it.
* Preserve it.
* Repair it.
* Adapt it carefully.

Existing working code is not automatically proof that the construct is officially supported for all CleverTap use cases.

---

## 2.4 Unknown

The construct is not sufficiently established.

Do not invent compatibility.

If the construct is essential to the requested solution, ask for verification or provide a limitation/workaround.

---

# 3. Source-of-Truth Hierarchy

Use the following order of authority:

1. Current CleverTap official documentation.
2. Verified CleverTap implementation behavior.
3. Known-working CleverTap/customer implementation.
4. Generic Liquid knowledge.
5. Generic Shopify documentation.

Generic Shopify behavior must never override CleverTap-specific compatibility.

If CleverTap documentation does not mention a feature, do not automatically conclude that generic Liquid support applies.

---

# 4. Engagement Channels

CleverTap Liquid can be used in supported campaign/message contexts including:

* Email.
* Mobile Push.
* SMS.
* Webhooks.
* App Inbox.
* In-App.
* Native Display.
* WhatsApp.

Channel-specific behavior and limitations must still be checked where relevant.

---

# 5. Variable Namespaces

CleverTap Liquid commonly exposes data through namespaces such as:

```liquid
Profile.property
```

and:

```liquid
Event.property
```

Property names are case-sensitive and must be preserved exactly.

Do not invent properties.

Do not change:

```text
Profile.sign_up_date
```

to:

```text
Profile.signup_date
```

unless the user explicitly requests the change.

---

# 6. Output Tags

Liquid output uses:

```liquid
{{ expression }}
```

Whitespace-control forms may be used where supported:

```liquid
{{- expression -}}
```

The skill should preserve existing whitespace-control syntax unless there is a reason to change it.

---

# 7. Conditional Tags

CleverTap documents conditional constructs including:

```liquid
{% if condition %}
{% elsif condition %}
{% else %}
{% endif %}
```

Also supported:

```liquid
{% unless condition %}
{% endunless %}
```

and:

```liquid
{% case value %}
{% when value %}
{% else %}
{% endcase %}
```

Use documented syntax.

---

# 8. if

Documented conditional syntax:

```liquid
{% if condition %}
...
{% endif %}
```

Multiple conditions may be combined using documented logical operators.

Example:

```liquid
{% if Profile.age >= 18 %}
Adult
{% endif %}
```

Do not assume undocumented operators.

---

# 9. elsif

Use:

```liquid
{% elsif condition %}
```

Example:

```liquid
{% if Profile.age >= 60 %}
Senior
{% elsif Profile.age >= 18 %}
Adult
{% else %}
Minor
{% endif %}
```

Do not substitute unsupported syntax such as:

```liquid
{% else if condition %}
```

unless that exact syntax is verified for the target environment.

---

# 10. else

Use:

```liquid
{% else %}
```

for the fallback branch of an applicable conditional structure.

Do not silently add an `else` branch when it changes the intended business logic.

---

# 11. unless

Documented form:

```liquid
{% unless condition %}
...
{% endunless %}
```

Use only where it improves clarity or matches existing logic.

---

# 12. case / when

Documented form:

```liquid
{% case Profile.plan %}
  {% when "free" %}
    Free
  {% when "premium" %}
    Premium
  {% else %}
    Unknown
{% endcase %}
```

Do not assume arbitrary pattern matching behavior.

---

# 13. for

Documented loop construct:

```liquid
{% for item in collection %}
...
{% endfor %}
```

Use only with a valid collection.

Validate:

* Collection existence.
* Collection type.
* Loop bounds.
* `limit`.
* `offset`.
* `break`.
* `continue`.

---

# 14. break

Use:

```liquid
{% break %}
```

to terminate a supported loop.

Do not use it outside a valid loop context.

---

# 15. continue

Use:

```liquid
{% continue %}
```

to skip to the next supported loop iteration.

Do not assume behavior outside documented loop contexts.

---

# 16. split

CleverTap documents `split` for creating a collection from a string.

Example pattern:

```liquid
{% assign items = "a,b,c" | split: "," %}
```

Validate the input type and resulting collection before using it.

Do not infer additional string-manipulation capabilities from `split`.

---

# 17. abort

CleverTap supports:

```liquid
{% abort %}
```

Use only where intentionally required.

Do not introduce `abort` merely to hide missing or invalid data unless that behavior is desired.

---

# 18. limit

`limit` is documented for controlling the number of loop iterations.

Example:

```liquid
{% for item in collection limit: 3 %}
...
{% endfor %}
```

Do not assume `limit` works as a general-purpose variable transformation.

---

# 19. offset

`offset` is documented for controlling the starting position of supported loops.

Example:

```liquid
{% for item in collection offset: 2 %}
...
{% endfor %}
```

Validate collection and loop behavior.

---

# 20. tablerow

CleverTap documents the `tablerow` loop construct.

Use only when the requested output actually requires table-row generation.

Do not invent undocumented parameters or behaviors.

---

# 21. now

CleverTap documents:

```liquid
now
```

for the current date/time.

CleverTap documentation indicates that `now` defaults to IST / UTC+5:30.

Do not automatically assume that `now` is a numeric Unix epoch value in every context.

For example:

```liquid
{% assign current_epoch = now %}
```

must not automatically be treated as a valid numeric epoch assignment without verifying the behavior required by the calculation.

If a calculation depends on `now` being numeric, explicitly validate that assumption.

---

# 22. date

CleverTap documents date formatting in Liquid contexts.

A documented pattern is conceptually:

```liquid
{{ "now" | date: "%Y-%m-%d %H:%M" }}
```

Date formatting must not be confused with arbitrary date arithmetic.

The existence of date formatting does not establish the existence of:

```text
date_diff
to_epoch
years_since
```

or similar functions.

---

# 23. date_tz

CleverTap documents:

```text
date_tz
```

for timezone-aware date handling.

It accepts an IANA timezone.

Example concept:

```text
Asia/Kolkata
America/New_York
Europe/London
```

Use `date_tz` when the requirement involves explicit timezone handling and the documented operation satisfies the requirement.

Do not replace timezone-aware logic with a hard-coded offset unless there is a specific reason to do so.

---

# 24. assign

CleverTap supports:

```liquid
{% assign variable = value %}
```

Whitespace-control variants may also appear:

```liquid
{%- assign variable = value -%}
```

Use `assign` extensively when building complex procedural calculations.

Example:

```liquid
{%- assign seconds_per_day = 86400 -%}
```

Intermediate variables are preferred for complex algorithms because they make the calculation easier to validate.

---

# 25. raw

CleverTap supports `raw` for preventing Liquid interpretation within a block where appropriate.

Do not modify raw content unnecessarily.

---

# 26. comment

CleverTap supports Liquid comments.

Example:

```liquid
{% comment %}
Explain the calculation here.
{% endcomment %}
```

Whitespace-control forms may also be present:

```liquid
{%- comment -%}
...
{%- endcomment -%}
```

Comments are especially useful for complex mathematical/date algorithms.

---

# 27. Operators

CleverTap documents comparison and logical operators including:

```text
==
!=
>
<
>=
<=
and
or
contains
```

Do not invent operators.

---

# 28. String Comparisons

Supported comparison examples include:

```liquid
{% if Profile.country == "India" %}
...
{% endif %}
```

and:

```liquid
{% if Profile.country != "India" %}
...
{% endif %}
```

Preserve the data type expected by the comparison.

Do not assume that numeric strings and numbers always compare identically.

---

# 29. Numeric Comparisons

Documented comparison operators include:

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
{% if Profile.age >= 18 %}
...
{% endif %}
```

Validate the input type before relying on numeric comparison.

---

# 30. and / or

Logical operators:

```text
and
or
```

Example:

```liquid
{% if Profile.age >= 18 and Profile.country == "India" %}
...
{% endif %}
```

Validate condition precedence rather than assuming complex expressions will always evaluate as intended.

When readability matters, use nested conditions.

---

# 31. contains

CleverTap documents:

```text
contains
```

for supported containment checks.

Example:

```liquid
{% if Profile.interests contains "football" %}
...
{% endif %}
```

Do not assume `contains` supports arbitrary object traversal or undocumented data types.

---

# 32. Nested Object Personalization

CleverTap supports nested object personalization subject to documented account, data, and channel constraints.

Nested properties must be referenced using their actual object hierarchy.

Do not invent object structures.

Example concept:

```text
Profile.parent.child
```

must correspond to an actual nested property.

---

# 33. Nested Object Supported Channels

Nested object functionality is subject to channel-specific support.

The skill must verify the relevant channel before claiming nested-object compatibility.

Do not assume that a feature available in one channel automatically works in every channel.

---

# 34. Nested Object System Limits

CleverTap documentation describes nested object support up to a limited depth.

Do not assume arbitrary nesting.

If the requested property exceeds documented nesting limits, identify the limitation rather than generating unsupported syntax.

---

# 35. Nested Object Account Configuration

Nested object behavior can depend on account configuration/capability.

If a requested solution depends on nested objects and the account capability is unknown, identify the dependency.

Do not state that the logic will work for every account without qualification.

---

# 36. Nested Profile Properties

Profile nested objects must be accessed using the exact structure supplied by CleverTap.

Do not invent:

```text
Profile.customer.address.city
```

unless the user's actual profile structure contains it.

---

# 37. Nested Event Properties

Event nested objects must be based on actual event payload structure.

Do not invent event properties or nesting.

Confirm event context where necessary.

---

# 38. Arrays and Indexing

Loops and collections are supported in documented contexts.

Do not automatically assume every generic Liquid array-indexing syntax is supported.

If a requirement depends on direct indexing, verify the syntax before generating it.

Prefer documented loop constructs where possible.

---

# 39. Default Values

CleverTap documentation includes default-value usage in Liquid examples.

Example:

```liquid
{{ Profile.property | default: "Default" }}
```

Use a default only when:

* The user requested it.
* Existing logic contains it.
* The intended behavior requires it.

Do not invent user-facing fallback strings.

---

# 40. Date Property Behavior

CleverTap transforms date-type properties for Liquid usage.

A date property may be represented in a format such as:

```text
2023-05-29 15:55:00
```

The exact source representation must still be considered when building manual date calculations.

Do not assume every date-like property is stored identically.

---

# 41. Epoch Handling

There is no general rule that a CleverTap date property can be converted to epoch using a generic Liquid filter such as:

```liquid
| to_epoch
```

Do not invent:

```text
to_epoch
timestamp
unix_timestamp
date_to_epoch
```

unless independently verified.

If an epoch value is required, determine whether:

1. A documented CleverTap operation can produce it.
2. A verified implementation behavior can produce it.
3. A mathematical conversion using verified primitives can reproduce it.
4. The calculation should instead be performed upstream.

---

# 42. Date Difference

Do not assume CleverTap provides:

```text
date_diff
days_between
years_since
months_between
```

unless explicitly verified.

If a date difference is required, determine whether the result can be built using supported/verified primitives.

---

# 43. Date/Timezone Rules

When calculating with dates:

1. Establish the source timezone.
2. Establish the target timezone.
3. Establish the representation of the date.
4. Establish the representation of `now`.
5. Keep both timestamps in the same unit.
6. Apply timezone conversion consistently.
7. Do not use hard-coded offsets blindly.
8. Consider daylight-saving behavior where applicable.
9. Distinguish local date arithmetic from elapsed-time arithmetic.

CleverTap's documented `date_tz` functionality should be preferred where appropriate.

---

# 44. Arithmetic

Arithmetic requires special caution.

Current CleverTap Liquid documentation clearly documents comparison/logical operators, but the current documentation should not automatically be treated as confirmation that every generic Liquid arithmetic filter is officially supported.

In particular, do not automatically classify these as officially documented CleverTap filters solely because they exist in generic Shopify Liquid:

```text
plus
minus
times
divided_by
```

Likewise, do not automatically classify:

```text
slice
```

as CleverTap-supported merely because it exists in generic Liquid.

If these constructs appear in existing known-working CleverTap code, they may be analyzed and preserved as existing implementation evidence.

For new generation, compatibility must be established before relying on them.

---

# 44A. Complex Procedural and Algorithmic Liquid

Complex mathematical/date requirements must be treated as algorithms.

The following evidence classes apply:

## Class 1 — Officially documented

The operation is explicitly documented by CleverTap.

Safe for new generation.

---

## Class 2 — Verified implementation evidence

The operation is confirmed through reliable CleverTap implementation evidence.

May be used with an appropriate compatibility note.

---

## Class 3 — Existing known-working customer code

The operation exists in customer Liquid that is known to work.

May be preserved or analyzed.

Do not automatically declare it officially supported.

---

## Class 4 — Unknown / generic-only

The operation is known only from generic Liquid/Shopify or has not been verified.

Do not use it for new CleverTap logic without verification.

---

## 44A.1 Arithmetic Primitive Classification

The following operations require explicit compatibility consideration:

```text
slice
plus
minus
times
divided_by
```

Do not automatically promote them to officially documented CleverTap functionality.

If they occur in a known-working algorithm supplied by the user, preserve them when appropriate.

---

## 44A.2 Mathematical Decomposition

If a direct operation is unavailable, decompose the requirement.

Example:

```text
Desired result:
years since signup

Possible algorithm:
signup date
→ date components
→ date number
→ epoch timestamp
→ current timestamp
→ elapsed seconds
→ years
```

Every step must be individually evaluated.

---

## 44A.3 Date Component Extraction

If existing Liquid extracts components using:

```liquid
slice
```

the skill may analyze that implementation.

For example:

```liquid
{%- assign year = date | slice: 0, 4 -%}
{%- assign month = date | slice: 5, 2 -%}
{%- assign day = date | slice: 8, 2 -%}
```

However, this must not automatically be described as an officially documented CleverTap feature unless verified.

---

## 44A.4 Date-to-Epoch Algorithms

A manual date-to-epoch algorithm may involve:

```text
year
month
day
hour
minute
second
day-count calculation
epoch offset
seconds/day
timezone adjustment
```

The skill must validate every component.

Do not generate such an algorithm merely because the mathematics looks valid.

The underlying Liquid primitives must also be sufficiently established.

---

## 44A.5 Current Timestamp

If an existing implementation uses:

```liquid
{%- assign current_epoch = now -%}
```

the skill must determine whether `now` is being treated as:

* A formatted date/time.
* A numeric timestamp.
* Another representation.

Do not assume the representation without evidence.

---

## 44A.6 Epoch Units

All timestamps in an arithmetic calculation must use the same unit.

Common possibilities include:

```text
seconds
milliseconds
```

Do not subtract milliseconds from seconds.

---

## 44A.7 Integer Division

When:

```liquid
divided_by
```

is used, determine or flag the implementation's numeric behavior.

Potential concerns include:

* Integer truncation.
* Decimal output.
* Rounding.
* Negative values.

Do not assume decimal behavior.

---

## 44A.8 Timezone Offsets

A hard-coded offset such as:

```text
19800 seconds
```

represents:

```text
5 hours 30 minutes
```

and may correspond to IST.

Such an offset is not a universal timezone conversion mechanism.

Use only when the source and target timezone assumptions justify it.

---

## 44A.9 365-Day Year Approximation

The value:

```text
31536000
```

represents:

```text
365 × 24 × 60 × 60
```

Therefore:

```text
elapsed_seconds / 31536000
```

calculates years using a fixed 365-day duration.

This is an approximation.

It is not equivalent to an exact calendar-year difference.

The skill must explicitly identify this distinction.

---

# 45. Unsupported Shopify Functionality

Do not assume that generic Shopify Liquid functionality works in CleverTap.

Examples of functionality that must not be invented or assumed without verification include:

```text
date_diff
days_between
to_epoch
years_since
months_between
arbitrary Ruby-style expressions
unsupported programming syntax
```

When a Shopify feature is unsupported, determine whether:

1. A documented CleverTap equivalent exists.
2. A verified mathematical workaround exists.
3. Existing known-working customer code demonstrates an implementation.
4. The calculation must be moved upstream.

---

# 46. Unsupported Programming Syntax

Liquid is not a general-purpose programming language.

Do not generate:

```text
for loops with unsupported syntax
function definitions
classes
Ruby code
JavaScript
Python
arbitrary expressions
```

inside Liquid unless the requested environment explicitly supports the construct.

---

# 47. Campaign-Context Limitations

Liquid behavior can depend on campaign context.

Consider:

* Channel.
* Profile properties.
* Event properties.
* Qualification context.
* Personalization availability.
* Account configuration.
* Campaign type.

Do not assume that Liquid behavior in one campaign context automatically applies to another.

---

# 48. Event-Property Context

Event properties may require the appropriate event/qualification context.

Do not assume that any arbitrary event property is available in every campaign.

If the event context is missing and materially affects the solution, ask for clarification.

---

# 49. HTML + Liquid

When Liquid is embedded in HTML:

```html
<a href="{{ Profile.url }}">...</a>
```

validate both:

1. Liquid syntax.
2. HTML syntax.

Do not modify unrelated HTML.

Preserve:

* Attributes.
* Quotation style.
* Tags.
* Existing content.
* Whitespace where practical.

---

# 50. HTML Validation

For HTML containing Liquid:

* Ensure tags remain balanced.
* Ensure attributes remain properly quoted.
* Ensure Liquid does not accidentally break markup.
* Ensure output is valid for the intended HTML context.

Do not insert arbitrary HTML escaping unless required.

---

# 51. Validation

Every generated or repaired Liquid should be evaluated at three levels.

## Syntax

Does the Liquid structure appear valid?

## Semantic logic

Does the Liquid implement the requested business behavior?

## Compatibility

Are the constructs sufficiently established as CleverTap-compatible?

For complex algorithms also validate:

## Mathematical correctness

Does the calculation produce the intended mathematical result?

---

# 52. Logical Dry Runs

Perform conceptual dry runs for complex logic.

Example:

```text
Input:
2024-01-15 10:30:00

Expected intermediate flow:
year
month
day
hour
minute
second
day count
epoch seconds
elapsed seconds
years
```

A logical dry run is not equivalent to executing the code in CleverTap.

Never claim actual CleverTap execution unless it occurred.

---

# 53. Compatibility Decision Rules

Use these rules when deciding whether to generate a construct.

### Rule 1

Officially documented CleverTap functionality may be generated.

### Rule 2

Verified implementation behavior may be generated with an appropriate qualification.

### Rule 3

Known-working customer code may be preserved or adapted carefully.

### Rule 4

Generic Shopify functionality must not be automatically generated.

### Rule 5

Unknown functionality must not be invented.

---

# 54. No Invented Functionality

Never invent a filter because the requested behavior needs it.

Bad:

```liquid
{{ Profile.sign_up_date | date_diff: now, "years" }}
```

if `date_diff` has not been established.

Bad:

```liquid
{{ Profile.sign_up_date | to_epoch }}
```

if `to_epoch` has not been established.

Instead, decompose the calculation or explain the limitation.

---

# 55. No Invented Data

Never invent:

```text
Profile.signup_date
Event.signup_date
Profile.createdAt
Event.created_at
```

unless the user has provided or established the property.

Business terminology is not sufficient evidence for a property name.

---

# 56. Property Names and Capitalization

Preserve exact property names.

These are not automatically interchangeable:

```text
Profile.sign_up_date
Profile.Sign_Up_Date
Profile.signUpDate
Profile.signupDate
```

Do not normalize property names without instruction.

---

# 57. Data Types

The skill must consider the actual type of every important input.

Examples:

```text
Date
Datetime
String
Number
Boolean
Array
Object
```

A string that looks like a date is not automatically equivalent to a date-type property.

A numeric string is not automatically equivalent to a number.

---

# 58. Array Safety

Before iterating over a collection:

* Confirm it is a collection.
* Confirm the collection exists.
* Consider empty collections.
* Consider loop limits.
* Consider break/continue behavior.

Do not assume arbitrary indexing syntax.

---

# 59. Nested-Object Safety

Before using nested personalization:

* Confirm the property structure.
* Confirm nesting depth.
* Confirm channel support.
* Confirm account capability.
* Preserve exact property names.

Do not generate arbitrary nested paths.

---

# 60. Existing Liquid

Existing customer Liquid is important evidence.

When the user says:

> This code works in CleverTap.

do not unnecessarily replace it with a theoretically cleaner solution.

Instead:

1. Understand the existing algorithm.
2. Identify the requested change.
3. Preserve working constructs.
4. Modify only what is necessary.
5. Clearly distinguish known-working behavior from documented support.

---

# 61. Minimal-Change Repair

When repairing existing Liquid:

* Preserve variable names.
* Preserve algorithm structure.
* Preserve comments.
* Preserve whitespace controls where practical.
* Preserve existing business logic.
* Change only the broken portion.

Do not rewrite a complete working algorithm merely because it could be shorter.

---

# 62. Unsupported Functionality Workaround

When a requested operation is unsupported:

### First choice

Find a documented CleverTap equivalent.

### Second choice

Decompose into verified primitives.

### Third choice

Use an existing known-working implementation if appropriate.

### Fourth choice

Move the calculation upstream.

Do not invent a new Liquid function.

---

# 63. Documentation Freshness

CleverTap Liquid functionality may evolve.

When compatibility is important, prefer current CleverTap documentation over historical examples.

A previously working construct should not automatically be assumed to represent the current documented compatibility surface.

Likewise, the absence of a construct from current documentation does not prove that existing customer code cannot work.

Classify the evidence correctly.

---

# 64. Final Production-Readiness Rule

Before describing Liquid as production-ready, confirm:

```text
Property source
+
Data type
+
Liquid syntax
+
CleverTap compatibility
+
Business logic
+
Mathematical correctness
+
Timezone
+
Epoch units
+
Missing-value behavior
+
Edge cases
```

If one of these remains materially uncertain, state the uncertainty.

---

# 65. Final Hard Rules

1. Target CleverTap LiqP 0.7.9.
2. CleverTap documentation takes precedence over generic Shopify Liquid knowledge.
3. Never invent Liquid tags.
4. Never invent Liquid filters.
5. Never invent Liquid operators.
6. Never invent Profile properties.
7. Never invent Event properties.
8. Never assume a Shopify feature is CleverTap-compatible.
9. Never assume undocumented functionality is supported.
10. Never claim actual CleverTap execution without actual execution.
11. Preserve known-working customer Liquid where appropriate.
12. Distinguish existing working code from official documentation.
13. Validate syntax separately from logic.
14. Validate logic separately from compatibility.
15. Validate mathematical calculations separately from syntax.
16. Do not invent `date_diff`.
17. Do not invent `to_epoch`.
18. Do not invent `days_between`.
19. Do not invent `years_since`.
20. Do not assume `now` is numeric without verification.
21. Do not mix epoch units.
22. Do not hide timezone assumptions.
23. Do not describe 365-day division as an exact calendar-year calculation.
24. Do not assume arithmetic filters are officially documented merely because generic Liquid supports them.
25. When `%s` or another unavailable construct is involved, determine what mathematical result it was intended to provide.
26. When possible, decompose unsupported direct operations into verified primitive operations.
27. Use intermediate variables for complex mathematical/date calculations.
28. Validate intermediate values and units.
29. Ask for the source property when it has not been established.
30. Ask for the data type when it materially affects the solution.
31. Ask for timezone when it materially affects the calculation.
32. Ask whether “years” means calendar years or elapsed duration when the distinction matters.
33. Do not silently drop business conditions.
34. Do not silently change property names.
35. Do not silently change user-facing fallback behavior.
36. Do not silently replace exact calculations with approximations.
37. Clearly identify assumptions.
38. Clearly identify approximations.
39. Clearly identify unverified implementation behavior.
40. If a requirement cannot safely be implemented in Liquid, explain the limitation and provide an upstream workaround where appropriate.

---

# References

[1] CleverTap — Liquid Tags
[https://docs.clevertap.com/docs/liquid-tags](https://docs.clevertap.com/docs/liquid-tags)

[2] CleverTap — Nested Objects in User Properties
[https://docs.clevertap.com/docs/nested-objects-in-user-properties](https://docs.clevertap.com/docs/nested-objects-in-user-properties)

[3] CleverTap — Personalize Message / Liquid personalization documentation
[https://docs.clevertap.com/docs/personalize-message-all](https://docs.clevertap.com/docs/personalize-message-all)

---

# Reference Maintenance Rule

When this compatibility reference is updated:

1. Prefer current CleverTap documentation.
2. Remove assumptions that are no longer valid.
3. Do not promote generic Shopify functionality to CleverTap support without evidence.
4. Preserve known-working implementation notes separately from official documentation.
5. Update the complex-algorithm guidance when new CleverTap capabilities are verified.
6. Keep the distinction between:

   * documented,
   * verified,
   * known-working,
   * unknown.
7. Never use this file as justification for inventing unsupported functionality.

```

This second file is designed to work with the `SKILL.md` I provided first, particularly around the **`%s` → explicit mathematical logic** requirement and the distinction between **officially documented functionality vs. known-working existing Liquid**.
```
