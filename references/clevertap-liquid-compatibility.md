CleverTap Liquid Compatibility Reference
1. Compatibility Target
This reference is for Liquid logic intended to run in CleverTap.

Target Liquid engine:

LiqP 0.7.9

The primary compatibility source is the current CleverTap Liquid documentation.

Official CleverTap Liquid documentation:

https://docs.clevertap.com/docs/liquid-tags

Nested object documentation:

https://docs.clevertap.com/docs/nested-objects-in-user-properties

This reference must be treated as a compatibility guide, not as a generic Shopify Liquid reference.


2. Compatibility Classification
Every Liquid construct should be classified into one of the following categories.
2.1 Officially documented
The construct is explicitly documented by CleverTap.

Use it for new Liquid generation.


2.2 Verified implementation behavior
The construct is known to work in the relevant CleverTap environment through reliable implementation evidence, testing, or confirmed internal behavior.

It may be used when appropriate, but do not describe it as officially documented unless the documentation explicitly supports it.


2.3 Existing known-working customer code
The construct appears in Liquid supplied by the customer and the customer indicates that it currently works.

The skill may:

Analyze it.
Explain it.
Preserve it.
Repair it.
Adapt it carefully.

Existing working code is not automatically proof that the construct is officially supported for all CleverTap use cases.


2.4 Unknown
The construct is not sufficiently established.

Do not invent compatibility.

If the construct is essential to the requested solution, ask for verification or provide a limitation/workaround.


3. Source-of-Truth Hierarchy
Use the following order of authority:

Current CleverTap official documentation.
Verified CleverTap implementation behavior.
Known-working CleverTap/customer implementation.
Generic Liquid knowledge.
Generic Shopify documentation.

Generic Shopify behavior must never override CleverTap-specific compatibility.

If CleverTap documentation does not mention a feature, do not automatically conclude that generic Liquid support applies.


4. Engagement Channels
CleverTap Liquid can be used in supported campaign/message contexts including:

Email.
Mobile Push.
SMS.
Webhooks.
App Inbox.
In-App.
Native Display.
WhatsApp.

Channel-specific behavior and limitations must still be checked where relevant.


5. Variable Namespaces
CleverTap Liquid commonly exposes data through namespaces such as:

Profile.property

and:

Event.property

Linked Content is exposed through a third namespace, Linked.<APIName>.<label>. See section 44B.

Property names are case-sensitive and must be preserved exactly.

Do not invent properties.

Do not change:

Profile.sign_up_date

to:

Profile.signup_date

unless the user explicitly requests the change.


6. Output Tags
Liquid output uses:

{{ expression }}

Whitespace-control forms may be used where supported:

{{- expression -}}

The skill should preserve existing whitespace-control syntax unless there is a reason to change it.


7. Conditional Tags
CleverTap documents conditional constructs including:

{% if condition %}
{% elsif condition %}
{% else %}
{% endif %}

Also supported:

{% unless condition %}
{% endunless %}

and:

{% case value %}
{% when value %}
{% else %}
{% endcase %}

Use documented syntax.


8. if
Documented conditional syntax:

{% if condition %}
...
{% endif %}

Multiple conditions may be combined using documented logical operators.

Example:

{% if Profile.age >= 18 %}
Adult
{% endif %}

Do not assume undocumented operators.


9. elsif
Use:

{% elsif condition %}

Example:

{% if Profile.age >= 60 %}
Senior
{% elsif Profile.age >= 18 %}
Adult
{% else %}
Minor
{% endif %}

Do not substitute unsupported syntax such as:

{% else if condition %}

unless that exact syntax is verified for the target environment.


10. else
Use:

{% else %}

for the fallback branch of an applicable conditional structure.

Do not silently add an else branch when it changes the intended business logic.


11. unless
Documented form:

{% unless condition %}
...
{% endunless %}

Use only where it improves clarity or matches existing logic.


12. case / when
Documented form:

{% case Profile.plan %}
  {% when "free" %}
    Free
  {% when "premium" %}
    Premium
  {% else %}
    Unknown
{% endcase %}

Do not assume arbitrary pattern matching behavior.


13. for
Documented loop construct:

{% for item in collection %}
...
{% endfor %}

Use only with a valid collection.

Validate:

Collection existence.
Collection type.
Loop bounds.
limit.
offset.
break.
continue.


14. break
Use:

{% break %}

to terminate a supported loop.

Do not use it outside a valid loop context.


15. continue
Use:

{% continue %}

to skip to the next supported loop iteration.

Do not assume behavior outside documented loop contexts.


16. split
CleverTap documents split for creating a collection from a string.

Example pattern:

{% assign items = "a,b,c" | split: "," %}

Validate the input type and resulting collection before using it.

Do not infer additional string-manipulation capabilities from split.


17. abort
CleverTap supports:

{% abort %}

Use only where intentionally required.

Do not introduce abort merely to hide missing or invalid data unless that behavior is desired.


18. limit
limit is documented for controlling the number of loop iterations.

Example:

{% for item in collection limit: 3 %}
...
{% endfor %}

Do not assume limit works as a general-purpose variable transformation.


19. offset
offset is documented for controlling the starting position of supported loops.

Example:

{% for item in collection offset: 2 %}
...
{% endfor %}

Validate collection and loop behavior.


20. tablerow
CleverTap documents the tablerow loop construct.

Use only when the requested output actually requires table-row generation.

Do not invent undocumented parameters or behaviors.


21. now
CleverTap documents:

now

for the current date/time.

CleverTap documentation indicates that now defaults to IST / UTC+5:30.

Do not automatically assume that now is a numeric Unix epoch value in every context.

For example:

{% assign current_epoch = now %}

must not automatically be treated as a valid numeric epoch assignment without verifying the behavior required by the calculation.

If a calculation depends on now being numeric, explicitly validate that assumption.


22. date
CleverTap documents date formatting in Liquid contexts.

A documented pattern is conceptually:

{{ "now" | date: "%Y-%m-%d %H:%M" }}

Date formatting must not be confused with arbitrary date arithmetic.

The existence of date formatting does not establish the existence of:

date_diff
to_epoch
years_since

or similar functions.


23. date_tz
CleverTap documents:

date_tz

for timezone-aware date handling.

It accepts an IANA timezone.

Example concept:

Asia/Kolkata
America/New_York
Europe/London

Use date_tz when the requirement involves explicit timezone handling and the documented operation satisfies the requirement.

Do not replace timezone-aware logic with a hard-coded offset unless there is a specific reason to do so.


24. assign
CleverTap supports:

{% assign variable = value %}

Whitespace-control variants may also appear:

{%- assign variable = value -%}

Use assign extensively when building complex procedural calculations.

Example:

{%- assign seconds_per_day = 86400 -%}

Intermediate variables are preferred for complex algorithms because they make the calculation easier to validate.


25. raw
CleverTap supports raw for preventing Liquid interpretation within a block where appropriate.

Do not modify raw content unnecessarily.


26. comment
CleverTap supports Liquid comments.

Example:

{% comment %}
Explain the calculation here.
{% endcomment %}

Whitespace-control forms may also be present:

{%- comment -%}
...
{%- endcomment -%}

Comments are especially useful for complex mathematical/date algorithms.


27. Operators
CleverTap documents comparison and logical operators including:

==
!=
>
<
>=
<=
and
or
contains

Do not invent operators.


28. String Comparisons
Supported comparison examples include:

{% if Profile.country == "India" %}
...
{% endif %}

and:

{% if Profile.country != "India" %}
...
{% endif %}

Preserve the data type expected by the comparison.

Do not assume that numeric strings and numbers always compare identically.


29. Numeric Comparisons
Documented comparison operators include:

>
<
>=
<=
==
!=

Example:

{% if Profile.age >= 18 %}
...
{% endif %}

Validate the input type before relying on numeric comparison.


30. and / or
Logical operators:

and
or

Example:

{% if Profile.age >= 18 and Profile.country == "India" %}
...
{% endif %}

Validate condition precedence rather than assuming complex expressions will always evaluate as intended.

When readability matters, use nested conditions.


31. contains
CleverTap documents:

contains

for supported containment checks.

Example:

{% if Profile.interests contains "football" %}
...
{% endif %}

Do not assume contains supports arbitrary object traversal or undocumented data types.


32. Nested Object Personalization
CleverTap supports nested object personalization subject to documented account, data, and channel constraints.

Nested properties must be referenced using their actual object hierarchy.

Do not invent object structures.

Example concept:

Profile.parent.child

must correspond to an actual nested property.


33. Nested Object Supported Channels
Nested object functionality is subject to channel-specific support.

The skill must verify the relevant channel before claiming nested-object compatibility.

Do not assume that a feature available in one channel automatically works in every channel.


34. Nested Object System Limits
CleverTap documentation describes nested object support up to a limited depth.

Do not assume arbitrary nesting.

If the requested property exceeds documented nesting limits, identify the limitation rather than generating unsupported syntax.


35. Nested Object Account Configuration
Nested object behavior can depend on account configuration/capability.

If a requested solution depends on nested objects and the account capability is unknown, identify the dependency.

Do not state that the logic will work for every account without qualification.


36. Nested Profile Properties
Profile nested objects must be accessed using the exact structure supplied by CleverTap.

Do not invent:

Profile.customer.address.city

unless the user's actual profile structure contains it.


37. Nested Event Properties
Event nested objects must be based on actual event payload structure.

Do not invent event properties or nesting.

Confirm event context where necessary.


38. Arrays and Indexing
Loops and collections are supported in documented contexts.

Do not automatically assume every generic Liquid array-indexing syntax is supported.

If a requirement depends on direct indexing, verify the syntax before generating it.

Prefer documented loop constructs where possible.


39. Default Values
CleverTap documentation includes default-value usage in Liquid examples.

Example:

{{ Profile.property | default: "Default" }}

Use a default only when:

The user requested it.
Existing logic contains it.
The intended behavior requires it.

Do not invent user-facing fallback strings.


40. Date Property Behavior
CleverTap transforms date-type properties for Liquid usage.

A date property may be represented in a format such as:

2023-05-29 15:55:00

The exact source representation must still be considered when building manual date calculations.

Do not assume every date-like property is stored identically.


41. Epoch Handling
There is no general rule that a CleverTap date property can be converted to epoch using a generic Liquid filter such as:

| to_epoch

Do not invent:

to_epoch
timestamp
unix_timestamp
date_to_epoch

unless independently verified.

If an epoch value is required, determine whether:

A documented CleverTap operation can produce it.
A verified implementation behavior can produce it.
A mathematical conversion using verified primitives can reproduce it.
The calculation should instead be performed upstream.


42. Date Difference
Do not assume CleverTap provides:

date_diff
days_between
years_since
months_between

unless explicitly verified.

If a date difference is required, determine whether the result can be built using supported/verified primitives.


43. Date/Timezone Rules
When calculating with dates:

Establish the source timezone.
Establish the target timezone.
Establish the representation of the date.
Establish the representation of now.
Keep both timestamps in the same unit.
Apply timezone conversion consistently.
Do not use hard-coded offsets blindly.
Consider daylight-saving behavior where applicable.
Distinguish local date arithmetic from elapsed-time arithmetic.

CleverTap's documented date_tz functionality should be preferred where appropriate.


44. Arithmetic
Arithmetic requires special caution.

Current CleverTap Liquid documentation clearly documents comparison/logical operators, but the current documentation should not automatically be treated as confirmation that every generic Liquid arithmetic filter is officially supported.

In particular, do not automatically classify these as officially documented CleverTap filters solely because they exist in generic Shopify Liquid:

plus
minus
times
divided_by

Likewise, do not automatically classify:

slice

as CleverTap-supported merely because it exists in generic Liquid.

If these constructs appear in existing known-working CleverTap code, they may be analyzed and preserved as existing implementation evidence.

For new generation, compatibility must be established before relying on them.


44A. Complex Procedural and Algorithmic Liquid
Complex mathematical/date requirements must be treated as algorithms.

The following evidence classes apply:
Class 1 — Officially documented
The operation is explicitly documented by CleverTap.

Safe for new generation.


Class 2 — Verified implementation evidence
The operation is confirmed through reliable CleverTap implementation evidence.

May be used with an appropriate compatibility note.


Class 3 — Existing known-working customer code
The operation exists in customer Liquid that is known to work.

May be preserved or analyzed.

Do not automatically declare it officially supported.


Class 4 — Unknown / generic-only
The operation is known only from generic Liquid/Shopify or has not been verified.

Do not use it for new CleverTap logic without verification.


44A.1 Arithmetic Primitive Classification
The following operations require explicit compatibility consideration:

slice
plus
minus
times
divided_by

Do not automatically promote them to officially documented CleverTap functionality.

If they occur in a known-working algorithm supplied by the user, preserve them when appropriate.


44A.2 Mathematical Decomposition
If a direct operation is unavailable, decompose the requirement.

Example:

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

Every step must be individually evaluated.


44A.3 Date Component Extraction
If existing Liquid extracts components using:

slice

the skill may analyze that implementation.

For example:

{%- assign year = date | slice: 0, 4 -%}
{%- assign month = date | slice: 5, 2 -%}
{%- assign day = date | slice: 8, 2 -%}

However, this must not automatically be described as an officially documented CleverTap feature unless verified.


44A.4 Date-to-Epoch Algorithms
A manual date-to-epoch algorithm may involve:

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

The skill must validate every component.

Do not generate such an algorithm merely because the mathematics looks valid.

The underlying Liquid primitives must also be sufficiently established.


44A.5 Current Timestamp
If an existing implementation uses:

{%- assign current_epoch = now -%}

the skill must determine whether now is being treated as:

A formatted date/time.
A numeric timestamp.
Another representation.

Do not assume the representation without evidence.


44A.6 Epoch Units
All timestamps in an arithmetic calculation must use the same unit.

Common possibilities include:

seconds
milliseconds

Do not subtract milliseconds from seconds.


44A.7 Integer Division
When:

divided_by

is used, determine or flag the implementation's numeric behavior.

Potential concerns include:

Integer truncation.
Decimal output.
Rounding.
Negative values.

Do not assume decimal behavior.


44A.8 Timezone Offsets
A hard-coded offset such as:

19800 seconds

represents:

5 hours 30 minutes

and may correspond to IST.

Such an offset is not a universal timezone conversion mechanism.

Use only when the source and target timezone assumptions justify it.


44A.9 365-Day Year Approximation
The value:

31536000

represents:

365 × 24 × 60 × 60

Therefore:

elapsed_seconds / 31536000

calculates years using a fixed 365-day duration.

This is an approximation.

It is not equivalent to an exact calendar-year difference.

The skill must explicitly identify this distinction.


44B. Linked Content
Source: https://docs.clevertap.com/docs/linked-content

Linked Content personalizes a message with data fetched at send time from an API that is configured in the dashboard (Settings > Setup > Linked Content). It is documented for Email, Mobile Push, SMS, Webhooks, WhatsApp, and Native Display.
44B.1 Officially documented syntax
Linked.<APIName>.<label>
Linked.<APIName>.<label>.<field>

Documented examples:

{% if Linked.cityBasedWeather.temp > 30 %}
{% for restaurant in Linked.RestaurantAPI.restaurants_info limit:5 %}
  {{ restaurant.name }} {{ restaurant.user_rating.rating_text }}
{% endfor %}

<APIName> is a nickname assigned in the dashboard. Every documented example is a single word without spaces.
<label> is an object label from the API response, a custom label, or a system label.
System labels: raw (raw response), json (response parsed as JSON), http_status_code, headers.
Nested fields use dot notation. Arrays use the documented for loop.
44B.2 Not documented: bracket form and names with spaces
The only documented bracket syntax is for event properties that contain spaces:

Event["Property Name"]

CleverTap documents no bracket form and no space-containing name for Linked Content. Therefore:

Linked["trivago hotel recommender"].key
Linked['trivago hotel recommender'].key

are classified as 2.4 Unknown. Do not generate them. Do not describe them as supported.

If the user's API name contains spaces, ask them to rename the API in the dashboard to a space-free name and use Linked.<APIName>.<label>.

If such a form already exists in user code, preserve it, label it unknown / known-working-only at best, and recommend renaming.
44B.3 Rules
Never infer the API name or label. Ask for the exact values or a sample response.
Preserve exact capitalization of the API name and labels.
Every printed Linked value still needs a non-empty default.
When Linked Content and other Liquid appear in the same template, validate every Linked.* reference independently (API name spelled identically, label exists, dot form, default present, quoting valid), then validate the Profile / Event / conditional Liquid separately. Report both.
A Linked reference inside an HTML attribute must follow section 49A.
Linked responses can be personalized with {{Profile.x}}, {{Event["x"]}}, {{Campaign.campaignId}}, {{ExternalTrigger.x}}, and {{Profile.Identity}} (private beta). Treat this as an account-dependent capability. Event properties fall back to their default for single-occurrence inaction campaigns with a delay over 24 hours.
CleverTap does not retry a failed Linked Content API call, and waits five seconds for the response. Missing or failed responses therefore fall back to the default value.


45. Unsupported Shopify Functionality
Do not assume that generic Shopify Liquid functionality works in CleverTap.

Examples of functionality that must not be invented or assumed without verification include:

date_diff
days_between
to_epoch
years_since
months_between
arbitrary Ruby-style expressions
unsupported programming syntax

When a Shopify feature is unsupported, determine whether:

A documented CleverTap equivalent exists.
A verified mathematical workaround exists.
Existing known-working customer code demonstrates an implementation.
The calculation must be moved upstream.


46. Unsupported Programming Syntax
Liquid is not a general-purpose programming language.

Do not generate:

for loops with unsupported syntax
function definitions
classes
Ruby code
JavaScript
Python
arbitrary expressions

inside Liquid unless the requested environment explicitly supports the construct.


47. Campaign-Context Limitations
Liquid behavior can depend on campaign context.

Consider:

Channel.
Profile properties.
Event properties.
Qualification context.
Personalization availability.
Account configuration.
Campaign type.

Do not assume that Liquid behavior in one campaign context automatically applies to another.


48. Event-Property Context
Event properties may require the appropriate event/qualification context.

Do not assume that any arbitrary event property is available in every campaign.

If the event context is missing and materially affects the solution, ask for clarification.


49. HTML + Liquid
When Liquid is embedded in HTML:

<a href="{{ Profile.url }}">...</a>

validate both:

Liquid syntax.
HTML syntax.

Do not modify unrelated HTML.

Preserve:

Attributes.
Quotation style.
Tags.
Existing content.
Whitespace where practical.


49A. Quoting Inside HTML Attributes
Never nest the same quote type inside an HTML attribute that contains Liquid.

Wrong:

<a href="{{ Linked["api"].key | default:"https://example.com" }}">

Right:

<a href="{{ Linked.api.key | default:'https://example.com' }}">

Rules:

Inside a double-quoted attribute, use single quotes for every Liquid string: default:'...', Event['Property Name'], Profile['name']. Inside a single-quoted attribute, use double quotes.
Applies to href, src, alt, title, style, and any other attribute that contains Liquid.
Evidence level: an HTML-parsing risk, not a CleverTap-documented rule. The editor, an HTML parser, or click tracking can end the attribute at the inner quote before Liquid runs. Report it as a risk, not as documented behavior.
A default containing an apostrophe inside an attribute: ask the user for apostrophe-free text rather than guessing an escape.
Text outside attributes may use either quote type.
Preserve existing correct quoting and change only nested quotes.


49B. AMP4Email
Source of truth: the official AMP4EMAIL validator (the engine behind validator.ampproject.org, also used by Gmail).

Spec: https://amp.dev/documentation/guides-and-tutorials/learn/email-spec/amp-email-format

CleverTap AMP for Email documentation: https://docs.clevertap.com/docs/ampforemail

Documented by CleverTap: AMP for Email needs the Email Booster Max / Advanced Email add-on, is sent through SendGrid, and every AMP email must have an HTML fallback (AMP expires after 30 days, is not shown when forwarded, and is not supported by every client). The "Email with custom HTML & AMP" template has an AMP HTML Body box (shows Valid or "X Errors") and a separate HTML Body for the fallback. CleverTap's FAQ says the AMP Playground does not recognize @SpecialLink, and errors about it can be ignored.
49B.1 Detection and evidence level
An AMP4Email template has ⚡4email or amp4email on the <html> tag, typically within the first ~500 characters.

For AMP templates apply AMP4EMAIL rules instead of generic HTML rules. Do not flag amp-* tags as unknown. Still lint the Liquid.

The official validator cannot be run by this skill. Any AMP result is a manual emulation of its rules: classify it as verified-by-rule-reading, never as official validation. Recommend validator.ampproject.org (format AMP4EMAIL).
49B.2 Required skeleton
<!doctype html>
<html ⚡4email>   (or <html amp4email>)
<head> and <body>
<meta charset="utf-8">
<script async src="https://cdn.ampproject.org/v0.js"></script>
<style amp4email-boilerplate>body{visibility:hidden}</style>

Only one <style amp-custom>, in <head>, at most 75 KB, with no !important and no position: fixed or sticky.
49B.3 Validator error codes and fixes
Error code
Meaning
Fix
MANDATORY_TAG_MISSING, MANDATORY_CDATA_MISSING_OR_INCORRECT
Required boilerplate missing or altered
Restore the exact skeleton in 49B.2
DISALLOWED_TAG
img, video, audio, iframe, link rel=stylesheet, object, embed, base
Use amp-img, amp-anim and similar. form needs amp-form and an https action-xhr
DISALLOWED_SCRIPT_TAG
Script other than the runtime, an extension script, or application/json
Remove inline JS and third-party libraries
DISALLOWED_ATTR
on* handlers, target other than _blank
Remove the attribute. AMP's on="tap:..." is allowed
DISALLOWED_STYLE_ATTR
style="..."
Move the styles into <style amp-custom>
INVALID_URL_PROTOCOL, DISALLOWED_RELATIVE_URL
http:// or relative resource URL
Use absolute https://
MISSING_REQUIRED_EXTENSION, EXTENSION_UNUSED
amp-* element without its script, or script without use
Add or remove <script async custom-element="amp-x" src="https://cdn.ampproject.org/v0/amp-x-0.1.js">
MANDATORY_ATTR_MISSING, SPECIFIED_LAYOUT_INVALID, IMPLIED_LAYOUT_INVALID, MISSING_LAYOUT_ATTRIBUTES, INCONSISTENT_UNITS_FOR_WIDTH_HEIGHT
amp-img needs src, width, height (same unit), and ideally layout
Valid layouts: fixed, responsive, fill, fixed-height, nodisplay, container, intrinsic
DUPLICATE_UNIQUE_TAG, DUPLICATE_ATTRIBUTE, DUPLICATE_DIMENSION, WRONG_PARENT_TAG, DISALLOWED_TAG_ANCESTOR, DISALLOWED_CHILD_TAG_NAME
Duplicate or misplaced tags and attributes
Keep one of each unique tag, remove repeated attributes, fix nesting
STYLESHEET_TOO_LONG
<style amp-custom> over 75 KB
Minify, drop unused rules
CSS_SYNTAX_DISALLOWED_PROPERTY, CSS_SYNTAX_DISALLOWED_PROPERTY_VALUE, CSS_SYNTAX_QUALIFIED_RULE_HAS_NO_DECLARATIONS
Blocked CSS property or value, empty rule
Remove filter, mix-blend-mode, behavior, !important, empty rules
INVALID_JSON_CDATA
Malformed JSON in application/json, amp-list, or amp-state
Fix the JSON


Extensions commonly used in AMP4Email: amp-carousel, amp-accordion, amp-list, amp-bind, amp-form, amp-selector, amp-mustache, amp-lightbox, amp-timeago, amp-fit-text, amp-image-lightbox, amp-anim. amp-img and amp-layout need no extension script. Check the spec for any extension not listed here. Do not invent extensions.
49B.4 Auto-fixable versus suggestion-only
Mechanical (the CleanSlate AMP tab offers a Fix button): DISALLOWED_ATTR (strip the attribute), DUPLICATE_ATTRIBUTE (drop the second occurrence), INVALID_URL_PROTOCOL (http:// to https://), MISSING_REQUIRED_EXTENSION (insert the script before </head>, never twice), EXTENSION_UNUSED (delete that script line).

All other codes are suggestion-only because the fix needs the user's judgment. Offer the corrected snippet and change nothing else.
49B.5 Liquid inside AMP
Liquid in attribute values or URLs (src="{{ x }}") cannot be resolved statically. Mark it UNKNOWN, not PASS, and require an https default.
Each {% if %} branch must produce valid AMP. Check every branch, tag balance, and that exactly one <style amp-custom> remains.
amp-mustache {{var}} collides with Liquid {{ }}. CleverTap documents the fix: wrap each mustache expression in {% raw %}...{% endraw %}, for example {% raw %}{{#cart_items}}{% endraw %} and {% raw %}{{name}}{% endraw %}. Evidence level: officially documented (AMP For Email page, "Skipping Curly Braces in Amp-Mustache Templates").
<html ⚡4email data-css-strict> is the form used in CleverTap's own examples, so data-css-strict on the html tag is fine.
A @SpecialLink token in the template is a CleverTap editor token, not AMP. Do not flag it as an AMP error.
Liquid output inside <style amp-custom> counts toward the 75 KB cap.
Section 49A (attribute quoting) applies.
49B.6 Delivery assumptions that cannot be checked from the template
An AMP email needs a text/x-amp-html MIME part, a sender registered with Gmail, and an HTML fallback part. State these as assumptions.


50. HTML Validation
For HTML containing Liquid:

Ensure tags remain balanced.
Ensure attributes remain properly quoted.
Ensure Liquid does not accidentally break markup.
Ensure output is valid for the intended HTML context.

Do not insert arbitrary HTML escaping unless required.


51. Validation
Every generated or repaired Liquid should be evaluated at three levels.
Syntax
Does the Liquid structure appear valid?
Semantic logic
Does the Liquid implement the requested business behavior?
Compatibility
Are the constructs sufficiently established as CleverTap-compatible?

For complex algorithms also validate:
Mathematical correctness
Does the calculation produce the intended mathematical result?


52. Logical Dry Runs
Perform conceptual dry runs for complex logic.

Example:

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

A logical dry run is not equivalent to executing the code in CleverTap.

Never claim actual CleverTap execution unless it occurred.


53. Compatibility Decision Rules
Use these rules when deciding whether to generate a construct.
Rule 1
Officially documented CleverTap functionality may be generated.
Rule 2
Verified implementation behavior may be generated with an appropriate qualification.
Rule 3
Known-working customer code may be preserved or adapted carefully.
Rule 4
Generic Shopify functionality must not be automatically generated.
Rule 5
Unknown functionality must not be invented.


54. No Invented Functionality
Never invent a filter because the requested behavior needs it.

Bad:

{{ Profile.sign_up_date | date_diff: now, "years" }}

if date_diff has not been established.

Bad:

{{ Profile.sign_up_date | to_epoch }}

if to_epoch has not been established.

Instead, decompose the calculation or explain the limitation.


55. No Invented Data
Never invent:

Profile.signup_date
Event.signup_date
Profile.createdAt
Event.created_at

unless the user has provided or established the property.

Business terminology is not sufficient evidence for a property name.


56. Property Names and Capitalization
Preserve exact property names.

These are not automatically interchangeable:

Profile.sign_up_date
Profile.Sign_Up_Date
Profile.signUpDate
Profile.signupDate

Do not normalize property names without instruction.


57. Data Types
The skill must consider the actual type of every important input.

Examples:

Date
Datetime
String
Number
Boolean
Array
Object

A string that looks like a date is not automatically equivalent to a date-type property.

A numeric string is not automatically equivalent to a number.


58. Array Safety
Before iterating over a collection:

Confirm it is a collection.
Confirm the collection exists.
Consider empty collections.
Consider loop limits.
Consider break/continue behavior.

Do not assume arbitrary indexing syntax.


59. Nested-Object Safety
Before using nested personalization:

Confirm the property structure.
Confirm nesting depth.
Confirm channel support.
Confirm account capability.
Preserve exact property names.

Do not generate arbitrary nested paths.


60. Existing Liquid
Existing customer Liquid is important evidence.

When the user says:

This code works in CleverTap.

do not unnecessarily replace it with a theoretically cleaner solution.

Instead:

Understand the existing algorithm.
Identify the requested change.
Preserve working constructs.
Modify only what is necessary.
Clearly distinguish known-working behavior from documented support.


61. Minimal-Change Repair
When repairing existing Liquid:

Preserve variable names.
Preserve algorithm structure.
Preserve comments.
Preserve whitespace controls where practical.
Preserve existing business logic.
Change only the broken portion.

Do not rewrite a complete working algorithm merely because it could be shorter.


62. Unsupported Functionality Workaround
When a requested operation is unsupported:
First choice
Find a documented CleverTap equivalent.
Second choice
Decompose into verified primitives.
Third choice
Use an existing known-working implementation if appropriate.
Fourth choice
Move the calculation upstream.

Do not invent a new Liquid function.


63. Documentation Freshness
CleverTap Liquid functionality may evolve.

When compatibility is important, prefer current CleverTap documentation over historical examples.

A previously working construct should not automatically be assumed to represent the current documented compatibility surface.

Likewise, the absence of a construct from current documentation does not prove that existing customer code cannot work.

Classify the evidence correctly.


64. Final Production-Readiness Rule
Before describing Liquid as production-ready, confirm:

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

If one of these remains materially uncertain, state the uncertainty.


65. Final Hard Rules
Target CleverTap LiqP 0.7.9.
CleverTap documentation takes precedence over generic Shopify Liquid knowledge.
Never invent Liquid tags.
Never invent Liquid filters.
Never invent Liquid operators.
Never invent Profile properties.
Never invent Event properties.
Never assume a Shopify feature is CleverTap-compatible.
Never assume undocumented functionality is supported.
Never claim actual CleverTap execution without actual execution.
Preserve known-working customer Liquid where appropriate.
Distinguish existing working code from official documentation.
Validate syntax separately from logic.
Validate logic separately from compatibility.
Validate mathematical calculations separately from syntax.
Do not invent date_diff.
Do not invent to_epoch.
Do not invent days_between.
Do not invent years_since.
Do not assume now is numeric without verification.
Do not mix epoch units.
Do not hide timezone assumptions.
Do not describe 365-day division as an exact calendar-year calculation.
Do not assume arithmetic filters are officially documented merely because generic Liquid supports them.
When %s or another unavailable construct is involved, determine what mathematical result it was intended to provide.
When possible, decompose unsupported direct operations into verified primitive operations.
Use intermediate variables for complex mathematical/date calculations.
Validate intermediate values and units.
Ask for the source property when it has not been established.
Ask for the data type when it materially affects the solution.
Ask for timezone when it materially affects the calculation.
Ask whether “years” means calendar years or elapsed duration when the distinction matters.
Do not silently drop business conditions.
Do not silently change property names.
Do not silently change user-facing fallback behavior.
Do not silently replace exact calculations with approximations.
Clearly identify assumptions.
Clearly identify approximations.
Clearly identify unverified implementation behavior.
If a requirement cannot safely be implemented in Liquid, explain the limitation and provide an upstream workaround where appropriate.
Never generate Linked["..."] or a Linked API name containing spaces. Use Linked.<APIName>.<label> and ask for a space-free API name.
Never infer a Linked Content API name or label.
Never nest the same quote type inside an HTML attribute that contains Liquid.
For AMP4Email, never claim official validation. State that it is a manual emulation of the AMP4EMAIL rules and recommend validator.ampproject.org.


References
[1] CleverTap — Liquid Tags https://docs.clevertap.com/docs/liquid-tags

[2] CleverTap — Nested Objects in User Properties https://docs.clevertap.com/docs/nested-objects-in-user-properties

[3] CleverTap — Personalize Message / Liquid personalization documentation https://docs.clevertap.com/docs/personalize-message-all

[4] CleverTap — Linked Content https://docs.clevertap.com/docs/linked-content

[5] CleverTap — AMP For Email https://docs.clevertap.com/docs/ampforemail

[6] AMP Project — AMP for Email format specification https://amp.dev/documentation/guides-and-tutorials/learn/email-spec/amp-email-format


Reference Maintenance Rule
When this compatibility reference is updated:

Prefer current CleverTap documentation.

Remove assumptions that are no longer valid.

Do not promote generic Shopify functionality to CleverTap support without evidence.

Preserve known-working implementation notes separately from official documentation.

Update the complex-algorithm guidance when new CleverTap capabilities are verified.

Keep the distinction between:

documented,
verified,
known-working,
unknown.

Never use this file as justification for inventing unsupported functionality.
