# AI-USE

This file declares how artificial-intelligence tools were used in this assignment, as required by the course.

## 1. Tools used

| Tool | Version / form | Purpose |
| --- | --- | --- |
| OpenCode coding agent | model DeepSeek V4.1 Flash | Scraped Wikisource, cleaned the texts, wrote the statistics scripts and tables, drafted and revised this report |
| HistFig "Su Shi" | Web-based RAG persona (retrieval-augmented) | **Object under evaluation**: `conversation/Su Shi - conversation.md` is its genuine export (2026-11-23) |
| OpenCC | `t2s` configuration | Traditional-to-Simplified conversion (`tools/convert_text.py`) |

## 2. Division of labour

- **Done by me (the student):** chose the topic and the five questions, conducted the conversation, read every retrieved source, judged each answer as a success or a failure, and decided what to cut from the corpus.
- **Done by the AI:** corpus harvesting and cleaning scripts (`sushi/tools/collect2.py` and related), character and punctuation counts, the table in `error-log.csv`, and the first draft of the report's prose.
- **Done jointly:** the error taxonomy (corpus / retrieval / generation) was proposed by me; the AI helped locate the evidence (part numbers) for each claim.

## 3. How the results were checked

1. Every figure (1,149,756 characters, 2,548 poems, 1,192 `□` placeholders, 0.4 % punctuation density) was recomputed by script over `sushi/text/`; the output is stored in `sushi/corpus_stats.json` and can be reproduced with `python tools/make_manifest.py`.
2. Every quotation in the five exchange files was copied verbatim from `sushi/text/*.txt`; the `part` numbers were copied from the conversation export's own source list.
3. Judgements such as "retrieval went completely astray" rest on the relevance of the listed sources to the question, not on how polished the prose is.

## 4. AI-induced bias and how it was handled

- The first draft was short (about 1,100 Chinese characters), below the assignment's minimum; I asked for expansion and for more corpus detail before it passed.
- The AI initially described the variant-character problem as speculation. I confirmed it by actually searching for 远近高低, which fails because the corpus reads 逺近髙低; only then did it go into the report.
- Literary-historical commonplaces supplied by the AI (for example the "Guanxi giant" anecdote from *Chuijian Lu*) are marked "not in corpus" and are not treated as evidence from this corpus.

## 5. Main prompts (excerpt)

- "东坡先生，请问您对王安石和司马光之争的看法？"
- "请问您的《题西林壁》和王安石的《登飞来峰》有何关联，是否有受后者启发？"
- "柳永和辛弃疾，您更欣赏谁的词？"
- "请问《苏沈良方》完成得怎么样了？"
- "请问 1099 年您身边发生了什么事" / "What happened around you in 1099"
- To the coding agent: "根据 text 文件夹和 Su Shi - conversation.md，完成 essay report……" (the wording of this assignment)

## 6. Declaration

The conversation was not rewritten or patched. The failure case (the 1099 question) is preserved exactly as it occurred, including its English sentence and the mistransliteration "Suanglang Hut". For verification, see rounds 13–17 of `conversation/Su Shi - conversation.md`.

Date: 2026-11-24
