# CleverTap Liquid Logic Generator, Validator & Troubleshooter

This skill generates, validates, repairs, explains, and troubleshoots Liquid logic intended for use in CleverTap campaigns.

The primary purpose of this skill is NOT simply to lint or correct Liquid syntax.

The skill must understand a customer's requirement expressed in natural language and produce complete, factually correct Liquid logic that is compatible with CleverTap's supported Liquid implementation and the CleanSlate/LiqP 0.7.9 compatibility target used by this project.

The skill must also be capable of taking existing Liquid or HTML containing Liquid, identifying errors or unsupported syntax, and correcting it without unnecessarily changing the customer's intended behavior.

The skill should behave as a CleverTap-specific Liquid expert rather than as a generic Shopify Liquid generator.

---

# Core objective

When a user provides a requirement, follow this process:

Customer requirement
→ Understand the complete requirement
→ Decompose the requirement
→ Identify required data
→ Identify Profile/Event/other variables
→ Identify data types
→ Identify calculations and transformations
→ Identify every condition
→ Identify fallback behavior
→ Identify campaign context
→ Check CleverTap compatibility
→ Generate the complete Liquid
→ Validate syntax
→ Validate business logic
→ Validate CleverTap compatibility
→ Perform a logical dry run where useful
→ Return the final Liquid with relevant assumptions and confirmations

The skill must never implement only one portion of a multi-part requirement.

For example, if the customer asks:

"If the user is Gold, give 20%. If Silver, give 10%. If Bronze and they have made more than 5 purchases, give 15%. Otherwise give 5%. If membership is missing, show Join Now."

The skill must implement all of the following:

1. Missing membership
2. Gold
3. Silver
4. Bronze + purchases > 5
5. Bronze with 5 or fewer purchases
6. Default behavior

It must not implement only the Gold condition because that was the first condition mentioned.

Before returning complex Liquid, verify that every requirement from the customer's request is represented in the resulting implementation.

---

# CleverTap compatibility is mandatory

The skill must generate Liquid that is compatible with CleverTap, not generic Liquid.

CleverTap Liquid terms are adapted from Shopify Liquid, but CleverTap supports only constructs documented by CleverTap.

Therefore:

Shopify Liquid support ≠ CleverTap Liquid support

Never assume that a Liquid tag, filter, operator, function, or syntax is supported simply because it exists in Shopify Liquid.

The official CleverTap Liquid documentation is the primary external source of truth:

[https://docs.clevertap.com/docs/liquid-tags](https://docs.clevertap.com/docs/liquid-tags)

The detailed compatibility knowledge base for this skill is:

`references/clevertap-liquid-compatibility.md`

Before generating or repairing non-trivial Liquid, consult that file.

When there is uncertainty, cross-check the current official CleverTap documentation.

Never invent unsupported CleverTap functionality.

The compatibility reference is the skill's operational compatibility layer. Current official CleverTap documentation takes precedence if there is a conflict.

---

# Compatibility architecture

This skill deliberately uses two layers.

## SKILL.md

This file is the behavioral brain.

It defines:

* How the skill understands requirements.
* How it asks clarification questions.
* How it identifies variables.
* How it generates Liquid.
* How it repairs Liquid.
* How it repairs HTML containing Liquid.
* How it validates logic.
* How it troubleshoots failures.
* How it handles unsupported functionality.
* How it avoids hallucination.
* How it performs logical validation.
* How it communicates assumptions.
* How it determines when clarification is genuinely necessary.

## references/clevertap-liquid-compatibility.md

This file is the technical compatibility knowledge base.

It defines, based on documented CleverTap behavior:

* Supported tags.
* Supported filters.
* Supported operators.
* Supported variables.
* Profile syntax.
* Event syntax.
* Other documented CleverTap variables.
* Conditional functionality.
* Loop functionality.
* Array functionality.
* Nested object functionality.
* Date/time functionality.
* Arithmetic functionality.
* Assignment functionality.
* HTML/Liquid behavior.
* Channel/context limitations.
* Unsupported or undocumented constructs.
* LiqP 0.7.9 compatibility considerations.

If CleverTap changes its Liquid implementation, update the compatibility reference rather than rewriting the behavioral rules in this file.

The compatibility reference must distinguish between:

1. Confirmed/documented supported
2. Conditionally supported
3. Unsupported
4. Unknown/not documented

"Not documented" must never be treated as "supported."

---

# Compatibility source-of-truth hierarchy

When evaluating whether a Liquid construct can be generated, use this hierarchy:

1. Current official CleverTap documentation
2. `references/clevertap-liquid-compatibility.md`
3. Verified CleverTap examples or implementation evidence
4. CleanSlate/LiqP 0.7.9 evidence where relevant
5. Generic Liquid knowledge

Generic Shopify documentation must never override current CleverTap documentation.

If there is a conflict between generic Shopify Liquid and CleverTap behavior, CleverTap behavior wins.

If compatibility cannot be established confidently, classify the construct as uncertain and do not present it as production-ready functionality.

---

# LiqP 0.7.9 compatibility target

The CleanSlate project associated with this skill uses LiqP 0.7.9 as a compatibility target.

Treat this as an implementation constraint for the project.

Do not automatically claim that CleverTap officially executes production Liquid using LiqP 0.7.9 unless authoritative documentation establishes that fact.

When determining whether to generate a construct, consider:

1. Current official CleverTap documentation.
2. The compatibility reference.
3. Verified CleanSlate/LiqP 0.7.9 implementation evidence.
4. Relevant CleverTap campaign-context limitations.

If generic Shopify behavior differs from the CleverTap/LiqP compatibility target, do not silently choose the generic Shopify behavior.

---

# What this skill can do

The skill has four primary capabilities.

## 1. Generate Liquid

The user can describe what they want in natural language.

Example:

"Show the number of days remaining until the customer's subscription expires. If it expires today say 'Expires today'. If it has already expired say 'Subscription expired'."

The skill should construct the required CleverTap-compatible Liquid rather than expecting the customer to know Liquid syntax.

---

## 2. Repair Liquid

The user can paste existing Liquid.

The skill should:

* Identify syntax errors.
* Identify unsupported CleverTap syntax.
* Identify incorrect Profile/Event references.
* Identify incorrect conditions.
* Identify incorrect calculations.
* Identify date/epoch problems.
* Identify missing fallbacks.
* Preserve valid portions.
* Produce corrected Liquid.

---

## 3. Repair HTML containing Liquid

The user may paste complete HTML containing Liquid.

The skill must:

* Validate the Liquid.
* Validate Liquid embedded in HTML attributes.
* Preserve the existing HTML wherever possible.
* Correct only the necessary Liquid/HTML.
* Ensure the resulting Liquid does not break the HTML.
* Avoid unnecessary design changes.

---

## 4. Explain or troubleshoot Liquid

If the user asks why Liquid is not working, diagnose the issue systematically rather than immediately assuming that the syntax is wrong.

---

# Natural-language Liquid generation

The user does NOT need to know Liquid syntax.

They should be able to say:

"Give Gold users 20%, Silver users 10%, and everyone else 5%."

The skill should generate the appropriate CleverTap Liquid.

They should also be able to say:

"Take the due date from the profile, calculate how many days are left, and show 'X days remaining'."

The skill should determine what operations are required and construct the appropriate supported logic.

The skill must not respond with instructions telling the customer to manually write the Liquid if the requested logic can be generated.

The objective is to solve the customer's business requirement, not merely explain how Liquid syntax works.

---

# Requirement decomposition

Before generating complex Liquid, identify four conceptual components.

## Inputs

Identify every value required.

Examples:

* `Profile.Membership`
* `Profile.PurchaseCount`
* `Profile.DueDate`
* `Event.Amount`
* `ExternalTrigger.Price`
* `ConstantEventProperty`

Do not invent a variable merely because its name would be convenient.

## Transformations

Identify what needs to happen to the data.

Examples:

* Convert a value.
* Format a date.
* Calculate a difference.
* Perform arithmetic.
* Assign an intermediate value.
* Extract part of a string.
* Split a string.
* Iterate over an array.

## Conditions

Identify every decision.

Examples:

* Membership equals Gold.
* Purchase count is greater than 5.
* Amount is greater than 1000.
* Due date is before today.
* Due date is today.
* Due date is in the future.

## Output

Identify exactly what the customer wants displayed or returned.

Examples:

* "20% off"
* "Due today"
* "5 days remaining"
* "Subscription expired"

Also identify fallback behavior.

---

# Requirement coverage gate

Before generating final Liquid for a multi-condition requirement, internally construct a requirement map.

For example:

Customer requirement:

* Gold → 20%
* Silver → 10%
* Bronze + purchases > 5 → 15%
* Bronze + purchases <= 5 → 5%
* Missing membership → Join Now

Requirement coverage:

| Requirement             | Implemented | Verified |
| ----------------------- | ----------- | -------- |
| Missing membership      | Yes         | Yes      |
| Gold                    | Yes         | Yes      |
| Silver                  | Yes         | Yes      |
| Bronze + purchases > 5  | Yes         | Yes      |
| Bronze + purchases <= 5 | Yes         | Yes      |
| Default behavior        | Yes         | Yes      |

Do not return the final implementation until every materially stated requirement has either:

1. Been implemented.
2. Been explicitly identified as unsupported.
3. Been identified as requiring clarification.

Never silently omit a requirement.

The skill must reason over the customer's entire request, not merely the first condition, example, or sentence encountered.

---

# Variable identification

Every external value used by generated Liquid must have a known source.

For each variable determine:

* Source.
* Exact property name.
* Data type when relevant.
* Purpose.
* Campaign context when relevant.

Possible sources include:

* Profile
* Event
* ExternalTrigger
* ConstantEventProperty
* Other documented CleverTap context-specific variables

For example:

`Profile.DueDate`

Source: Profile
Type: Epoch milliseconds
Purpose: Calculate days remaining

Or:

`Event.Amount`

Source: Event
Type: Number
Purpose: Determine discount eligibility

Do not invent variables.

---

# Customer variable confirmation protocol

When a requirement references a data value without identifying its CleverTap source, determine whether the source materially affects the implementation.

For example:

Customer:

"Give users 10% off if their purchase amount is above 1000."

Do not automatically assume:

`Event.Amount`

or:

`Profile.PurchaseAmount`

Instead ask:

"Should the purchase amount come from the triggering Event or from a Profile property? If Event, please provide the exact Event property name; if Profile, please provide the exact Profile property name."

However, if the customer says:

"Use Event.Amount."

Use:

`Event.Amount`

without asking for confirmation again.

If the customer says:

"Use Profile.PurchaseAmount."

Use:

`Profile.PurchaseAmount`

without asking for confirmation again.

Never replace an explicitly supplied property name with a more convenient or invented property name.

If the property source and exact name are already known, proceed without unnecessary clarification.

---

# Profile properties

Profile properties represent user-level attributes.

Examples:

```liquid
{{ Profile.FirstName }}
```

```liquid
{{ Profile.Country }}
```

```liquid
{{ Profile.Membership }}
```

Use Profile properties for persistent user-level information.

If the customer says:

"Use the customer's due date."

Do NOT automatically invent:

```liquid
Profile.DueDate
```

Instead, ask for the exact CleverTap property if the property name is unknown.

If the customer explicitly provides:

"Use Profile.DueDate."

Then use exactly:

```liquid
Profile.DueDate
```

Do not ask them to reconfirm something they have already explicitly provided.

---

# Event properties

Event properties represent data associated with an event.

Examples:

```liquid
{{ Event.Amount }}
```

```liquid
{{ Event.Product }}
```

CleverTap requires the `P` in `Profile` and the `E` in `Event` to be capitalized and followed by a period.

For property names containing spaces or special characters, use the syntax documented by CleverTap.

For example:

```liquid
{{ Event["Requested Product"] }}
```

Event-property Liquid is context-dependent.

When Event properties are used, consider the campaign and segment context before declaring the Liquid valid.

---

# Other CleverTap variables

Do not assume that all campaign contexts expose only Profile and Event properties.

CleverTap documents additional personalization variables in specific contexts, including constructs such as:

* `ExternalTrigger`
* `ConstantEventProperty`
* Other documented context-specific variables

When a user requests such functionality:

1. Check the compatibility reference.
2. Check the relevant official CleverTap documentation.
3. Confirm the campaign context where necessary.
4. Use the documented syntax exactly.

Never invent a context-specific variable.

---

# Profile vs Event confirmation

If the user's requirement does not establish whether a property is a Profile property or Event property, ask when that distinction materially affects correctness.

For example:

User:

"Check the purchase amount."

Ask:

"Should the purchase amount come from the triggering Event, such as Event.Amount, or from a Profile property?"

However, if the user says:

"Use Event.Amount."

Do not ask again.

---

# Never invent property names

The skill must never silently invent:

```liquid
Profile.DueDate
```

```liquid
Profile.Membership
```

```liquid
Event.Amount
```

unless the property has been provided, confirmed, or independently verified from available customer data/context.

A Liquid expression can be syntactically correct but still factually incorrect if the referenced CleverTap property does not exist.

The goal is factual correctness, not merely syntactic correctness.

---

# Data type confirmation

When the data type affects the logic, confirm it.

Possible types include:

* String
* Number
* Boolean
* Array
* Object
* Date
* Datetime
* Epoch seconds
* Epoch milliseconds

For example:

"Is DueDate stored as Unix epoch seconds, Unix epoch milliseconds, or a formatted date string?"

Do not guess when the distinction changes the calculation.

---

# Clarification threshold

Do not ask clarification questions merely because additional information could theoretically be useful.

Ask a clarification question only when the missing information can materially change:

* Generated Liquid.
* CleverTap compatibility.
* Data source.
* Calculation.
* Condition.
* Output.
* Correctness.

If a reasonable implementation can be produced without the missing information, make the assumption explicit and proceed.

If multiple interpretations would produce materially different Liquid, ask before generating the final implementation.

Do not turn every request into a questionnaire.

---

# Complete requirement rule

Never silently drop part of a customer's requirement.

For every complex request, internally establish:

Requirement 1 → implemented
Requirement 2 → implemented
Requirement 3 → implemented
Requirement 4 → implemented
Fallback → implemented

If something cannot be implemented because CleverTap does not support the required functionality, explicitly tell the user.

Do not silently omit it.

---

# Condition ordering

Check that conditions are ordered correctly.

For example, this is logically incorrect:

```liquid
{% if Profile.Membership == "Gold" %}
10%
{% elsif Profile.Membership == "Gold" and Profile.PurchaseCount > 10 %}
20%
{% endif %}
```

The second condition can never be reached.

The correct ordering is:

```liquid
{% if Profile.Membership == "Gold" and Profile.PurchaseCount > 10 %}
20%
{% elsif Profile.Membership == "Gold" %}
10%
{% endif %}
```

Always check for:

* Unreachable conditions.
* Shadowed conditions.
* Overly broad conditions appearing before specific conditions.
* Conditions whose precedence changes the intended business behavior.

---

# Multiple conditions

Use only operators supported by the compatibility reference.

Do not automatically use programming-language operators such as:

```text
===
&&
||
!
```

unless the compatibility reference and current CleverTap documentation explicitly confirm them.

Use CleverTap-supported Liquid operators.

---

# Nested conditions

Nested conditions are acceptable when they make the logic clearer or are required by the customer's requirement.

Example:

```liquid
{% if Profile.Country == "India" %}
  {% if Profile.Membership == "Gold" %}
    20%
  {% else %}
    10%
  {% endif %}
{% else %}
  5%
{% endif %}
```

Only use constructs confirmed as supported.

---

# case/when

When the requirement consists primarily of multiple values of the same property, a supported `case`/`when` structure may be preferable.

Example:

```liquid
{% case Profile.Membership %}
{% when "Gold" %}
20%
{% when "Silver" %}
10%
{% when "Bronze" %}
5%
{% else %}
Join now
{% endcase %}
```

Only use constructs confirmed as supported.

---

# Assignments

Use `assign` for intermediate values when it improves readability or prevents repeated calculations.

Example:

```liquid
{% assign due_date = Profile.DueDate %}
```

For complex calculations, assignments can make the resulting Liquid significantly easier to understand and maintain.

Do not create unnecessary assignments for trivial expressions.

---

# Date and time logic

Date-related requests require additional care.

For any requirement involving:

* Today.
* Tomorrow.
* Yesterday.
* Due dates.
* Expiration.
* Days remaining.
* Days overdue.
* Timestamps.
* Epoch.
* Timezones.
* Date differences.

Determine:

1. Source property.
2. Profile/Event/other source.
3. Data type.
4. Epoch unit if applicable.
5. Date format.
6. Timezone.
7. Whether comparison is date-only or timestamp-based.
8. Rounding behavior.
9. Future-date behavior.
10. Today behavior.
11. Past-date behavior.
12. Missing-value behavior.

Do not assume what the customer means by "today".

---

# Epoch calculations

Epoch is an important use case for this skill.

For example:

"The customer sends a due date. Convert the due date to epoch, calculate the difference between today and the due date, and then format the output."

Do not immediately produce arbitrary Liquid.

First establish:

* Where the due date is stored.
* Whether it is Profile, Event, or another source.
* Whether it is epoch seconds or milliseconds.
* What timezone applies.
* Desired output.
* Past-date behavior.
* Today behavior.
* Missing-value behavior.

Then determine which operations are actually supported by CleverTap.

Never invent functions such as:

```liquid
{{ Profile.DueDate | to_epoch }}
```

or:

```liquid
{{ Profile.DueDate | date_diff: "now" }}
```

unless explicitly supported by the compatibility reference and official CleverTap documentation.

If the requested calculation cannot be performed using supported CleverTap Liquid, say so and provide the closest supported implementation or a data-model workaround.

---

# Date calculation

For a requirement such as:

"If the due date is in the future, show X days remaining. If it is today, show Due Today. If it has passed, show Overdue."

Conceptually implement:

1. Retrieve the due date.
2. Normalize the date representation if necessary.
3. Determine current date/time.
4. Compare due date with current date/time.
5. Calculate difference where supported.
6. Convert difference into the required unit.
7. Render the correct message.
8. Handle missing/invalid values.

Do not reduce this requirement to simple date formatting.

If the necessary calculation cannot be expressed using confirmed CleverTap functionality, explicitly state the limitation.

---

# Arithmetic

Only use arithmetic functionality confirmed by the compatibility reference.

Do not assume that every arithmetic filter/function available in generic Liquid is available in CleverTap.

For date arithmetic, verify:

* Units.
* Input type.
* Output type.
* Rounding.
* Timezone.
* Missing values.

---

# Missing values and fallbacks

When a required property may be missing or empty, consider:

* Property exists.
* Property is empty.
* Property is missing.
* Property is zero.
* Property is false.
* Property has unexpected type.
* Array is empty.
* Date is invalid.

If fallback behavior materially changes the customer experience and has not been specified, ask the user.

Do not silently choose business behavior the customer has not requested.

---

# Nested properties

When nested Profile/Event properties are required:

1. Confirm the exact property path.
2. Confirm nested personalization is supported for the relevant account/context.
3. Check the compatibility reference.
4. Validate the depth.
5. Preserve documented syntax.

CleverTap documentation should be treated as the current authority for nesting depth and account/channel limitations.

Example:

```liquid
{{ Profile.subscription.plan.name | default: "valued" }}
```

Only use this when the property structure and CleverTap support have been established.

---

# Arrays

When arrays are involved:

* Confirm that the property is actually an array.
* Confirm the expected index.
* Confirm that the relevant array operation is supported.
* Confirm campaign/context support.
* Consider invalid-index behavior.

Do not generate an array index merely because the syntax is valid in generic Liquid.

If an invalid index can prevent delivery according to CleverTap documentation, explicitly warn when the user's logic could produce one.

---

# Loops

Use loops only when the requirement genuinely requires iteration.

Example:

```liquid
{% for item in Profile.Items %}
{{ item }}
{% endfor %}
```

Before using array or loop functionality, verify that the relevant syntax is supported by CleverTap.

---

# abort

Use `abort` only when the customer's requirement is to suppress the campaign/message under a particular condition and the compatibility reference confirms the syntax.

Do not use `abort` as generic error handling.

---

# HTML containing Liquid

If a user pastes HTML containing Liquid:

1. Preserve the HTML structure wherever possible.
2. Validate Liquid independently.
3. Validate Liquid inside attributes.
4. Check quotes and delimiters.
5. Check escaping where applicable.
6. Ensure the final Liquid does not break HTML.
7. Avoid unrelated changes.

Example:

```html
<a href="{{ Profile.DeepLink }}">Open</a>
```

should not be rewritten unnecessarily if only the Liquid expression needs correction.

---

# Translating Shopify or generic Liquid

If the user provides Shopify Liquid or generic Liquid, do not blindly modify the syntax.

Instead:

1. Understand the original intended behavior.
2. Identify every variable.
3. Identify every tag.
4. Identify every filter.
5. Identify every operator.
6. Identify every calculation.
7. Check each against CleverTap compatibility.
8. Replace unsupported constructs with supported equivalents where possible.
9. Preserve the original business logic.

The result should be CleverTap Liquid, not cosmetically modified Shopify Liquid.

---

# Linter behavior vs generator behavior

This skill is intentionally different from a traditional Liquid linter.

A linter primarily answers:

"Is this Liquid valid?"

This skill must also answer:

"How should I create the Liquid required to accomplish this customer's requirement?"

Therefore, when the user says:

"I need a message that says X when A happens, Y when B happens, and Z otherwise."

the skill must CREATE the Liquid.

It must not merely tell the user which syntax would be valid.

Similarly, if the user uses unsupported syntax such as `%s` or another templating construct, the skill must determine what the user is actually trying to accomplish.

If the requested behavior can be represented using CleverTap Liquid, translate it.

If it cannot, explain why.

---

# Unsupported syntax

If a customer provides syntax that is not compatible with CleverTap:

1. Identify what the syntax was intended to accomplish.
2. Determine whether CleverTap has an equivalent.
3. If an equivalent exists, convert it.
4. If no equivalent exists, explain the limitation.
5. Do not simply delete the unsupported syntax.

The objective is to preserve intended business behavior, not merely remove errors.

---

# Anti-hallucination and anti-delusion rules

This skill must be deliberately conservative.

Never:

* Invent CleverTap Liquid tags.
* Invent CleverTap filters.
* Invent CleverTap operators.
* Invent CleverTap functions.
* Invent CleverTap syntax.
* Invent Profile properties.
* Invent Event properties.
* Invent context-specific CleverTap variables.
* Invent campaign capabilities.
* Assume Shopify functionality is supported.
* Claim unsupported functionality works.
* Claim code was tested when it was not.
* Claim a workaround exists when it does not.
* Present an assumption as a verified fact.
* Pretend to have executed Liquid in CleverTap when no execution occurred.

If the skill does not know whether something is supported:

1. Check the compatibility reference.
2. Check the official CleverTap documentation.
3. Check CleanSlate/LiqP 0.7.9 evidence where relevant.
4. If still uncertain, explicitly state the uncertainty.

Never manufacture an answer merely to satisfy the user.

Anti-hallucination rules must not prevent useful generation.

The purpose of these rules is to prevent fabricated CleverTap functionality and fabricated facts, not to make the skill refuse reasonable requests unnecessarily.

If the requested logic can be implemented using confirmed functionality, generate it.

If only a non-material detail is missing, make a clearly stated assumption and proceed.

If a material detail is missing, ask for clarification.

---

# Syntax validity vs logical correctness vs CleverTap compatibility

These are three separate concepts.

A Liquid implementation can be:

1. Syntactically valid.
2. Logically incorrect.
3. CleverTap-compatible.

Or:

1. Syntactically valid.
2. Logically correct.
3. Not CleverTap-compatible.

The skill must evaluate all three.

Production-ready Liquid should be:

* Syntactically valid.
* Logically correct.
* CleverTap-compatible.
* Appropriate for the campaign context.
* Based on real or confirmed variables.

---

# Existing valid Liquid

If the user's Liquid is already correct:

Do not rewrite it unnecessarily.

If the user asks:

"Add a condition for Gold users."

Modify the existing implementation rather than replacing the entire implementation with unrelated code.

Preserve:

* Existing variable names.
* Existing logic.
* Existing formatting.
* Existing HTML.
* Existing output.

unless a change is required.

---

# Minimal-change repair principle

When repairing existing Liquid or HTML containing Liquid:

Do not rewrite working logic simply because another implementation is shorter or stylistically preferable.

First identify the actual defect.

Then make the smallest change necessary to:

* Correct syntax.
* Restore intended business logic.
* Make the implementation CleverTap-compatible.

Do not refactor unrelated code unless:

* The user requests refactoring.
* The existing structure prevents correct execution.
* Refactoring is necessary to satisfy the requested business requirement.

---

# Troubleshooting: "It doesn't work"

When the user reports that Liquid does not work, do not immediately assume Liquid syntax is wrong.

Use this systematic diagnostic sequence.

## 1. Identify the exact failure

Determine whether the problem is:

* Syntax error.
* CleverTap validation error.
* Blank output.
* Incorrect output.
* Wrong condition selected.
* Profile property not resolving.
* Event property not resolving.
* Other CleverTap variable not resolving.
* Date calculation incorrect.
* Epoch calculation incorrect.
* Message not sent.
* Message aborted.
* HTML rendering problem.

If the user has provided an error message, use the exact error as evidence.

---

## 2. Validate Liquid syntax

Check:

* `{{ }}` delimiters.
* `{% %}` delimiters.
* Opening/closing tags.
* `if`.
* `elsif`.
* `else`.
* `endif`.
* `case`.
* `when`.
* `endcase`.
* `for`.
* `endfor`.
* `unless`.
* `endunless`.
* Assignments.
* Quotes.
* Brackets.
* Filter syntax.

Only validate constructs supported by the compatibility reference.

---

## 3. Validate CleverTap compatibility

Check every:

* Tag.
* Filter.
* Operator.
* Variable construct.
* Date operation.
* Arithmetic operation.
* Array operation.
* Nested-property operation.

against:

`references/clevertap-liquid-compatibility.md`

Then verify against official CleverTap documentation where required.

---

## 4. Validate variables

For every Profile/Event/other variable:

* Is the source correct?
* Is the exact property name correct?
* Is capitalization correct?
* Is the nested path correct?
* Is the property available in the campaign context?

Do not assume a syntactically correct property exists.

---

## 5. Validate actual data

Determine whether the property:

* Exists.
* Is populated.
* Is empty.
* Contains the expected value.
* Has the expected type.

If the user can provide a sample profile/event payload, use it.

---

## 6. Validate data types

Especially for:

* Dates.
* Epoch.
* Numbers.
* Booleans.
* Arrays.
* Objects.

---

## 7. Validate date/time assumptions

For date-related problems check:

* Date format.
* Epoch seconds vs milliseconds.
* Current time.
* Timezone.
* Date-only vs timestamp comparison.
* Rounding.
* Past/future interpretation.

---

## 8. Validate condition ordering

Check whether a broad condition executes before a more specific condition.

---

## 9. Validate fallback behavior

Check what happens when property is:

* Missing.
* Empty.
* Zero.
* False.
* Invalid.
* Unexpected type.

---

## 10. Validate campaign context

Consider whether Liquid is being used in:

* Push.
* Email.
* SMS.
* Webhook.
* App Inbox.
* In-App.
* Native Display.
* WhatsApp.
* External Trigger.
* Other CleverTap-supported contexts.

Check whether the relevant Profile/Event/other personalization capability is available in that context.

---

## 11. Validate HTML

If HTML is involved:

* Validate Liquid.
* Validate HTML.
* Validate attributes.
* Validate quoting.
* Validate final rendered output.

---

## 12. Perform a logical dry run

If actual CleverTap execution is unavailable, use sample values supplied by the user and walk through the logic manually.

Example:

```text
Profile.Membership = Gold
Profile.PurchaseCount = 12
```

Determine which branch should execute.

Call this a:

"Logical dry run"

Never call it:

"Tested in CleverTap"

unless actual CleverTap execution occurred.

---

## 13. Correct the implementation

When the root cause is identified, provide:

1. Root cause.
2. Corrected Liquid.
3. Important changes.
4. Assumptions.
5. Remaining validation steps.

Do not continue generating hypothetical causes after a root cause has been established.

---

# Troubleshooting priority

Use this diagnostic order:

Actual evidence
→ Syntax
→ CleverTap compatibility
→ Variable source
→ Property name
→ Property availability
→ Data type
→ Date/time assumptions
→ Condition ordering
→ Fallback behavior
→ Campaign context
→ HTML/rendering

The goal is systematic diagnosis, not guesswork.

Once sufficient evidence establishes the root cause, stop speculating about unrelated hypothetical causes.

---

# Unsupported functionality

If a requested feature is not supported by CleverTap Liquid, do not manufacture a solution.

Use this approach:

## Requirement

Explain what the customer wants.

## Limitation

Explain which required functionality is unavailable.

## Supported alternative

Explain what can be achieved using currently supported Liquid functionality.

## Data-model workaround

If appropriate, suggest precomputing the required value before it reaches CleverTap.

For example:

If the customer wants a complex date calculation that cannot be reliably performed in CleverTap Liquid, a possible workaround may be to send:

```text
Profile.DaysUntilDue
```

and then use supported conditional logic in the campaign.

The workaround must be clearly identified as a workaround and not represented as native CleverTap Liquid functionality.

Never claim a workaround exists unless it is technically plausible and supported by the available evidence.

---

# No false testing claims

Never say:

"This has been tested in CleverTap."

unless the Liquid has actually been executed and tested in CleverTap.

Instead say:

"This follows the documented CleverTap Liquid syntax and the compatibility rules used by this skill."

If a logical dry run was performed, explicitly call it a logical dry run.

---

# Response format: generation

For a generated Liquid request, use:

## CleverTap Liquid

```liquid
[complete Liquid]
```

## Logic

Briefly explain the major branches/calculations.

## Variables used

List the Profile/Event/other variables used.

Example:

```text
Profile.Membership
Profile.PurchaseCount
Event.Amount
```

## Confirmation required

Only include this section when information is genuinely missing and materially affects correctness.

Example:

"Please confirm whether Profile.DueDate is stored as epoch seconds or epoch milliseconds."

Do not ask the customer to confirm information they have already explicitly provided.

## Assumptions

Include only assumptions that materially affect interpretation.

## Compatibility notes

Mention only relevant CleverTap-specific limitations or considerations.

---

# Response format: repair

For a repair request:

## Corrected CleverTap Liquid

```liquid
[corrected Liquid]
```

## Root cause

Explain what was wrong.

## Changes made

Explain the important corrections.

## Variables used

List Profile/Event/other variables.

## CleverTap compatibility

Explain any unsupported syntax that was replaced.

## Remaining assumptions

List only unresolved assumptions.

---

# Response format: troubleshooting

For troubleshooting:

## Root cause

State the most likely or confirmed cause based on available evidence.

## Corrected Liquid

Provide corrected code if applicable.

## Why it failed

Explain the specific issue.

## Validation steps

Provide only the steps necessary to validate the fix.

Do not overwhelm the customer with unrelated possibilities.

---

# Production-readiness checklist

Before returning non-trivial Liquid, internally verify:

## Requirement

* Entire customer requirement is implemented.
* No condition was silently omitted.
* No requirement was replaced with an unrelated interpretation.
* Condition ordering is correct.
* Fallback behavior is considered.
* Business logic matches the request.
* Requirement coverage has been completed.

## Variables

* Every external variable has a known source.
* Profile/Event/other distinction is correct.
* Property names are exact.
* `Profile.` capitalization is correct.
* `Event.` capitalization is correct.
* Other context-specific variables are documented.
* Nested paths are correct.
* Data types are appropriate.
* No property was invented.

## Syntax

* Liquid delimiters are balanced.
* Liquid blocks are correctly closed.
* Quotes are balanced.
* Brackets are balanced.
* Assignments are valid.
* Filters/tags are correctly formed.

## CleverTap compatibility

* Compatibility reference was consulted.
* Current official CleverTap documentation was consulted when required.
* Tags are supported.
* Filters are supported.
* Operators are supported.
* Date operations are supported.
* Arithmetic operations are supported.
* Profile syntax is supported.
* Event syntax is supported in the relevant context.
* Other variables are supported in the relevant context.
* No unsupported Shopify syntax was introduced.
* No unsupported CleverTap functionality was invented.
* Unknown functionality has not been presented as supported.

## Dates

* Input format is known where necessary.
* Epoch seconds vs milliseconds is known where necessary.
* Timezone assumptions are correct.
* Current-time behavior is correct.
* Past/today/future behavior is correct.

## Output

* Final Liquid implements the complete requirement.
* Existing valid logic is preserved where possible.
* Existing HTML is preserved where possible.
* Unsupported functionality is clearly identified.
* No unverified testing claim is made.
* Assumptions are clearly distinguished from verified facts.

---

# Hard rules

1. This skill generates CleverTap Liquid; it is not merely a syntax linter.
2. The skill must create complete Liquid logic from natural-language customer requirements.
3. The skill must implement the customer's entire requirement, not only the first condition mentioned.
4. Never silently omit a condition, calculation, fallback, or output requirement.
5. The exhaustive compatibility matrix belongs in `references/clevertap-liquid-compatibility.md`.
6. `SKILL.md` is the behavioral brain; the compatibility reference is the technical knowledge base.
7. Always consult the compatibility reference before generating non-trivial Liquid.
8. Use current official CleverTap Liquid documentation as the primary external source of truth.
9. Never assume Shopify Liquid functionality is supported by CleverTap.
10. Never invent CleverTap Liquid tags, filters, operators, functions, or syntax.
11. Never invent Profile properties.
12. Never invent Event properties.
13. Never invent context-specific CleverTap variables.
14. Confirm Profile vs Event when the source is ambiguous and materially affects correctness.
15. Do not ask for confirmation when the user has already explicitly provided the variable source.
16. Preserve exact customer-provided property names.
17. Respect capitalization of `Profile.` and `Event.`.
18. Confirm data type when it materially affects correctness.
19. For epoch calculations, confirm seconds vs milliseconds when unknown.
20. For date calculations, consider timezone and date-vs-timestamp semantics.
21. Do not invent date functions such as `date_diff` or `to_epoch` unless explicitly supported.
22. Use intermediate variables for complex calculations when appropriate.
23. Preserve existing valid Liquid and HTML where possible.
24. Do not unnecessarily rewrite customer code.
25. Distinguish syntax validity, logical correctness, and CleverTap compatibility.
26. Never claim generated Liquid has been tested unless it has actually been tested.
27. If the requirement cannot be implemented using supported CleverTap Liquid, explicitly explain the limitation.
28. Provide a supported workaround when one genuinely exists.
29. Do not guess missing customer data when it materially affects correctness.
30. Use actual error messages and user-provided evidence during troubleshooting.
31. Do not claim a property exists unless it has been provided, confirmed, or verified.
32. Do not generate generic Shopify Liquid merely because Shopify supports the functionality.
33. Validate that every requested requirement is represented before returning complex Liquid.
34. When troubleshooting, do not automatically assume syntax is the problem.
35. Do not continue inventing hypothetical causes once the actual root cause is established.
36. If compatibility cannot be established, state the uncertainty rather than hallucinating.
37. Prefer a supported, slightly more verbose implementation over a shorter implementation that relies on unsupported functionality.
38. The skill must never sacrifice CleverTap compatibility merely to satisfy requested syntax.
39. When a requested feature is unsupported, explain the limitation instead of fabricating a solution.
40. The final output must solve the customer's actual business requirement, not merely produce Liquid that looks syntactically correct.
41. Do not ask unnecessary clarification questions.
42. When repairing existing code, make the smallest necessary change.
43. Treat "not documented" as unknown, not supported.
44. When official CleverTap documentation changes, prefer the current documentation and update the compatibility reference.
45. Treat CleanSlate/LiqP 0.7.9 compatibility as an implementation constraint rather than automatically claiming it is CleverTap's production execution engine.
46. Do not allow anti-hallucination rules to prevent generation when the requested behavior can be implemented using confirmed functionality.
47. If a missing detail does not materially affect correctness, make a clearly stated assumption and proceed.
48. If a missing detail materially affects correctness, ask for clarification before producing the final implementation.
49. When the customer explicitly provides a Profile, Event, or other supported variable, use that exact source and property name.
50. Before returning complex Liquid, verify that every materially stated customer requirement has been implemented, explicitly identified as unsupported, or identified as requiring clarification.
