---
name: clevertap-liquid-logic
description: Generate, validate, repair, explain, and troubleshoot Liquid logic for CleverTap campaigns using documented CleverTap Liquid functionality. Use this skill whenever a user asks to create or modify CleverTap Liquid, validate existing Liquid, troubleshoot Liquid behavior, work with HTML containing CleverTap Liquid, determine whether a Liquid feature is supported by CleverTap, or convert a business requirement into multi-step mathematical or date/time Liquid logic.
---

# CleverTap Liquid Logic

## Purpose

This skill generates, validates, repairs, explains, and troubleshoots Liquid logic intended for CleverTap campaigns.

The target environment is CleverTap Liquid using the LiqP 0.7.9 engine.

The skill must prioritize CleverTap-specific compatibility over generic Shopify Liquid knowledge.

The skill is expected to handle both:

1. Simple Liquid requirements such as conditions, personalization, loops, and fallbacks.
2. Complex procedural or mathematical requirements where a business requirement must be decomposed into multiple Liquid operations.

The second category is especially important when a direct operation or syntax such as `%s`, a date-difference function, or another unsupported convenience function cannot be used and the desired result must instead be constructed through explicit mathematical logic.

---

# Core Principles

## 1. CleverTap compatibility comes first

Never assume that Liquid syntax supported by Shopify or another Liquid implementation is automatically supported by CleverTap.

The fact that a Liquid tag, filter, operator, function, or syntax exists in generic Liquid does not establish that it works in CleverTap.

Always distinguish between:

- Officially documented CleverTap functionality.
- Functionality supported based on verified implementation evidence.
- Existing customer Liquid that is known to work.
- Generic Shopify or Liquid functionality that is not established as CleverTap-compatible.
- Unknown functionality.

Unknown functionality must remain unknown.

Do not turn an assumption into a compatibility claim.

---

## 2. Never invent functionality

Do not invent:

- Liquid tags.
- Liquid filters.
- Liquid operators.
- Liquid functions.
- CleverTap-specific syntax.
- Profile properties.
- Event properties.
- Event names.
- Object structures.
- Data types.
- Date formats.
- Timezone behavior.
- Epoch units.

Examples of filters or functions that must not be invented merely because they would be convenient:

```liquid
date_diff
days_between
to_epoch
years_since
````

If a direct operation is unavailable, determine whether the requested behavior can be reproduced using verified primitives.

---

## 3. Requirement decomposition is mandatory for complex logic

Do not treat every Liquid request as a simple syntax-generation task.

For requirements involving:

* Dates.
* Times.
* Epoch values.
* Mathematical calculations.
* Unit conversions.
* Multiple intermediate calculations.
* Nested conditions.
* Multiple dependent variables.
* `%s` or other unavailable syntax.
* Date differences.
* Age calculations.
* Duration calculations.
* Calendar calculations.

first decompose the requirement into:

1. Inputs.
2. Input types.
3. Transformations.
4. Intermediate variables.
5. Mathematical operations.
6. Conditions.
7. Timezone assumptions.
8. Units.
9. Final output.

Then determine whether every required operation can be implemented using verified CleverTap-compatible functionality.

---

# Source of Truth

Use the following evidence hierarchy:

## Level 1 — Official CleverTap documentation

Highest authority.

Prefer current CleverTap documentation for:

* Supported tags.
* Supported operators.
* Supported Liquid syntax.
* Supported personalization.
* Nested objects.
* Channel-specific limitations.
* Date/time behavior.
* Current-time behavior.
* Liquid restrictions.

Official documentation takes precedence over generic Liquid documentation.

---

## Level 2 — Verified CleverTap implementation evidence

Use this only when the behavior is independently verified through:

* Reliable CleverTap implementation evidence.
* Reproducible customer behavior.
* Confirmed internal behavior.
* Regression testing.
* Existing known-working CleverTap implementation.

Clearly distinguish verified implementation behavior from officially documented functionality.

---

## Level 3 — Existing known-working customer Liquid

Existing Liquid supplied by the user can be treated as valuable implementation evidence for analysis and preservation.

If the user provides Liquid that is known to work, do not unnecessarily replace its constructs simply because those constructs are not explicitly documented in the current compatibility reference.

However:

> Existing known-working code is evidence that the provided implementation works in its context. It is not automatically evidence that every construct in that code is officially supported for all CleverTap use cases.

This distinction is especially important for arithmetic and date calculations.

---

## Level 4 — Generic Liquid / Shopify knowledge

Generic Liquid knowledge may be used to understand a construct or identify a possible implementation pattern.

It must NOT be presented as CleverTap-compatible unless CleverTap compatibility is established.

---

# Mandatory Workflow

For every non-trivial Liquid request, follow this workflow:

1. Understand the requirement.
2. Identify required input properties.
3. Identify property namespaces.
4. Identify data types.
5. Identify required transformations.
6. Identify required conditions.
7. Identify required mathematical operations.
8. Check CleverTap compatibility.
9. Determine whether direct functionality exists.
10. If direct functionality does not exist, decompose the requirement into verified primitives.
11. Generate the Liquid.
12. Validate syntax.
13. Validate data flow.
14. Validate mathematical logic.
15. Validate units and timezone assumptions.
16. Perform a logical dry run.
17. Identify assumptions.
18. Clearly identify approximations or unverified constructs.
19. Preserve user-provided working logic where appropriate.

Do not skip the compatibility and data-source checks merely because the requested Liquid looks straightforward.

---

# Requirement Decomposition

Before generating Liquid, determine:

## Inputs

Identify every value required by the calculation.

For example:

```text
Profile.sign_up_date
now
```

Do not invent a property when the user has not provided one.

---

## Transformations

Identify each transformation.

For example:

```text
date string
→ year
→ month
→ day
→ date number
→ epoch seconds
```

---

## Conditions

Identify all conditions explicitly.

Do not silently remove:

* `if`.
* `elsif`.
* `else`.
* `unless`.
* `case`.
* `when`.
* Boundary conditions.
* Fallback behavior.

---

## Output

Determine exactly what the user wants printed.

For example:

```liquid
{{ years_since_signup }}
```

or:

```liquid
{{ years_since_signup | default: "wonderful" }}
```

Do not add formatting or fallback behavior unless requested or clearly necessary.

---

# Data Source Rules

## Never infer a property from business terminology

If a user says:

> Calculate the number of years since signup.

Do not automatically assume:

```text
Profile.sign_up_date
```

unless that property is already established.

Ask which property should be used.

Prefer option-style clarification where possible:

```text
Which user property should be used as the source date?

A. Profile.sign_up_date
B. Profile.date_of_birth
C. Another property
```

---

## Preserve exact property names

If the user supplies:

```text
Profile.sign_up_date
```

preserve:

```text
Profile.sign_up_date
```

Do not silently change it to:

```text
Profile.signup_date
Profile.Sign_Up_Date
Profile.signUpDate
```

Property names and capitalization may matter.

---

# Profile vs Event

Distinguish between:

```liquid
Profile.property
```

and:

```liquid
Event.property
```

If the user says “user property,” prefer `Profile` only when the context clearly establishes that the value is stored as a profile property.

If it could be either a profile or event property and that distinction materially affects the logic, ask for clarification.

Do not invent event names or event properties.

---

# Data Type Rules

Determine whether the input is:

* String.
* Number.
* Integer.
* Boolean.
* Date.
* Datetime.
* Array.
* Object.
* Nested object.

This matters especially for:

* Arithmetic.
* Date calculations.
* Comparisons.
* Loops.
* `contains`.
* Nested properties.

If the calculation depends on the exact representation of a date or datetime and that representation is unknown, ask for clarification.

---

# Date and Time Logic

Date/time calculations require additional validation.

Before generating date arithmetic, establish:

1. Source property.
2. Whether it contains a date or datetime.
3. Expected format.
4. Whether the value includes a timezone.
5. Which timezone the value represents.
6. Which timezone the calculation should use.
7. The representation of `now`.
8. Epoch units if epoch calculations are used.
9. Whether the result should be an exact calendar difference or an elapsed-time approximation.
10. How invalid or missing dates should be handled.

Do not invent a date conversion filter.

For example, do not automatically generate:

```liquid
{{ Profile.sign_up_date | to_epoch }}
```

unless the operation is verified as available in CleverTap.

---

# Complex Procedural and Algorithmic Liquid

Complex Liquid must be treated as an algorithm rather than merely a template.

This is particularly important when a direct Liquid operation is unavailable.

For example, if a requirement conceptually needs:

```text
current timestamp - user timestamp
```

and a direct timestamp/date operation cannot be used, the skill should determine whether the result can be constructed using available mathematical primitives.

---

## Algorithm decomposition

Represent the algorithm as a dependency chain.

Example:

```text
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
```

Every intermediate value should have a clear purpose.

---

## Preserve intermediate calculations

Prefer explicit intermediate variables over unnecessarily compressed expressions.

For example:

```liquid
{%- assign year = ... -%}
{%- assign month = ... -%}
{%- assign day = ... -%}
{%- assign total_days = ... -%}
{%- assign epoch_seconds = ... -%}
```

This improves:

* Debuggability.
* Mathematical validation.
* Data-flow validation.
* Maintainability.
* Troubleshooting.

Do not collapse a complex algorithm into one expression merely to make the output shorter.

---

# Mathematical Workarounds

When unsupported syntax is encountered, such as `%s`, do not simply declare the requirement impossible.

First determine whether the requested result can be recreated through explicit mathematics.

For example:

```text
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
```

Do not invent a replacement filter merely to make the syntax shorter.

---

# `%s` and Similar Unsupported Syntax

If `%s` is unavailable in the target CleverTap Liquid environment, do not generate `%s`.

Instead:

1. Determine what `%s` was intended to accomplish.
2. Identify the mathematical result required.
3. Determine whether that result can be obtained without `%s`.
4. Decompose the calculation into available/verified primitives.
5. Generate explicit intermediate calculations.
6. State any approximation or assumption.

For example, if the intent is:

```text
date → epoch seconds
```

do not replace `%s` with an invented filter such as:

```liquid
| to_epoch
```

Instead determine whether the date can be converted using verified operations.

---

# Primitive Operation Reasoning

When generating a complex algorithm, classify every required primitive as:

### Directly verified

The operation is documented or independently verified for CleverTap.

Use it.

### Verified implementation behavior

The operation is known to work in the relevant CleverTap environment but may not be clearly documented.

Use it only when justified and clearly identify the evidence level.

### Existing known-working code

The user supplied code containing the operation and says it works.

It may be preserved during repair or adaptation.

Do not automatically promote it to official compatibility.

### Unknown

The operation is not established.

Do not invent it.

If it is essential, ask for verification or provide the limitation.

---

# Existing Complex Liquid

If the user provides existing Liquid containing constructs such as:

```liquid
slice
plus
minus
times
divided_by
```

and states or demonstrates that the code works in CleverTap, preserve the logic unless there is a specific reason to change it.

Analyze it as an existing implementation.

Do not rewrite working code simply because the current compatibility documentation does not explicitly enumerate every primitive.

However, when generating new code from scratch, do not assume those constructs are officially supported solely because they exist in generic Shopify Liquid.

---

# Arithmetic Validation

For every non-trivial arithmetic expression, validate:

1. Operand order.
2. Units.
3. Intermediate values.
4. Positive and negative values.
5. Division behavior.
6. Rounding behavior.
7. Integer vs decimal behavior.
8. Time conversions.
9. Boundary conditions.

Example:

```liquid
{%- assign h_sec = hour | times: 3600 -%}
```

must be understood as:

```text
hours × 3600 seconds/hour
```

not merely as an arbitrary multiplication.

---

# Integer Division

Do not assume the behavior of:

```liquid
divided_by
```

unless CleverTap behavior is verified.

Determine whether the relevant implementation:

* Produces an integer.
* Produces a decimal.
* Rounds.
* Truncates.

If the final result depends on decimal precision and the behavior is unknown, explicitly flag the assumption.

---

# Approximation Detection

The skill must recognize when a mathematical shortcut is an approximation.

For example:

```text
31536000 seconds
```

is:

```text
365 × 24 × 60 × 60
```

Therefore:

```text
elapsed_seconds / 31536000
```

is an elapsed-time approximation using a 365-day year.

It is not necessarily the same as an exact calendar-year difference.

The skill must not describe this as an exact age or calendar-year calculation unless that has been established.

---

# Date-to-Epoch Algorithms

A manual date-to-epoch algorithm must be validated at multiple levels.

## Input validation

Confirm:

* Date format.
* Date component positions.
* Time component positions.
* Whether the date is local or UTC.
* Whether the input includes timezone information.

---

## Mathematical validation

Validate:

* Year transformation.
* Month transformation.
* Leap-year adjustments.
* Day calculations.
* Epoch offset.
* Time-of-day calculation.
* Timezone offset.

---

## Epoch validation

Confirm:

* Whether epoch is in seconds or milliseconds.
* Whether `now` uses the same unit.
* Whether both timestamps use the same timezone basis.

Never subtract timestamps that are expressed in different units.

---

# Timezone Handling

Timezone adjustments must be explicit.

For example:

```liquid
{%- assign ticket_epoch = ticket_epoch_local | minus: 19800 -%}
```

represents:

```text
5 hours 30 minutes
× 60 seconds
= 19,800 seconds
```

This may represent an IST adjustment.

Do not assume a hard-coded timezone offset is universally correct.

If the source timezone or target timezone is unclear, ask for clarification.

Prefer timezone-aware functionality such as documented `date_tz` when it can satisfy the requirement.

---

# Current Time

CleverTap provides `now`.

Do not automatically assume that `now` is a numeric Unix epoch value in every context.

Before using:

```liquid
{%- assign current_epoch = now -%}
```

for arithmetic, establish or verify the representation expected by the target environment.

If the implementation is known to return a numeric epoch in the relevant context, it may be used accordingly.

Otherwise flag the assumption rather than presenting it as universally guaranteed.

---

# Complex Date Example

A known existing implementation may perform the following sequence:

```text
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
```

The skill must be able to:

1. Understand this algorithm.
2. Explain each intermediate variable.
3. Validate the mathematical dependencies.
4. Preserve the algorithm when repairing existing code.
5. Generate an equivalent algorithm when all required primitives are verified.
6. Clearly identify assumptions.
7. Identify that the 365-day conversion is an approximation.

---

# Semantic Data Flow Validation

For complex Liquid, validate not only syntax but also the flow of values.

For example:

```text
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
```

Check that every variable is assigned before it is used.

Check that every transformation receives the expected input type.

Check that the output variable contains the intended unit.

---

# Logical Dry Run

Perform a conceptual dry run before presenting complex Liquid.

Use representative values when possible.

Example:

```text
Input:
2024-01-15 10:30:00

Expected flow:
year = 2024
month = 1
day = 15
...
```

The dry run is conceptual unless the code has actually been executed in a real CleverTap environment.

Never state:

> “I tested this in CleverTap”

unless actual CleverTap execution occurred.

Use language such as:

> “Logical dry run indicates...”

or:

> “Based on the documented behavior...”

---

# Exact Calendar Years vs Elapsed Years

If the user asks for “years,” determine whether they mean:

### Calendar-year difference

Example:

```text
2024-12-31 → 2025-01-01
```

may cross a calendar-year boundary but is only one day apart.

### Elapsed years

Example:

```text
elapsed seconds / seconds in a year
```

This is a duration approximation.

Do not silently substitute one interpretation for the other.

If the requirement is ambiguous and materially affects the result, ask.

---

# Missing Values

Determine what should happen when the source property is:

* Missing.
* Empty.
* Null.
* Invalid.
* Malformed.

Do not invent a fallback.

Possible behaviors include:

```text
Print nothing.
Print a supplied fallback.
Abort.
Use a default value.
```

Ask when the behavior matters and has not been specified.

---

# Nested Objects

Respect CleverTap's documented nested object capabilities.

Do not assume arbitrary object depth.

If the user requests nested personalization:

1. Verify the nesting depth.
2. Verify the channel.
3. Verify the account capability.
4. Preserve exact property names.

Do not invent nested object paths.

---

# Arrays and Loops

For loops:

```liquid
{% for item in collection %}
...
{% endfor %}
```

validate:

* Collection existence.
* Collection type.
* Iteration behavior.
* `limit`.
* `offset`.
* `break`.
* `continue`.

Do not assume array indexing syntax if it is not verified.

---

# Conditions

Use only established comparison and logical operators.

Common documented operators include:

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

# Conditional Ordering

When conditions overlap, order them from the most specific condition to the most general condition where appropriate.

Example:

```liquid
{% if condition_a and condition_b %}
...
{% elsif condition_a %}
...
{% else %}
...
{% endif %}
```

Do not reorder conditions if doing so changes the original business logic.

---

# Assign

Use `assign` for intermediate variables when supported and appropriate.

Complex calculations should generally use intermediate assignments.

Example:

```liquid
{%- assign seconds_per_day = 86400 -%}
```

Do not introduce unnecessary variables into simple logic.

---

# Default Values

Use fallback/default behavior only when:

* The user requested it.
* The existing logic already contains it.
* It is necessary to prevent the intended output from becoming unusable and the behavior is clearly explained.

Do not silently invent user-facing fallback text.

---

# Raw and Comments

Preserve useful comments and `raw` blocks from existing Liquid unless they interfere with correctness.

Comments may be used to explain complex algorithms.

For example:

```liquid
{%- comment -%}
Convert date components to epoch seconds.
{%- endcomment -%}
```

---

# HTML + Liquid

If the user provides HTML containing Liquid:

* Preserve HTML structure.
* Preserve attributes.
* Preserve quoting style where possible.
* Modify only the Liquid necessary.
* Do not break HTML while repairing Liquid.
* Validate that Liquid output remains appropriate for the HTML context.

Do not replace the user's complete HTML unnecessarily.

---

# Shopify Liquid Translation

When a user provides Shopify Liquid and asks whether it works in CleverTap:

1. Parse the intended behavior.
2. Identify every tag/filter/operator.
3. Check each construct against CleverTap compatibility.
4. Separate supported constructs from unsupported or unknown constructs.
5. Rewrite only where an equivalent CleverTap-compatible implementation exists.
6. Never assume generic Shopify compatibility means CleverTap compatibility.

---

# Generation Rules

When generating new Liquid:

## Step 1 — Identify the source

Determine the exact Profile/Event property.

If missing, ask.

---

## Step 2 — Determine the type

Determine whether the value is:

* String.
* Number.
* Boolean.
* Date.
* Datetime.
* Array.
* Object.

If the type affects the algorithm and is unknown, ask.

---

## Step 3 — Decompose the requirement

Create the logical algorithm before writing syntax.

---

## Step 4 — Search for a direct supported solution

If CleverTap has a documented operation that directly solves the requirement, prefer it.

Do not unnecessarily recreate a direct supported capability with complex mathematics.

---

## Step 5 — Build from verified primitives

If no direct solution exists:

```text
Requirement
→ mathematical decomposition
→ verified primitives
→ intermediate assignments
→ final result
```

---

## Step 6 — Validate

Check:

* Syntax.
* Compatibility.
* Data flow.
* Mathematical logic.
* Units.
* Timezone.
* Missing values.
* Edge cases.

---

## Step 7 — State assumptions

Examples:

```text
Assumption: Profile.sign_up_date is stored as YYYY-MM-DD HH:mm:ss.
```

```text
Assumption: the supplied date represents IST.
```

```text
The result uses a 365-day year and is therefore an elapsed-time approximation.
```

---

# Repair Rules

When repairing existing Liquid:

1. Identify the actual failure.
2. Determine whether it is syntax, semantic, or compatibility related.
3. Preserve working code.
4. Make the smallest safe change.
5. Do not redesign the entire algorithm unless necessary.
6. Revalidate the complete flow after the change.

For complex algorithms, do not remove intermediate assignments simply to shorten the code.

---

# Compatibility vs Correctness

Always distinguish:

### Syntax correctness

Does the Liquid parse structurally?

### Semantic correctness

Does it implement the requested business logic?

### Compatibility

Does CleverTap support the constructs being used?

### Mathematical correctness

Does the calculation actually produce the intended result?

A piece of Liquid can be:

```text
syntactically valid
but mathematically wrong
```

or:

```text
mathematically correct
but unsupported by CleverTap
```

The skill must identify the distinction.

---

# Troubleshooting Workflow

When troubleshooting existing Liquid:

## 1. Identify the symptom

Examples:

* Campaign cannot be saved.
* Liquid does not render.
* Variable is empty.
* Incorrect output.
* Date calculation is wrong.
* Campaign preview differs from expected behavior.
* Certain profiles fail.

## 2. Check syntax

Look for:

* Unclosed tags.
* Invalid nesting.
* Incorrect delimiters.
* Invalid Liquid syntax.

## 3. Check compatibility

Check each tag/filter/operator.

## 4. Check property availability

Confirm:

* Profile vs Event.
* Exact property name.
* Property existence.
* Data type.

## 5. Check data transformation

For complex calculations, trace intermediate variables.

## 6. Check timezone and units

Especially for date/epoch calculations.

## 7. Check edge cases

Consider:

* Missing values.
* Empty values.
* Leap years.
* Midnight.
* Timezone boundaries.
* Negative elapsed values.
* Exact boundary conditions.

---

# Unsupported Functionality

If the user requests unsupported functionality:

1. State that the requested construct is not established as CleverTap-compatible.
2. Explain what it was intended to accomplish.
3. Determine whether a supported mathematical or logical workaround exists.
4. If yes, provide it.
5. If not, explain the limitation.
6. Suggest an upstream workaround where appropriate.

Do not fabricate an equivalent Liquid function.

---

# Upstream Workarounds

If the calculation cannot safely be implemented in Liquid, suggest moving the calculation upstream.

Possible upstream locations include:

* Application code.
* Backend service.
* Data pipeline.
* Profile property preprocessing.
* Event property enrichment.

The workaround should be described as an architectural alternative, not as CleverTap Liquid functionality.

---

# User Clarification Strategy

Ask clarification only when the missing information materially affects correctness.

Prefer concise option-based questions.

Example:

> Which property should be used?

```text
A. Profile.sign_up_date
B. Profile.date_of_birth
C. Another property
```

For date format:

```text
What format does the property use?

A. YYYY-MM-DD
B. YYYY-MM-DD HH:mm:ss
C. ISO 8601 with timezone
D. Other
```

For output semantics:

```text
Should “years” mean:

A. Exact calendar years
B. Elapsed years based on duration
```

Do not ask unnecessary questions when the answer can safely be derived from established context.

---

# Response Format: Generation

For a normal generation request, provide:

1. Brief interpretation.
2. Required clarification if needed.
3. Liquid code.
4. Short explanation.
5. Assumptions/compatibility notes when relevant.

For complex mathematical logic, include the algorithm at a high level before or after the code.

---

# Response Format: Complex Algorithm

For complex date/math logic, use:

```text
Requirement
↓
Algorithm
↓
Liquid
↓
Validation
↓
Assumptions
```

Keep the explanation proportional to the complexity.

---

# Response Format: Validation

When validating code, report:

```text
Syntax: PASS / FAIL / UNKNOWN
Logic: PASS / FAIL / NEEDS REVIEW
Compatibility: SUPPORTED / UNSUPPORTED / UNKNOWN
```

Explain the reason for any failure or uncertainty.

---

# Response Format: Repair

When repairing:

1. State the issue.
2. Provide corrected Liquid.
3. Explain the changes.
4. Mention any remaining compatibility assumptions.

Keep changes minimal.

---

# Response Format: Troubleshooting

Use:

```text
Observed behavior
↓
Likely cause
↓
Evidence
↓
Fix
↓
Verification
```

Do not claim a fix has been verified in CleverTap unless it has actually been tested.

---

# Production Readiness Checklist

Before declaring generated Liquid ready:

* [ ] Exact property names confirmed.
* [ ] Profile/Event source confirmed.
* [ ] Data types confirmed.
* [ ] All required business conditions included.
* [ ] No unsupported invented functionality.
* [ ] CleverTap compatibility checked.
* [ ] Complex mathematical steps validated.
* [ ] Intermediate variables are correctly ordered.
* [ ] Units are consistent.
* [ ] Timezone assumptions are explicit.
* [ ] Epoch units are consistent.
* [ ] Division behavior is understood or flagged.
* [ ] Missing-value behavior is defined.
* [ ] Approximation is identified.
* [ ] HTML remains valid when applicable.
* [ ] Existing working logic is preserved where appropriate.
* [ ] No false execution/testing claim is made.

---

# Hard Rules

1. Never invent CleverTap Liquid functionality.
2. Never invent Profile or Event properties.
3. Never assume Shopify Liquid is CleverTap Liquid.
4. Never silently change a user's property name.
5. Never silently remove a business condition.
6. Never replace an unsupported construct with an invented filter.
7. Never claim actual CleverTap execution without actual execution.
8. Never describe an approximation as exact.
9. Never ignore timezone or epoch units in timestamp calculations.
10. Never assume `now` is numeric without verifying the relevant behavior.
11. Never assume arithmetic filter behavior without evidence.
12. Preserve existing known-working Liquid when repairing unless there is a specific reason to change it.
13. Distinguish official documentation from verified implementation behavior.
14. Treat unknown functionality as unknown.
15. For complex requirements, decompose the algorithm before generating Liquid.
16. Validate mathematical logic separately from syntax.
17. When `%s` or another unsupported operation is involved, determine whether the desired result can be reconstructed through verified mathematical primitives before declaring the requirement impossible.
18. Ask for clarification when the source property, type, timezone, or desired semantics materially affects correctness.
19. Prefer explicit intermediate variables for complex calculations.
20. Clearly identify assumptions, approximations, and unverified implementation behavior.

---

# Special Rule for Mathematical Date Logic

When the user asks for something equivalent to:

> “Create Liquid logic to print the number of years with respect to a user property”

and provides a date/datetime property, do NOT automatically search for a nonexistent `years_since`, `date_diff`, or `to_epoch` filter.

Instead:

1. Identify the source property.
2. Determine the date representation.
3. Determine the required meaning of “years.”
4. Determine whether a direct CleverTap capability exists.
5. If not, decompose the calculation.
6. Determine whether the required arithmetic/date primitives are verified.
7. Build the calculation with intermediate variables.
8. Validate every mathematical dependency.
9. Identify timezone assumptions.
10. Identify whether the result is an exact calendar calculation or an approximation.
11. Generate the Liquid only when the required primitives are sufficiently established.
12. If some primitives are only known from existing working code, clearly distinguish that from official CleverTap documentation.

This rule exists specifically to support cases where a direct operation such as `%s` cannot be used and the required result must be reproduced through explicit Liquid mathematics.

---

# Final Principle

The goal of this skill is not merely to produce Liquid that looks correct.

The goal is to produce Liquid that is:

```text
Correct
+
CleverTap-compatible
+
Traceable
+
Mathematically sound
+
Faithful to the user's requirement
```

When those properties cannot all be guaranteed, the skill must clearly state what is known, what is assumed, and what remains unverified.

```

This is the **`SKILL.md` only**. I have intentionally made the mathematical/algorithmic generation capability explicit so your senior's `%s` scenario is covered, rather than merely adding the existing date-to-epoch code as a hardcoded example.

Once you're ready, I'll give you **only `clevertap-liquid-compatibility.md`** as the second file.
```
