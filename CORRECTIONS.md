# Corrections log

Every value Statbook has ever published wrongly is recorded here,
permanently, with a numbered advisory (`SB-C-NNNN`) listing the wrong value,
the right value, the affected period, and how it happened. The wrong value
also stays in the affected figure's history with a `corrected` status —
history is never rewritten.

This log is a feature, not an embarrassment: a registry that claims it never
errs is lying about something.

## Advisories

### SB-C-0001 — assurance status corrected (no value was wrong)

**Found** 2026-07-26 · **published in** release 3.0.0 · **affected releases**
1.0.1, 2.0.0

**Figures.** `employment.unfair_dismissal.compensatory_cap`,
`employment.unfair_dismissal.min_basic_award`

**What was published.** Both figures were published with the assurance status
`corroborated` — Statbook's claim that two independent official sources state
the value. The second source was the gov.uk guidance page
<https://www.gov.uk/dismissal/compensation-and-tribunals>, and that page had
already been removed.

**What is right.** `single_source_A`: the value is stated by primary
legislation only. Both figures are set by the Schedule to SI 2026/310, and that
citation was, and remains, correct. The numbers themselves — £123,543.00 for
the compensatory cap and £9,157.00 for the minimum basic award, both from
6 April 2026 — were right in every release and are unchanged.

**Affected period.** From release 1.0.1 (compiled 2026-07-09) to release 3.0.0.
The overstatement was found on 2026-07-26 and corrected in the source data that
day, but 2.0.0 — the release that was live — carried it until 3.0.0 replaced
it.

**How it happened.** The corroboration gate checked the shape of the sources,
not what they said: two source roles present, the guidance source on an
official domain. It never asked whether the archived bytes of that guidance
page actually stated the figure. The page had been retired — the cited URL
redirects to <https://www.gov.uk/dismissal>, a guide that states neither figure
and carries no money amount at all, and every archived copy Statbook holds of
the original URL is a navigation shell that never stated either value. Nightly
re-verification did notice something, every night, but filed it as
`changed-value-unconfirmed`: the deliberately silent finding meaning "this
matcher has never matched this page", which cannot tell a wording gap from a
citation that is simply dead. So a published two-source claim rested on a page
that stated neither number, and nothing said so.

**The fix.** From 3.0.0 both figures publish as `single_source_A` and the dead
guidance source is dropped; no value changes. Two guards were added so the same
overstatement cannot ship again: the build now refuses a `corroborated` status
whose own archived source bytes do not state the value (Gate 2), and the
nightly escalates a cited page that states nothing of the value's kind — no
currency or percentage figure anywhere on it — into an alert instead of a
silent note. No value entry is marked `corrected`, because no value was wrong:
the protocol's `corrected` marker is for values.
