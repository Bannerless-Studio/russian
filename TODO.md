# TODO

Residuals after QA round 2 (word sample seed 31, sentence sample seed 32).
See `tools/REPORT.md` for the rules already in place.

## Words
- Predicatives (нужно, надо, можно, нельзя, жаль) ship as adverbs. A
  "predicative" POS would need an engine-side label; the gloss carries the
  meaning for now.
- есть has one entry, "to eat". Its "is / there is" use (У меня есть...)
  links to быть, which is right, but learners never see есть = "there is"
  as a word of its own.
- ё/е ambiguity inside verb forms: узнаем (pf. future of узнать) and узнаём
  (impf. present of узнавать) fold together, and the tagger picks one.
- Sense order for some B1 homonyms may still lead with a rare sense. All
  A1+A2 glosses were hand-skimmed in round 2; B1 was only sampled.
- Comparatives tagged ADJ (выше, быстрее) link to the adjective or the -о
  adverb depending on the tag. Both are defensible; neither is taught as
  a separate form.
- Hyphenated compound nouns (штаб-квартира) are split by spaCy unless the
  whole word is a Wiktionary headword that also reaches the frequency list.

- A1 sentence difficulty: about 15% of A1 example sentences go beyond A1
  grammar, mostly бы/будь conditionals and subordinate clauses. The A1
  ranker penalises participles, который, oblique cases and rare words, but
  not conditionals.
- убить and убийство sit at A1 by subtitle frequency. Their example
  sentences are kept to B1 by the violence filter; the words stay.
- Minor losses from the v2 reselection (not in the pack any more):
  далёкий, поздний, редко, бесплатный, запад.
- Audio coverage is 33.6% of sentences (v1 34.6%). The bare-form example
  rule adds sentences without audio; a Tatoeba audio request list would
  help.

## Sentence links
- что in "что это за ..." / "Что за ..." and in "что ты хочешь ..." is
  sometimes tagged as the conjunction instead of the pronoun.
- "ни" flips: "ни о чём" and "Сколько ни ..." are sometimes tagged CCONJ
  instead of the particle (both link to ни, the POS entry differs).
- Set phrases: "с тех пор" links пора ("it is time"); a phrase entry would
  be better.
- Some sentences with fewer than 4 tokens still appear at A1 (3 of 812),
  through the shared fallback for A1 words with fewer than 2 candidates.

## Reading passages
- A native-speaker pass over the 60 texts has not been done yet; only an
  automated QA pass plus manual QA rounds (see `tools/REPORT_passages.md`).
- The pack is subtitle-ranked, so several everyday words are missing from
  the word list (кафе, торт, бегать, советовать, по-русски); the 60 texts
  avoid them. Adding these belongs in a v2 word-list pass, not the passage
  linker.
- 21 out-of-pack lemmas remain across the 60 passages, each needed by its
  topic and declared with a reason (recipe steps, собеседование, спектакль,
  зонт, приют, ёлка...); unlinked tokens otherwise are names, quoted
  titles, and the numerals двухсот/оба.
- The Russian passage linker rules (adjective/numeral surface reads as a
  pack adverb, меньше -> мало, стоит/стоят disambiguation, "Тому, кто",
  "Новый год", "Целую" sign-off, "друг другу" no-link, ellipsis sentence
  breaks) live in vocab-engine's `packbuilder/langs/ru.py` passage hooks,
  not in this repo.
- Full manual QA notes and per-passage coverage/link numbers are in
  `tools/REPORT_passages.md`.

## Engine / publishing
- vocab-engine must be committed with langs/ru.py and the new hooks before
  `./check.sh` passes without `PACKBUILDER_PATH=../vocab-engine/tools`.
  Then run `git submodule update --remote engine` and rebuild.
- Browser verification is still to do: ru-RU TTS voice, typing without ё or
  stress, the pron toggle showing stress marks, and audio playback.
- Republish 09e90bc: sentence spans (19114/19118 linked words placed); inflected forms now cloze targets.
