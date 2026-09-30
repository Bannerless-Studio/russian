# Russian A1-B1 vocab pack

Static data pack for a language-agnostic vocab trainer (`key: "ru"`). 2000
words spanning A1-B1, each with a short English gloss and its stressed form
(`pron`, for example соба́ка), plus example sentences with translations and,
where the licence permits, native audio.

**Live:** https://bannerless-studio.github.io/russian/

**Script primer.** An "Алфавит" stage now runs before A1 and teaches the
Cyrillic alphabet (33 units) with symbol-to-sound, recognition and
word-reading items. It's skippable with "I can read it" and reversible later
from Progress.

Open the link, pick a level (or take the placement test), and start a Today
session: short rounds of flashcard-style review mixed with new words, plus a
Read tab with short passages and comprehension questions, and typing practice
for spelling. Progress (what you've seen, what's due for review) is saved in
your browser only, and can be exported/imported as a file to move between
devices. The site works offline once loaded (it registers a service worker).

**Scope note:** this app gives the vocabulary base for B1: words, glosses,
and example sentences with audio. TORFL-1 also needs grammar, writing and
speaking practice, which this app does not teach.

**Data quality:** after QA round 2, the hand-checked samples are as follows. A 60-word stratified sample (seed 31) has 60/60 correct primary senses, and every A1 and A2 gloss was hand-skimmed. There are 0 wrong parts of speech in the top 300 by rank. A 90-sentence sample (seed 32) has 4 wrong word links out of 523 (99.2%), and 86/90 sentences are fully correct. All 2,000 words carry `pron` (1,716 with a stress mark; one-vowel and ё words need none). Every word has at least one example sentence, and 1,076 of 3,203 sentences have permissive native audio. Word ids are frozen in `tools/id_map_v1.json`, so learner progress survives rebuilds. Known residuals are listed in `TODO.md`, and the rules and counts are in `tools/REPORT.md`.

**Content policy:** sentences on sexual content, suicide, threats/violence,
dying/death wishes or weapons are kept out of A1/A2. убить, убийство and
стрелять sit at B1 under the shared word-level ceiling, and their example
sentences are held to B1 by this filter. Sentences about rape or sexual/child abuse are
removed at every level. Four profanity words (ублюдок, сука, сучка, хер) are
cut by a hand list. See `TODO.md` for the exact rule history and counts.

## Reading passages (Read tab)

60 short reading texts, 20 each at A1, A2 and B1, with comprehension
questions each. A level's 20 passages unlock once you've learned 70% of that
level's words. Tapping any word in a passage shows its gloss, including
inflected forms. Comprehension questions feed missed words back into the
review queue as weak words. The passages and questions are machine-written,
checked by an automated QA pass rather than a native speaker.

A passage's Today spaced re-read (after 7 days) becomes a listening pass
when audio is available for every sentence: the text stays hidden and about
half the questions are audio-only.

Words show their gloss and a stressed form (`pron`, e.g. соба́ка); typing
never requires ё vs е or the stress mark.

## What's in this repo

This repo holds the Russian data pack (`pack/`) and the data files its build
reads (`tools/`), plus [`vocab-engine`](https://github.com/Bannerless-Studio/vocab-engine)
as a git submodule at `engine/`, which holds the shared UI, drill logic and
pack builder used by every language in this trainer. See `tools/README.md`
for a file-by-file breakdown of `tools/` (including how words, senses and
sentence links are chosen), and `CLAUDE.md` for the full architecture and
build commands.

## Rebuild and publish (maintainers)

```
git clone --recurse-submodules <this repo>   # or: git submodule update --init
cd russian && python3 -m venv .venv && source .venv/bin/activate
pip install -r tools/requirements.txt
python3 tools/build_pack.py && python3 engine/tools/jsonify_pack.py pack
./build.sh && ./check.sh
```

See `tools/README.md` for what each rebuild step reads/writes and `CLAUDE.md`
for the pinned commands, submodule-update flow and forbidden patterns.

## Sources and licences

| Data | Source | Licence | Used for |
|---|---|---|---|
| Spoken/subtitle frequency | [hermitdave/FrequencyWords](https://github.com/hermitdave/FrequencyWords) (`ru_full.txt`, 2018 OpenSubtitles) | CC-BY-SA 4.0 | word ranking |
| Written/general frequency | [`wordfreq`](https://github.com/rspeer/wordfreq) Python package | CC-BY-SA 4.0 (data), MIT (code) | word ranking |
| Glosses, POS, gender, aspect, stress | [kaikki.org](https://kaikki.org) Russian Wiktionary extract | CC-BY-SA 3.0 / GFDL (Wiktionary) | English glosses, POS, noun gender, verb aspect, stressed form (`pron`), inflection map |
| POS tagging / lemmatisation (build time only) | [spaCy](https://spacy.io) (MIT) with `ru_core_news_sm` 3.8.0 and [pymorphy3](https://github.com/no-plagiarism/pymorphy3) | model: MIT (trained on Nerus); pymorphy3: MIT | corpus POS, lemma and sense choice; sentence word links. The pack ships no model files. |
| Example sentences | [Tatoeba](https://tatoeba.org) `rus_sentences_detailed.tsv` | CC-BY 2.0 FR | sentence text (contributor usernames in `pack/attribution.json`) |
| Sentence translations | Tatoeba `eng_sentences.tsv` + `links.tar.bz2` | CC-BY 2.0 FR | English translations |
| Sentence audio | Tatoeba `sentences_with_audio.tar.bz2` | CC BY 4.0 / CC BY-SA / CC0, per clip; only these permissive clips are linked (no NC/ND) | `sentences.json[].audio`; recorders per licence in `pack/attribution.json` |
| CEFR cross-check (not shipped) | [kotoshu/frequency-list-kelly](https://github.com/kotoshu/frequency-list-kelly) `ru.json` | research use only | sanity check only, read from `.cache/`, never copied into `pack/` |

Licence: code MIT, pack data CC BY-SA 4.0, see LICENSE.

No source is non-commercial. The tagger model is MIT, so this pack has no
licence restriction beyond the CC-BY / CC-BY-SA attribution terms.

## Level bands

Candidate (lemma, POS) pairs are ranked by the mean of log subtitle rank and
log `wordfreq` rank. This is a reproducible proxy for CEFR level, not an
official classification. `tools/REPORT.md` includes a cross-check against the
Kelly CEFR-tagged list.
