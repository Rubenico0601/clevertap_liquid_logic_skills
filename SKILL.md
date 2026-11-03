
name: clevertap-liquid-logic description: Generate, validate, repair, explain, and troubleshoot Liquid logic for CleverTap campaigns using documented CleverTap Liquid functionality. Use this skill whenever a user asks to create or modify CleverTap Liquid, validate existing Liquid, troubleshoot Liquid behavior, work with HTML containing CleverTap Liquid, determine whether a Liquid feature is supported by CleverTap, or convert a business requirement into multi-step mathematical or date/time Liquid logic. Also use it for Linked Content references (Linked..) and for validating AMP4Email templates.
CleverTap Liquid Logic
Purpose
This skill generates, validates, repairs, explains, and troubleshoots Liquid logic intended for CleverTap campaigns.

The target environment is CleverTap Liquid using the LiqP 0.7.9 engine.

The skill must prioritize CleverTap-specific compatibility over generic Shopify Liquid knowledge.

The skill is expected to handle both:

Simple Liquid requirements such as conditions, personalization, loops, and fallbacks.
Complex procedural or mathematical requirements where a business requirement must be decomposed into multiple Liquid operations.

The second category is especially important when a direct operation or syntax such as %s, a date-difference function, or another unsupported convenience function cannot be used and the desired result must instead be constructed through explicit mathematical logic.


Core Principles
1. CleverTap compatibility comes first
Never assume that Liquid syntax supported by Shopify or another Liquid implementation is automatically supported by CleverTap.

The fact that a Liquid tag, filter, operator, function, or syntax exists in generic Liquid does not establish that it works in CleverTap.

Always distinguish between:

Officially documented CleverTap functionality.
Functionality supported based on verified implementation evidence.
Existing customer Liquid that is known to work.
Generic Shopify or Liquid functionality that is not established as CleverTap-compatible.
Unknown functionality.

Unknown functionality must remain unknown.

Do not turn an assumption into a compatibility claim.


2. Never invent functionality
Do not invent:

Liquid tags.
Liquid filters.
Liquid operators.
Liquid functions.
CleverTap-specific syntax.
Profile properties.
Event properties.
Event names.
Object structures.
Data types.
Date formats.
Timezone behavior.
Epoch units.

Examples of filters or functions that must not be invented merely because they would be convenient:

date_diff
days_between
to_epoch
years_since

If a direct operation is unavailable, determine whether the requested behavior can be reproduced using verified primitives.


3. Requirement decomposition is mandatory for complex logic
Do not treat every Liquid request as a simple syntax-generation task.

For requirements involving:

Dates.
Times.
Epoch values.
Mathematical calculations.
Unit conversions.
Multiple intermediate calculations.
Nested conditions.
Multiple dependent variables.
%s or other unavailable syntax.
Date differences.
Age calculations.
Duration calculations.
Calendar calculations.

first decompose the requirement into:

Inputs.
Input types.
Transformations.
Intermediate variables.
Mathematical operations.
Conditions.
Timezone assumptions.
Units.
Final output.

Then determine whether every required operation can be implemented using verified CleverTap-compatible functionality.


Source of Truth
Use the following evidence hierarchy:
Level 1 — Official CleverTap documentation
Highest authority.

Prefer current CleverTap documentation for:

Supported tags.
Supported operators.
Supported Liquid syntax.
Supported personalization.
Nested objects.
Channel-specific limitations.
Date/time behavior.
Current-time behavior.
Liquid restrictions.

Official documentation takes precedence over generic Liquid documentation.


Level 2 — Verified CleverTap implementation evidence
Use this only when the behavior is independently verified through:

Reliable CleverTap implementation evidence.
Reproducible customer behavior.
Confirmed internal behavior.
Regression testing.
Existing known-working CleverTap implementation.

Clearly distinguish verified implementation behavior from officially documented functionality.


Level 3 — Existing known-working customer Liquid
Existing Liquid supplied by the user can be treated as valuable implementation evidence for analysis and preservation.

If the user provides Liquid that is known to work, do not unnecessarily replace its constructs simply because those constructs are not explicitly documented in the current compatibility reference.

However:

Existing known-working code is evidence that the provided implementation works in its context. It is not automatically evidence that every construct in that code is officially supported for all CleverTap use cases.

This distinction is especially important for arithmetic and date calculations.


Level 4 — Generic Liquid / Shopify knowledge
Generic Liquid knowledge may be used to understand a construct or identify a possible implementation pattern.

It must NOT be presented as CleverTap-compatible unless CleverTap compatibility is established.


Mandatory Workflow
For every non-trivial Liquid request, follow this workflow:

Understand the requirement.
Identify required input properties.
Identify property namespaces.
Identify data types.
Identify required transformations.
Identify required conditions.
Identify required mathematical operations.
Check CleverTap compatibility.
Determine whether direct functionality exists.
If direct functionality does not exist, decompose the requirement into verified primitives.
Generate the Liquid.
Validate syntax.
Validate data flow.
Validate mathematical logic.
Validate units and timezone assumptions.
Perform a logical dry run.
Identify assumptions.
Clearly identify approximations or unverified constructs.
Preserve user-provided working logic where appropriate.

Do not skip the compatibility and data-source checks merely because the requested Liquid looks straightforward.


Requirement Decomposition
Before generating Liquid, determine:
Inputs
Identify every value required by the calculation.

For example:

Profile.sign_up_date
now

Do not invent a property when the user has not provided one.


Transformations
Identify each transformation.

For example:

date string
→ year
→ month
→ day
→ date number
→ epoch seconds


Conditions
Identify all conditions explicitly.

Do not silently remove:

if.
elsif.
else.
unless.
case.
when.
Boundary conditions.
Fallback behavior.


Output
Determine exactly what the user wants printed.

For example:

{{ years_since_signup }}

or:

{{ years_since_signup | default: "wonderful" }}

Do not add formatting or fallback behavior unless requested or clearly necessary.


Data Source Rules
Never infer a property from business terminology
If a user says:

Calculate the number of years since signup.

Do not automatically assume:

Profile.sign_up_date

unless that property is already established.

Ask which property should be used.

Prefer option-style clarification where possible:

Which user property should be used as the source date?

A. Profile.sign_up_date
B. Profile.date_of_birth
C. Another property


Preserve exact property names
If the user supplies:

Profile.sign_up_date

preserve:

Profile.sign_up_date

Do not silently change it to:

Profile.signup_date
Profile.Sign_Up_Date
Profile.signUpDate

Property names and capitalization may matter.


Profile vs Event
Distinguish between:

Profile.property

and:

Event.property

If the user says “user property,” prefer Profile only when the context clearly establishes that the value is stored as a profile property.

If it could be either a profile or event property and that distinction materially affects the logic, ask for clarification.

Do not invent event names or event properties.


Data Type Rules
Determine whether the input is:

String.
Number.
Integer.
Boolean.
Date.
Datetime.
Array.
Object.
Nested object.

This matters especially for:

Arithmetic.
Date calculations.
Comparisons.
Loops.
contains.
Nested properties.

If the calculation depends on the exact representation of a date or datetime and that representation is unknown, ask for clarification.


Date and Time Logic
Date/time calculations require additional validation.

Before generating date arithmetic, establish:

Source property.
Whether it contains a date or datetime.
Expected format.
Whether the value includes a timezone.
Which timezone the value represents.
Which timezone the calculation should use.
The representation of now.
Epoch units if epoch calculations are used.
Whether the result should be an exact calendar difference or an elapsed-time approximation.
How invalid or missing dates should be handled.

Do not invent a date conversion filter.

For example, do not automatically generate:

{{ Profile.sign_up_date | to_epoch }}

unless the operation is verified as available in CleverTap.


Complex Procedural and Algorithmic Liquid
Complex Liquid must be treated as an algorithm rather than merely a template.

This is particularly important when a direct Liquid operation is unavailable.

For example, if a requirement conceptually needs:

current timestamp - user timestamp

and a direct timestamp/date operation cannot be used, the skill should determine whether the result can be constructed using available mathematical primitives.


Algorithm decomposition
Represent the algorithm as a dependency chain.

Example:

Profile.sign_up_date
        ↓
extract date components
        ↓
convert components into a day count
        ↓
convert day count into seconds
        ↓
add time components
        ↓
apply timezone adjustment
        ↓
obtain current time
        ↓
subtract timestamps
        ↓
convert elapsed seconds into years
        ↓
output result

Every intermediate value should have a clear purpose.


Preserve intermediate calculations
Prefer explicit intermediate variables over unnecessarily compressed expressions.

For example:

{%- assign year = ... -%}
{%- assign month = ... -%}
{%- assign day = ... -%}
{%- assign total_days = ... -%}
{%- assign epoch_seconds = ... -%}

This improves:

Debuggability.
Mathematical validation.
Data-flow validation.
Maintainability.
Troubleshooting.

Do not collapse a complex algorithm into one expression merely to make the output shorter.


Mathematical Workarounds
When unsupported syntax is encountered, such as %s, do not simply declare the requirement impossible.

First determine whether the requested result can be recreated through explicit mathematics.

For example:

Unsupported direct operation
        ↓
Identify required result
        ↓
Break result into primitive operations
        ↓
Check each primitive
        ↓
Construct intermediate variables
        ↓
Validate the complete calculation

Do not invent a replacement filter merely to make the syntax shorter.


%s and Similar Unsupported Syntax
If %s is unavailable in the target CleverTap Liquid environment, do not generate %s.

Instead:

Determine what %s was intended to accomplish.
Identify the mathematical result required.
Determine whether that result can be obtained without %s.
Decompose the calculation into available/verified primitives.
Generate explicit intermediate calculations.
State any approximation or assumption.

For example, if the intent is:

date → epoch seconds

do not replace %s with an invented filter such as:

| to_epoch

Instead determine whether the date can be converted using verified operations.


Primitive Operation Reasoning
When generating a complex algorithm, classify every required primitive as:
Directly verified
The operation is documented or independently verified for CleverTap.

Use it.
Verified implementation behavior
The operation is known to work in the relevant CleverTap environment but may not be clearly documented.

Use it only when justified and clearly identify the evidence level.
Existing known-working code
The user supplied code containing the operation and says it works.

It may be preserved during repair or adaptation.

Do not automatically promote it to official compatibility.
Unknown
The operation is not established.

Do not invent it.

If it is essential, ask for verification or provide the limitation.


Existing Complex Liquid
If the user provides existing Liquid containing constructs such as:

slice
plus
minus
times
divided_by

and states or demonstrates that the code works in CleverTap, preserve the logic unless there is a specific reason to change it.

Analyze it as an existing implementation.

Do not rewrite working code simply because the current compatibility documentation does not explicitly enumerate every primitive.

However, when generating new code from scratch, do not assume those constructs are officially supported solely because they exist in generic Shopify Liquid.


Arithmetic Validation
For every non-trivial arithmetic expression, validate:

Operand order.
Units.
Intermediate values.
Positive and negative values.
Division behavior.
Rounding behavior.
Integer vs decimal behavior.
Time conversions.
Boundary conditions.

Example:

{%- assign h_sec = hour | times: 3600 -%}

must be understood as:

hours × 3600 seconds/hour

not merely as an arbitrary multiplication.


Integer Division
Do not assume the behavior of:

divided_by

unless CleverTap behavior is verified.

Determine whether the relevant implementation:

Produces an integer.
Produces a decimal.
Rounds.
Truncates.

If the final result depends on decimal precision and the behavior is unknown, explicitly flag the assumption.


Approximation Detection
The skill must recognize when a mathematical shortcut is an approximation.

For example:

31536000 seconds

is:

365 × 24 × 60 × 60

Therefore:

elapsed_seconds / 31536000

is an elapsed-time approximation using a 365-day year.

It is not necessarily the same as an exact calendar-year difference.

The skill must not describe this as an exact age or calendar-year calculation unless that has been established.


Date-to-Epoch Algorithms
A manual date-to-epoch algorithm must be validated at multiple levels.
Input validation
Confirm:

Date format.
Date component positions.
Time component positions.
Whether the date is local or UTC.
Whether the input includes timezone information.


Mathematical validation
Validate:

Year transformation.
Month transformation.
Leap-year adjustments.
Day calculations.
Epoch offset.
Time-of-day calculation.
Timezone offset.


Epoch validation
Confirm:

Whether epoch is in seconds or milliseconds.
Whether now uses the same unit.
Whether both timestamps use the same timezone basis.

Never subtract timestamps that are expressed in different units.


Timezone Handling
Timezone adjustments must be explicit.

For example:

{%- assign ticket_epoch = ticket_epoch_local | minus: 19800 -%}

represents:

5 hours 30 minutes
× 60 seconds
= 19,800 seconds

This may represent an IST adjustment.

Do not assume a hard-coded timezone offset is universally correct.

If the source timezone or target timezone is unclear, ask for clarification.

Prefer timezone-aware functionality such as documented date_tz when it can satisfy the requirement.


Current Time
CleverTap provides now.

Do not automatically assume that now is a numeric Unix epoch value in every context.

Before using:

{%- assign current_epoch = now -%}

for arithmetic, establish or verify the representation expected by the target environment.

If the implementation is known to return a numeric epoch in the relevant context, it may be used accordingly.

Otherwise flag the assumption rather than presenting it as universally guaranteed.


Complex Date Example
A known existing implementation may perform the following sequence:

Profile.sign_up_date
→ extract YYYY
→ extract MM
→ extract DD
→ extract HH
→ extract mm
→ extract SS
→ calculate adjusted year/month
→ calculate total days
→ subtract epoch offset
→ convert days to seconds
→ add time-of-day seconds
→ adjust timezone
→ obtain current timestamp
→ subtract signup timestamp
→ divide by seconds in 365 days
→ print result

The skill must be able to:

Understand this algorithm.
Explain each intermediate variable.
Validate the mathematical dependencies.
Preserve the algorithm when repairing existing code.
Generate an equivalent algorithm when all required primitives are verified.
Clearly identify assumptions.
Identify that the 365-day conversion is an approximation.


Semantic Data Flow Validation
For complex Liquid, validate not only syntax but also the flow of values.

For example:

date
 ↓
year/month/day
 ↓
day count
 ↓
epoch seconds
 ↓
elapsed seconds
 ↓
years

Check that every variable is assigned before it is used.

Check that every transformation receives the expected input type.

Check that the output variable contains the intended unit.


Logical Dry Run
Perform a conceptual dry run before presenting complex Liquid.

Use representative values when possible.

Example:

Input:
2024-01-15 10:30:00

Expected flow:
year = 2024
month = 1
day = 15
...

The dry run is conceptual unless the code has actually been executed in a real CleverTap environment.

Never state:

“I tested this in CleverTap”

unless actual CleverTap execution occurred.

Use language such as:

“Logical dry run indicates...”

or:

“Based on the documented behavior...”


Exact Calendar Years vs Elapsed Years
If the user asks for “years,” determine whether they mean:
Calendar-year difference
Example:

2024-12-31 → 2025-01-01

may cross a calendar-year boundary but is only one day apart.
Elapsed years
Example:

elapsed seconds / seconds in a year

This is a duration approximation.

Do not silently substitute one interpretation for the other.

If the requirement is ambiguous and materially affects the result, ask.


Missing Values
Determine what should happen when the source property is:

Missing.
Empty.
Null.
Invalid.
Malformed.

Do not invent a fallback.

Possible behaviors include:

Print nothing.
Print a supplied fallback.
Abort.
Use a default value.

Ask when the behavior matters and has not been specified.


Nested Objects
Respect CleverTap's documented nested object capabilities.

Do not assume arbitrary object depth.

If the user requests nested personalization:

Verify the nesting depth.
Verify the channel.
Verify the account capability.
Preserve exact property names.

Do not invent nested object paths.


Arrays and Loops
For loops:

{% for item in collection %}
...
{% endfor %}

validate:

Collection existence.
Collection type.
Iteration behavior.
limit.
offset.
break.
continue.

Do not assume array indexing syntax if it is not verified.


Conditions
Use only established comparison and logical operators.

Common documented operators include:

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


Conditional Ordering
When conditions overlap, order them from the most specific condition to the most general condition where appropriate.

Example:

{% if condition_a and condition_b %}
...
{% elsif condition_a %}
...
{% else %}
...
{% endif %}

Do not reorder conditions if doing so changes the original business logic.


Assign
Use assign for intermediate variables when supported and appropriate.

Complex calculations should generally use intermediate assignments.

Example:

{%- assign seconds_per_day = 86400 -%}

Do not introduce unnecessary variables into simple logic.


Default Values
Use fallback/default behavior only when:

The user requested it.
The existing logic already contains it.
It is necessary to prevent the intended output from becoming unusable and the behavior is clearly explained.

Do not silently invent user-facing fallback text.


Raw and Comments
Preserve useful comments and raw blocks from existing Liquid unless they interfere with correctness.

Comments may be used to explain complex algorithms.

For example:

{%- comment -%}
Convert date components to epoch seconds.
{%- endcomment -%}


HTML + Liquid
If the user provides HTML containing Liquid:

Preserve HTML structure.
Preserve attributes.
Preserve quoting style where possible.
Modify only the Liquid necessary.
Do not break HTML while repairing Liquid.
Validate that Liquid output remains appropriate for the HTML context.

Do not replace the user's complete HTML unnecessarily.


Linked Content
Linked Content pulls send-time data from an API configured in the CleverTap dashboard (Settings > Setup > Linked Content). Source: https://docs.clevertap.com/docs/linked-content
Documented syntax
Linked.<APIName>.<label>
Linked.<APIName>.<label>.<field>

Documented examples: {{ Linked.cityBasedWeather.temp }} and {% for restaurant in Linked.RestaurantAPI.restaurants_info limit:5 %}.

<APIName> is the nickname the user gave the API. Every documented example is a single word with no spaces.
<label> is an object label from the API response, a custom label, or a system label: raw, json, http_status_code, headers.
Arrays are looped with the documented for loop. Nested fields use dot notation.
Linked Content works with Email, Mobile Push, SMS, Webhooks, WhatsApp, and Native Display.
Rules
Never infer the API name or the label. If the user has not supplied them, ask for the exact API name and the labels (or a sample response).
Generate only the documented dot form Linked.<APIName>.<label>.
Do NOT generate the bracket form Linked["API name"], and do NOT use API names containing spaces. The only documented bracket form is Event["Property Name"] for event properties with spaces. No bracket or space form is documented for Linked Content, so it is unknown and must not be presented as supported.
If the user's API name contains spaces, say the form is not documented, and ask them to rename the API in the dashboard to a space-free name, then use the dot form.
If existing user code already uses Linked["..."] or a name with spaces, preserve it, but mark the result UNKNOWN for compatibility and recommend renaming.
Every printed Linked value still needs a non-empty default.
When Linked Content and other Liquid (Profile, Event, if, assign) are in the same template, validate every Linked.* reference on its own: API name spelled identically throughout, label exists, dot form, default present, quoting valid in context. Then validate the surrounding Liquid separately, and report both results.
Never claim a Linked reference works in CleverTap unless it was actually tested.


Quoting Inside HTML Attributes
Never nest the same quote type inside an HTML attribute.

Wrong:

<a href="{{ Linked["api"].key | default:"https://example.com" }}">

Right:

<a href="{{ Linked.api.key | default:'https://example.com' }}">

Inside a double-quoted attribute, use single quotes for every Liquid string: default:'...', Event['Property Name'], Profile['name']. If the attribute is single-quoted, use double quotes inside.
This applies to href, src, alt, title, style, and any other attribute that contains Liquid.
Evidence level: this is an HTML-parsing risk, not a CleverTap-documented rule. An editor, HTML parser, or click-tracking step can end the attribute at the inner quote before Liquid runs. State it as a risk, not as documented behavior.
If a default value contains an apostrophe and sits inside an attribute, ask the user for apostrophe-free text rather than guessing an escape.
Text nodes outside attributes may use either quote type.
Preserve existing correct quoting. Change only nested quotes.


AMP4Email Validation
Use this section when the template is AMP4Email, or the user asks for AMP validation or mentions AMP.
Detecting AMP
Treat the template as AMP4Email when the first ~500 characters contain ⚡4email or <html ... amp4email, or the user says it is an AMP email.

For an AMP template:

Apply AMP4EMAIL rules instead of generic HTML rules.
Never flag amp-* tags as unknown tags.
Still lint the Liquid.
Source of truth and honesty
The reference is the official AMP4EMAIL validator, the same engine as validator.ampproject.org and the one Gmail uses. Spec: https://amp.dev/documentation/guides-and-tutorials/learn/email-spec/amp-email-format

The official validator cannot be run by this skill. State that the check is a manual emulation of its rules, never say "validated by AMP", and recommend confirming in validator.ampproject.org (format AMP4EMAIL). Do not invent AMP tags, attributes, extensions, or error codes. Cite line numbers for each finding.
Required skeleton
Missing items are flagged as MANDATORY_TAG_MISSING or MANDATORY_CDATA_MISSING_OR_INCORRECT:

<!doctype html>
<html ⚡4email> or <html amp4email>
<head> and <body>
<meta charset="utf-8">
<script async src="https://cdn.ampproject.org/v0.js"></script>
<style amp4email-boilerplate>body{visibility:hidden}</style> with that exact body

Also: only one <style amp-custom>, inside <head>, at most 75 KB (STYLESHEET_TOO_LONG), with no !important and no position: fixed or sticky.
Rule checks (error code: meaning, fix)
DISALLOWED_TAG: <img>, <video>, <audio>, <iframe>, <link rel=stylesheet>, <object>, <embed>, <base> are not allowed. Use amp-img, amp-anim and similar. <form> needs the amp-form extension and an https action-xhr.
DISALLOWED_SCRIPT_TAG: only the AMP runtime, <script async custom-element="amp-x" src="https://cdn.ampproject.org/v0/amp-x-0.1.js">, and <script type="application/json"> are allowed. No inline JavaScript or third-party libraries.
DISALLOWED_ATTR: on* handlers (onclick, onload), target other than _blank. DISALLOWED_STYLE_ATTR: style="..." is not allowed; move styles into <style amp-custom>. AMP's own on="tap:..." is allowed.
INVALID_URL_PROTOCOL / DISALLOWED_RELATIVE_URL: resource URLs must be absolute https://.
MISSING_REQUIRED_EXTENSION / EXTENSION_UNUSED: every amp-* element (amp-carousel, amp-accordion, amp-list, amp-bind, amp-form, amp-selector, amp-mustache, amp-lightbox, amp-timeago, amp-fit-text, amp-image-lightbox, amp-anim) needs its matching script in <head>, and an unused extension script is flagged. amp-img and amp-layout need no extension. Check the spec for any extension not listed here.
MANDATORY_ATTR_MISSING / SPECIFIED_LAYOUT_INVALID / IMPLIED_LAYOUT_INVALID / MISSING_LAYOUT_ATTRIBUTES / INCONSISTENT_UNITS_FOR_WIDTH_HEIGHT: amp-img needs src, width, height (same unit), and ideally layout. Valid layouts: fixed, responsive, fill, fixed-height, nodisplay, container, intrinsic.
DUPLICATE_UNIQUE_TAG / DUPLICATE_ATTRIBUTE / DUPLICATE_DIMENSION / WRONG_PARENT_TAG / DISALLOWED_TAG_ANCESTOR / DISALLOWED_CHILD_TAG_NAME: one of each unique tag, no repeated attributes, correct nesting.
CSS_SYNTAX_DISALLOWED_PROPERTY / CSS_SYNTAX_DISALLOWED_PROPERTY_VALUE / CSS_SYNTAX_QUALIFIED_RULE_HAS_NO_DECLARATIONS: remove blocked properties (filter, mix-blend-mode, behavior), !important, and empty CSS rules.
INVALID_JSON_CDATA: malformed JSON in application/json scripts or in amp-list / amp-state config.
Auto-fixable versus suggestion-only
These five are mechanical, and the user can apply them with the Fix button in CleanSlate's AMP tab: DISALLOWED_ATTR (strip the attribute), DUPLICATE_ATTRIBUTE (drop the second occurrence), INVALID_URL_PROTOCOL (http:// to https://), MISSING_REQUIRED_EXTENSION (insert the script before </head>, never twice), EXTENSION_UNUSED (delete that script line).

Everything else is suggestion-only because it needs the user's judgment, such as which duplicate to keep or where to move content. Offer the corrected snippet, but change nothing else.
Liquid inside AMP
Validate the structure the recipient receives. Liquid inside attribute values or URLs (src="{{ x }}") cannot be resolved statically. Mark it UNKNOWN, not PASS, and require an https default, for example default:'https://example.com/'.
Each {% if %} branch must produce valid AMP. Check every branch, that tags stay balanced, and that exactly one <style amp-custom> remains.
amp-mustache {{var}} collides with Liquid {{ }}. CleverTap documents the fix: wrap each mustache expression in {% raw %}...{% endraw %}, for example {% raw %}{{name}}{% endraw %} (CleverTap docs, AMP For Email, https://docs.clevertap.com/docs/ampforemail).
<html ⚡4email data-css-strict> is valid and is the form CleverTap's own examples use.
A @SpecialLink token is a CleverTap editor token, not AMP. Do not flag it as an AMP error.
Liquid output inside <style amp-custom> counts toward the 75 KB cap.
The quoting rule in "Quoting Inside HTML Attributes" applies to AMP too.
AMP response format
Verdict: PASS / n errors / n warnings
Findings (per item): Line | Error code | ERROR or WARNING | Message | Fix | Auto-fixable Y/N
Liquid: PASS / FAIL / UNKNOWN
Assumptions and test-send recommendation

Per CleverTap's AMP For Email page, an AMP email also needs the Email Booster Max / Advanced Email add-on, a sending address registered with each email client (Gmail allowlisting can take up to three weeks), and an HTML fallback (AMP expires after 30 days and is not shown when forwarded). In the dashboard use the "Email with custom HTML & AMP" template: AMP HTML Body for the AMP, HTML Body for the fallback. None of this can be checked from the template, so state it as an assumption.


Shopify Liquid Translation
When a user provides Shopify Liquid and asks whether it works in CleverTap:

Parse the intended behavior.
Identify every tag/filter/operator.
Check each construct against CleverTap compatibility.
Separate supported constructs from unsupported or unknown constructs.
Rewrite only where an equivalent CleverTap-compatible implementation exists.
Never assume generic Shopify compatibility means CleverTap compatibility.


Generation Rules
When generating new Liquid:
Step 1 — Identify the source
Determine the exact Profile/Event property.

If missing, ask.


Step 2 — Determine the type
Determine whether the value is:

String.
Number.
Boolean.
Date.
Datetime.
Array.
Object.

If the type affects the algorithm and is unknown, ask.


Step 3 — Decompose the requirement
Create the logical algorithm before writing syntax.


Step 4 — Search for a direct supported solution
If CleverTap has a documented operation that directly solves the requirement, prefer it.

Do not unnecessarily recreate a direct supported capability with complex mathematics.


Step 5 — Build from verified primitives
If no direct solution exists:

Requirement
→ mathematical decomposition
→ verified primitives
→ intermediate assignments
→ final result


Step 6 — Validate
Check:

Syntax.
Compatibility.
Data flow.
Mathematical logic.
Units.
Timezone.
Missing values.
Edge cases.


Step 7 — State assumptions
Examples:

Assumption: Profile.sign_up_date is stored as YYYY-MM-DD HH:mm:ss.

Assumption: the supplied date represents IST.

The result uses a 365-day year and is therefore an elapsed-time approximation.


Repair Rules
When repairing existing Liquid:

Identify the actual failure.
Determine whether it is syntax, semantic, or compatibility related.
Preserve working code.
Make the smallest safe change.
Do not redesign the entire algorithm unless necessary.
Revalidate the complete flow after the change.

For complex algorithms, do not remove intermediate assignments simply to shorten the code.


Compatibility vs Correctness
Always distinguish:
Syntax correctness
Does the Liquid parse structurally?
Semantic correctness
Does it implement the requested business logic?
Compatibility
Does CleverTap support the constructs being used?
Mathematical correctness
Does the calculation actually produce the intended result?

A piece of Liquid can be:

syntactically valid
but mathematically wrong

or:

mathematically correct
but unsupported by CleverTap

The skill must identify the distinction.


Troubleshooting Workflow
When troubleshooting existing Liquid:
1. Identify the symptom
Examples:

Campaign cannot be saved.
Liquid does not render.
Variable is empty.
Incorrect output.
Date calculation is wrong.
Campaign preview differs from expected behavior.
Certain profiles fail.
2. Check syntax
Look for:

Unclosed tags.
Invalid nesting.
Incorrect delimiters.
Invalid Liquid syntax.
3. Check compatibility
Check each tag/filter/operator.
4. Check property availability
Confirm:

Profile vs Event.
Exact property name.
Property existence.
Data type.
5. Check data transformation
For complex calculations, trace intermediate variables.
6. Check timezone and units
Especially for date/epoch calculations.
7. Check edge cases
Consider:

Missing values.
Empty values.
Leap years.
Midnight.
Timezone boundaries.
Negative elapsed values.
Exact boundary conditions.


Unsupported Functionality
If the user requests unsupported functionality:

State that the requested construct is not established as CleverTap-compatible.
Explain what it was intended to accomplish.
Determine whether a supported mathematical or logical workaround exists.
If yes, provide it.
If not, explain the limitation.
Suggest an upstream workaround where appropriate.

Do not fabricate an equivalent Liquid function.


Upstream Workarounds
If the calculation cannot safely be implemented in Liquid, suggest moving the calculation upstream.

Possible upstream locations include:

Application code.
Backend service.
Data pipeline.
Profile property preprocessing.
Event property enrichment.

The workaround should be described as an architectural alternative, not as CleverTap Liquid functionality.


User Clarification Strategy
Ask clarification only when the missing information materially affects correctness.

Prefer concise option-based questions.

Example:

Which property should be used?

A. Profile.sign_up_date
B. Profile.date_of_birth
C. Another property

For date format:

What format does the property use?

A. YYYY-MM-DD
B. YYYY-MM-DD HH:mm:ss
C. ISO 8601 with timezone
D. Other

For output semantics:

Should “years” mean:

A. Exact calendar years
B. Elapsed years based on duration

Do not ask unnecessary questions when the answer can safely be derived from established context.


Response Format: Generation
For a normal generation request, provide:

Brief interpretation.
Required clarification if needed.
Liquid code.
Short explanation.
Assumptions/compatibility notes when relevant.

For complex mathematical logic, include the algorithm at a high level before or after the code.


Response Format: Complex Algorithm
For complex date/math logic, use:

Requirement
↓
Algorithm
↓
Liquid
↓
Validation
↓
Assumptions

Keep the explanation proportional to the complexity.


Response Format: Validation
When validating code, report:

Syntax: PASS / FAIL / UNKNOWN
Logic: PASS / FAIL / NEEDS REVIEW
Compatibility: SUPPORTED / UNSUPPORTED / UNKNOWN

Explain the reason for any failure or uncertainty.


Response Format: Repair
When repairing:

State the issue.
Provide corrected Liquid.
Explain the changes.
Mention any remaining compatibility assumptions.

Keep changes minimal.


Response Format: Troubleshooting
Use:

Observed behavior
↓
Likely cause
↓
Evidence
↓
Fix
↓
Verification

Do not claim a fix has been verified in CleverTap unless it has actually been tested.


Production Readiness Checklist
Before declaring generated Liquid ready:

Exact property names confirmed.
Profile/Event source confirmed.
Data types confirmed.
All required business conditions included.
No unsupported invented functionality.
CleverTap compatibility checked.
Complex mathematical steps validated.
Intermediate variables are correctly ordered.
Units are consistent.
Timezone assumptions are explicit.
Epoch units are consistent.
Division behavior is understood or flagged.
Missing-value behavior is defined.
Approximation is identified.
HTML remains valid when applicable.
Existing working logic is preserved where appropriate.
No false execution/testing claim is made.
Linked Content references use the documented Linked.<APIName>.<label> form with a space-free API name.
No same-type quotes are nested inside HTML attributes.
AMP templates were checked against AMP4EMAIL rules and labelled a manual emulation, not an official validation.


Hard Rules
Never invent CleverTap Liquid functionality.
Never invent Profile or Event properties.
Never assume Shopify Liquid is CleverTap Liquid.
Never silently change a user's property name.
Never silently remove a business condition.
Never replace an unsupported construct with an invented filter.
Never claim actual CleverTap execution without actual execution.
Never describe an approximation as exact.
Never ignore timezone or epoch units in timestamp calculations.
Never assume now is numeric without verifying the relevant behavior.
Never assume arithmetic filter behavior without evidence.
Preserve existing known-working Liquid when repairing unless there is a specific reason to change it.
Distinguish official documentation from verified implementation behavior.
Treat unknown functionality as unknown.
For complex requirements, decompose the algorithm before generating Liquid.
Validate mathematical logic separately from syntax.
When %s or another unsupported operation is involved, determine whether the desired result can be reconstructed through verified mathematical primitives before declaring the requirement impossible.
Ask for clarification when the source property, type, timezone, or desired semantics materially affects correctness.
Prefer explicit intermediate variables for complex calculations.
Clearly identify assumptions, approximations, and unverified implementation behavior.
Never generate Linked["..."] or a Linked API name containing spaces. Use Linked.<APIName>.<label> and ask for a space-free API name.
Never nest the same quote type inside an HTML attribute that contains Liquid.
For AMP4Email, never claim official validation. State it is a manual emulation of the AMP4EMAIL rules and recommend validator.ampproject.org.


Special Rule for Mathematical Date Logic
When the user asks for something equivalent to:

“Create Liquid logic to print the number of years with respect to a user property”

and provides a date/datetime property, do NOT automatically search for a nonexistent years_since, date_diff, or to_epoch filter.

Instead:

Identify the source property.
Determine the date representation.
Determine the required meaning of “years.”
Determine whether a direct CleverTap capability exists.
If not, decompose the calculation.
Determine whether the required arithmetic/date primitives are verified.
Build the calculation with intermediate variables.
Validate every mathematical dependency.
Identify timezone assumptions.
Identify whether the result is an exact calendar calculation or an approximation.
Generate the Liquid only when the required primitives are sufficiently established.
If some primitives are only known from existing working code, clearly distinguish that from official CleverTap documentation.

This rule exists specifically to support cases where a direct operation such as %s cannot be used and the required result must be reproduced through explicit Liquid mathematics.


Final Principle
The goal of this skill is not merely to produce Liquid that looks correct.

The goal is to produce Liquid that is:

Correct
+
CleverTap-compatible
+
Traceable
+
Mathematically sound
+
Faithful to the user's requirement

When those properties cannot all be guaranteed, the skill must clearly state what is known, what is assumed, and what remains unverified.
