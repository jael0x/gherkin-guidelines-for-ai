# Gherkin Guidelines for AI

Adapted from AutomationPanda/gherkin-guidelines-for-ai (v1.0.0). This is the quality standard for all Gherkin specs this skill produces.

## Guiding Principles

1. **Behavior-first**: Describe *what* the system does, never *how* it is implemented.
2. **Human readability**: Scenarios read like specifications any engineer or stakeholder can understand.
3. **Specification by example**: Use concrete, realistic data in scenarios.
4. **One behavior per scenario**: Each scenario covers exactly one behavior.
5. **Independence**: Every scenario runs in isolation, no dependency on other scenarios.

## File and Organization

- Save Gherkin in `*.feature` files using **kebab-case** naming.
- One `Feature` block per file. Multiple `Scenario`/`Scenario Outline` blocks are fine.
- Consolidate feature files in `specs/` directory. Optionally organize into sub-directories by functional area.

## Formatting

- **2-space indentation** for all block bodies.
- **Lines under 120 characters**.
- **One blank line** between major sections and between adjacent scenarios.
- **No blank lines** between steps within a scenario or background.
- **Avoid comments**; let scenario text be self-explanatory.

## Vocabulary

- Use a **stable, consistent vocabulary** across all features.
- Never swap synonyms unless the product genuinely distinguishes between terms.
- Match the project's existing domain language.

### Cross-file consistency

- **No contradictions**: Two specs MUST NOT define conflicting behavior for the same domain concept (e.g., different timeout values, contradictory validation rules, incompatible state transitions).
- **No duplicate coverage**: If a behavior is already specced in one feature file, do not re-spec it in another with different wording. Reference the existing spec or consolidate.
- **Single source of truth**: Each behavior is owned by exactly one feature file. If multiple features touch the same domain object, ensure their scenarios cover distinct behaviors, not overlapping ones.

## Feature Block

- Name the Feature after the behavior area it covers.
- Align the file name with the Feature title (kebab-case).
- Keep the Feature title to a single line.
- Include a user story beneath the title:
  ```
  As a <role>
  I want <goal>
  So that <reason>
  ```

## Background Section

- Optional. Use only for starting state shared by *all* scenarios in the file.
- Maximum one Background per Feature.
- Do not use Background when only one scenario needs that setup.
- Keep backgrounds short; if they grow, split the feature file.

## Scenario Structure

- Each Scenario gets a **single-line, behavior-focused title**.
- Steps MUST be **chronologically executable**.
- Use **declarative** language (describe what, not how).
- Maintain **domain/business-level abstraction** using product language.
- Use **concrete, realistic example values** (real names, amounts, dates). Never `foo`, `bar`, `test123`.
- Target **fewer than 10 steps**; if longer, consider splitting or using tables.

### Scenario Outline

Use only when the same behavior needs multiple input/output variations. Prefer plain `Scenario` when inputs don't materially change the behavior.

## Step Language

- **Third person, present tense**.
- **Subject-predicate phrasing**.
- Proper English grammar and spelling.
- **Double quotes** for string parameters.
- Do not combine multiple actions/assertions with conjunctions in one step; split into separate steps.

## Given/When/Then (Arrange/Act/Assert)

- Strict **Given -> When -> Then** order. Never repeat phases within one scenario.
- `And` continues the same phase. `But` is allowed sparingly for contrast.
- **Never use `Or`** as a step keyword; use separate scenarios or Scenario Outline instead.
- **Then outcomes MUST be observable and checkable** from the scenario text. No vague "it works" or "it succeeds" without stating how.

## Tables and Structured Data

- Use **step data tables** to pass lists instead of long chains of `And` steps.
- Use concise, descriptive headers (often single-token, kebab-case).
- Keep tables to a single screen view.
- Use **doc strings** (`"""..."""`) for multiline or structured content (JSON, XML) instead of enormous quoted strings.

## Anti-Patterns (MUST avoid)

1. Mixing multiple behaviors into one scenario.
2. Bundling unrelated concerns (functional + performance + accessibility) into a single spec.
3. Encoding UI implementation details in step text (selectors, XPath, "wait for", "scroll to", element IDs).
4. Including HTTP endpoints, SQL, internal schemas, or storage mechanics (unless they ARE the behavior).
5. Over-specifying navigation and clicks when state could communicate preconditions more clearly.
6. Bloated Given chains setting up unnecessary context.
7. Vague assertions without observable signals.
8. Placeholder data that doesn't read like a real specification.
9. Overusing Scenario Outline with rows that don't provide distinct behavioral value.
10. Extremely long scenarios or tables that create a wall-of-text effect.
11. Contradictory specs across files: two features specifying conflicting outcomes for the same action or domain rule.
12. Duplicate scenarios across files: same behavior specced in multiple feature files using different wording (vocabulary drift symptom).
13. Ambiguous outcomes: `Then` steps that two reasonable readers could interpret differently, or that cannot be verified without additional context.

## Pre-Finalization Checklist

Before outputting ANY Gherkin, verify against this list:

- [ ] One behavior per scenario; can run independently
- [ ] No unrelated concerns bundled
- [ ] Stable vocabulary maintained
- [ ] Domain-level abstraction; no unnecessary UI/API/DB mechanics
- [ ] State prioritized over navigation where clearer
- [ ] Minimal but sufficient Given context
- [ ] Concrete, realistic example data
- [ ] Steps in third-person, present tense, subject-predicate
- [ ] Strict Given -> When -> Then order; Then outcomes observable
- [ ] No UI/automation mechanics in steps
- [ ] Blank line between scenarios
- [ ] Short scenarios (ideally < 10 steps); tables fit one screen
- [ ] 2-space indentation, lines < 120 chars
- [ ] No contradictions with existing specs in `specs/`
- [ ] No duplicate scenarios covering the same behavior in other files
- [ ] Vocabulary consistent with existing specs (not just within this file)

## Template

```gherkin
Feature: <behavior area>
  As a <role>
  I want <goal>
  So that <reason>

  Background:
    Given <shared starting state>

  Scenario: <single behavior>
    Given <context>
    And <additional context>
    When <action>
    And <continued action>
    Then <observable outcome>
    And <additional observable outcome>

  Scenario: <single behavior using a step data table>
    Given the following <domain entities> exist:
      | <column-a> | <column-b> |
      | <value 1>  | <value 2>  |
    When <action>
    Then <observable outcome>

  Scenario Outline: <same behavior, varying inputs>
    Given <context>
    When <action> with "<input>"
    Then the outcome is "<outcome>"

    Examples:
      | input   | outcome   |
      | <case1> | <result1> |
      | <case2> | <result2> |
```
