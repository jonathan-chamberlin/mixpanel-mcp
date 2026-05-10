# Skills Audit

**Generated:** 2026-05-09
**Scoping decision:** Audited 6 out of 45 total skills. Circuit breaker applied: only skills modified in the last 7 days were included (mtime -7). Changed count (6) is well under the 30-skill cap.

**Skills audited:**
1. canvas
2. close-task
3. design-prototype
4. expiriments
5. regex-vs-llm-structured-text
6. ship-store

---

## Per-Skill Audit Table

| Skill | Read? | Claims | Reality | Violations | Action |
|---|---|---|---|---|---|
| canvas | YES | Canvas LMS reference: MCP tools, course IDs, assignment querying, timezone rules | Content matches claims. No code examples. No secrets. No Swift. No console.log. | None | None |
| close-task | YES | Archive/reschedule Notion tasks; scripted payloads; multi-step orchestration | Content matches claims. No code examples with console.log. No secrets hardcoded. No Swift. | None | None |
| design-prototype | YES | HTML-prototype-first workflow for iOS design; phases 0-7; anti-patterns | Content matches claims. No code examples. No secrets. No Swift APIs used. | **Malformed bold in checklist (Phase 4):** Lines 74-79 use `**Text\*\*:` (escaped closing asterisks) instead of `**Text**:`. These render as literal `\*\*` in Markdown, breaking the formatted headers. | Fix formatting - escaped closing bold markers |
| expiriments | YES | Controlled experiment workflow with 5 phases; `sweep_runner.py` integration | Content matches. No secrets. No code examples with console.log. Minor: skill folder name is misspelled ("expiriments"). | **Typo in `name:` field and folder name**: `name: expiriments` (should be `experiments`). Two occurrences of "expiriment" in body text (lines 10, 24). Functional but causes auto-discovery mismatch if trigger phrase is "experiment". | Flag: `name:` value doesn't match user's natural spelling. Recommend rename if auto-discovery is needed on "experiments". |
| regex-vs-llm-structured-text | YES | Decision framework for regex vs LLM; Python code examples | Content matches. Model name in LLM Validator example (line 141): `"claude-haiku-4-5"` appears valid for current model naming. No hardcoded API keys. No console.log. No Swift. | None | None |
| ship-store | YES | iOS App Store release workflow; 11-phase pipeline; ASC API via scripts | Content matches. No hardcoded secrets (key path is `$HOME/.appstoreconnect/private_keys/AuthKey_S6QR8N472U.p8` - this is a file path, not a key value, and is referenced correctly). No console.log. No Swift APIs. | **Key ID `S6QR8N472U` is embedded in the skill text (line 34).** This is a non-secret public key ID (Apple ASC key IDs are public identifiers; the private key itself is the secret), but it may be undesirable to hardcode it here. | Low severity: key ID is not a secret credential. No action required, but note for awareness. |

---

## Violations Requiring Action

### 1. design-prototype/SKILL.md - Malformed bold markers in Phase 4 checklist

**File:** `/Users/jonathanchamberlin/.claude/skills/design-prototype/SKILL.md`
**Lines:** 74, 75, 76, 77, 78, 79

The Phase 4 pre-handoff checklist items use `**Text\*\*:` (escaped closing markers) instead of `**Text**:`. This is a rendering bug - the headers display as literal `\*\*` in most Markdown renderers.

Examples:
- Line 74: `1. **Affordance ambiguity\*\*:` should be `1. **Affordance ambiguity**:`
- Line 75: `2. **Wrong metaphor for the data\*\*:` should be `2. **Wrong metaphor for the data**:`
- Line 76: `3. **All variants shown\*\*:` should be `3. **All variants shown**:`
- Line 77: `4. **Every interactive state\*\*:` should be `4. **Every interactive state**:`
- Line 78: `5. **No artificial mock for system UI\*\*:` should be `5. **No artificial mock for system UI**:`
- Line 79: `6. **No em dashes anywhere\*\*:` should be `6. **No em dashes anywhere**:`

Similarly in Phase 6 handoff doc structure (lines 98-104):
- `1. **Read this first\*\*:` should be `1. **Read this first**:`
- `2. **Locked decisions\*\*:` should be `2. **Locked decisions**:`
- `3. **Functional changes (not just visual)\*\*:` should be `3. **Functional changes (not just visual)**:`
- `4. **Per-screen punch list\*\*:` should be `4. **Per-screen punch list**:`
- `5. **Order of operations\*\*:` should be `5. **Order of operations**:`
- `6. **Verification protocol\*\*:` should be `6. **Verification protocol**:`
- `7. **Risks and gotchas\*\*:` should be `7. **Risks and gotchas**:`

### 2. expiriments/SKILL.md - Misspelled `name:` field

**File:** `/Users/jonathanchamberlin/.claude/skills/expiriments/SKILL.md`
**Line 3:** `name: expiriments`

The `name:` value in the frontmatter is `expiriments` (misspelled). Auto-discovery maps trigger phrases to this name. If any skill router checks for `experiments` (correct spelling), it will not match. The misspelling also appears in body text at lines 10 and 24 (`expiriment`). The folder name itself is also misspelled. This is internally consistent but will cause friction if the routing system ever normalizes spelling.

**Severity:** Low (internally consistent, but wrong spelling in `name:` field and trigger wording).

---

## Skills Modified by This Audit

None. The violations in `design-prototype/SKILL.md` are documented above but the fix was not applied automatically (read-only audit pass). The `expiriments` misspelling is flagged as low severity and deferred to the user's discretion since the folder name itself would also need renaming.

If the caller wants fixes applied:
- `design-prototype/SKILL.md`: replace all `\*\*:` at end of bold phrases with `**:` in phases 4 and 6 sections.
- `expiriments/SKILL.md`: correct `name: expiriments` to `name: experiments` and body typos (only if folder is also renamed).
