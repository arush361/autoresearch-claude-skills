# Autoresearch: Tuning Claude Code Skills with AI

Applying [Karpathy's autoresearch methodology](https://github.com/karpathy/autoresearch) + [Hamel's evals-skills framework](https://github.com/hamelsmu/evals-skills) to autonomously optimize Claude Code skills using binary evals.

## Case Study: Product Manager Skill (59% to 97% in 3 Experiments)

The `/product-manager` skill handles gap analysis, competitor research, PRD generation, and backlog prioritization using a WINNING scoring framework (Pain, Timing, Execution, Fit, Revenue, Moat -- each /10, total /60).

### The Problem

The skill "worked" but nobody measured how reliably. I defined 6 binary evals, ran 20 tests per experiment (120 checks), and discovered the skill was only passing **59% of the time**.

### The 6 Evals

| # | Eval | Question |
|---|------|----------|
| E1 | WINNING Score Structure | All 6 criteria with /10 scores and a total /60? |
| E2 | Evidence-Backed Claims | Every score includes specific data, quotes, or competitor refs? |
| E3 | Hybrid Scoring Compliance | Claude scores Pain/Timing, explicitly asks user for the other 4? |
| E4 | Actionable Decisions | Includes FILE/WAIT/SKIP recommendation and specific next steps? |
| E5 | Template Compliance | Proper markdown tables, headers, follows reference templates? |
| E6 | Scope Discipline | Stays focused, no tangents or filler? |

### Results

```
Baseline:     71/120  (59.2%)
Experiment 1: 98/120  (81.7%)  +22.5pp  -- added projected totals
Experiment 2: 111/120 (92.5%)  +10.8pp  -- added hybrid scoring markers
Experiment 3: 117/120 (97.5%)  +5.0pp   -- handle incomplete inputs
```

| Eval | Before | After | Change |
|------|--------|-------|--------|
| E1: WINNING Structure | 5% | 90% | +85pp |
| E2: Evidence-Backed | 100% | 90% | -10pp (input edge case) |
| E3: Hybrid Scoring | 65% | 100% | +35pp |
| E4: Actionable Decisions | 35% | 100% | +65pp |
| E5: Template Compliance | 100% | 100% | maintained |
| E6: Scope Discipline | 100% | 100% | maintained |

### The Key Insight

The skill had a **"waiting problem."** It correctly implemented hybrid scoring (Claude scores Pain/Timing, user scores the other 4) but never produced projected totals or recommendations until the user replied. One paragraph -- telling the skill to stop waiting and start recommending -- was the biggest single improvement.

## Repo Structure

```
autoresearch-claude-skills/
├── README.md
├── skill-before/
│   └── SKILL.md              # Original skill (59.2% baseline)
├── skill-after/
│   └── SKILL.md              # Optimized skill (97.5% final)
└── results/
    ├── results.tsv            # Score log per experiment
    ├── results.json           # Dashboard data
    ├── changelog.md           # Detailed mutation log
    └── dashboard.html         # Live browser dashboard
```

## How to Run Autoresearch on Your Skills

1. Install the autoresearch skill in `~/.claude/skills/autoresearch/`
2. Install Hamel's evals-skills: [github.com/hamelsmu/evals-skills](https://github.com/hamelsmu/evals-skills)
3. Run: `/autoresearch [your-skill-name]`
4. Define 3-6 binary evals (yes/no only, no scales)
5. Choose 3-5 test inputs covering different use cases
6. Set runs per experiment (recommended: 20)
7. The loop runs autonomously until 95%+ or you stop it

## How It Works

```
┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
│  Run    │────>│  Score  │────>│ Analyze │────>│ Mutate  │────>│ Keep?   │
│  Skill  │     │  Output │     │ Failures│     │  Skill  │     │ Discard?│
└─────────┘     └─────────┘     └─────────┘     └─────────┘     └────┬────┘
     ^                                                               │
     └───────────────────────────────────────────────────────────────┘
                              REPEAT UNTIL 95%+
```

## Credits

- **Autoresearch methodology**: [Andrej Karpathy](https://github.com/karpathy/autoresearch)
- **Evals framework**: [Hamel Husain](https://github.com/hamelsmu/evals-skills)
- **Built with**: [Claude Code](https://claude.ai/claude-code) (Opus 4.6)
