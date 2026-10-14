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
- Republish ef44c6e: стрелять A2->B1 (band edge: техника B1->A2); no override keys deleted; set-counter and no-voice planner fixes

## Republish 7b8c173 (2026-10-08, port wave 3)
- Republish 7b8c173: typed modes, day-aware scheduling, reading rotation, goals, pairs, frequency tiers, Progress v2, redesigned tabs, session estimates. Pack diff vs fe88110: every word gains `ft` (100 ambient / 1385 core / 515 peripheral), pack.json gains the generic flag set + `eta`; nothing else (tools/eta.json from `eta_checks --calibrate --sessions 600`: gain 0.005368 / 0.003294 / 0.002695, known 7.668, all gates PASS 3/3). check.sh runs `packbuilder enrich --check`.
- Migration proof: rollback hash fe88110dc4341ebc003ab27072bab181eb4fba30; previous live md5 index c418dd4a0e023ee8d14aff1cd47e0eaf, sw a5850f475a1170dd9904f647373d3be9. Storage: new fields day/sn/t/u/f/p/pm/pv/pause/read.done s,ls/today.tw on first use; boot writes nothing; previous build ef44c6e/aa00571 carries them (migration [port] 9/9).
- Live proof 16999c7 (2026-10-08) KEEP: 12-session seed from fe88110 (primer learned, 33 script units) byte-equal after boot/reload/Progress open (leaving Progress adds only prog.pv); one Today session writes w/s/sets/sessions/script/sn/day/pm; that record boots on fe88110 byte-equal (boot + reload), one key vocab_ru, a session runs there; back on live byte-equal; 0 console errors, 0 failed requests.
- Migration proof on engine 8745de1 (2026-10-09, scratch, live to follow): rollback hash c9c3a09ff98c2a0bf593b5958adf37ad2f6a7f91; previous live md5 index 25c8dd5fc94747d4c1e44f3fd4bd4b3b, sw ee9eb812cc59a25bcffd505f1a4c3b8c. Storage: new fields read.done[pid].t, prog.pl and prog.script choice on a placement; boot writes nothing (migrations [rs] [pc]); previous build 7b8c173 carries them. 12-session seed (primer learned) from c9c3a09 byte-equal after boot/reload/Progress open (leaving Progress adds only prog.pv); one Today session on the new build; that record boots on c9c3a09 byte-equal (boot + reload), one key vocab_ru, a session runs there; back on the new build byte-equal; 13/13 checks, 0 console errors, 0 failed requests.
- Live proof 8745de1 (2026-10-09) KEEP: live md5 index 40a52e6df95dbd498cce866e639566d4, sw c4066c666598d102e1782ee795fef924 equal to local; the same 13/13 checks against the live URL, 0 console errors, 0 failed requests.
- Migration proof on engine d2876d8 (adaptive placement with placed-word probation, level exam and critical set, cloze and pattern option fixes, notranslate, session estimates recalibrated): rollback hash 46d1e5ff109aa500a09cc6490857b966bbe7ec95 (engine 8745de1). Previous live md5: index.html 40a52e6df95dbd498cce866e639566d4, sw.js c4066c666598d102e1782ee795fef924. New build: index.html 1c91342eeb593b4dd4575ef3fe5d4e93, sw.js 5092dfea91476a3758ac0bc5810c1130. Proof on the scratch build (live pending the push): a 12-session seed played on 46d1e5f, and a placement plus exam record, both byte-equal on the new and previous builds in both directions, no backup keys, 0 console errors. Verdict KEEP. Goal 1 ETA ships null per the queue verdict.
- Migration proof on engine d6fe859 (round d6fe859, 2026-10-14): exam retakes and critical-set latch, text-stable sentence ids. Storage: prog.exam[level] history keys h k p u (numbered level B1 only); boot writes nothing new. Slim live proof 52 of 52 true, verdict empty: 12-session seed on the previous build 3bc0c4f boots, reloads and shows Progress byte-equal and back; a record with exam-2 history keys (B1 h, k) boots on 3bc0c4f byte-equal and back, keys kept. Pack diff: none (guard ok). ETA external. Previous live md5 index 1c91342eeb593b4dd4575ef3fe5d4e93, sw 5092dfea91476a3758ac0bc5810c1130; rollback hash 3bc0c4f344f2f230eb8289d2cbce226005f57317. New build: index.html 8920fde211235c4d46804cb83380dfc8, sw.js 6a49f819cf42a9260105dc10a7fa5b89. Browser: 21 of 22 PASS; the failed item is tatoeba audio aborts (ERR_ABORTED), identical on 3bc0c4f. The Test-row score after a retake pass is NOT browser-checked (B1 is the first numbered level).
- Migration proof on engine 7d47e32 (round E, 2026-10-14): gloss and tier QA pass (280 gloss fixes, 101 tier moves; ft tiers A1 [98,496,6] A2 [0,538,162] B1 [0,430,270]). Storage: nothing new; boot writes nothing new. Slim live proof 52 of 52 true, verdict empty: 12-session seed on the previous build b0c204b boots, reloads and shows Progress byte-equal and back; a placed record and an exam record boot on b0c204b byte-equal and keep their fields. Pack diff vs HEAD: none (rebuild on 7d47e32 byte-identical, no rebuild commit); passages.json kept as committed. ETA external. Previous live md5 index 8920fde211235c4d46804cb83380dfc8, sw 6a49f819cf42a9260105dc10a7fa5b89; rollback hash b0c204b63c3e22b3039c8fdb8119efc76f8ba4e8. New build: index.html 04a47529ab847e0e074ae62e50d994bb, sw.js 8bfb847348ec4de20b46dbc93f355d10. Browser 7 of 8 PASS (scratch, 360x780 touch): changed gloss стоить "to cost (to be worth ...)" renders in Words and in a drill reveal; level exam ready for A1 and a 20-item exam; notranslate; 0 console errors. The one miss is tatoeba audio download aborts (ERR_ABORTED, ORB), identical on b0c204b, not from this round. Controls: pack_drift goldens (own russian, siblings), pure_arabic and boot_{arabic,hindi,swahili,urdu}_seed3 fail, accepted; pack_flags 39/0, port_sites russian 27/0. Screenshots .cache/live/round-7d47e32.
