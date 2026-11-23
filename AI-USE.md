# AI-USE

This statement follows the five points required by the course: which tools and models were used, what they were used for, representative prompts and excerpts, the important changes I made, and how I checked the output.

## 1. Tools and models used

| # | Tool | Model / version | What it is |
| --- | --- | --- | --- |
| 1 | OpenCode (coding agent) | DeepSeek V4.1 Flash (`deepseek-v4.1-flash`, provider `opencode-go`) | An LLM agent that can run shell commands and edit files in this repository |
| 2 | HistFig, persona "Su Shi" | Model not disclosed by the platform | A web-based RAG chatbot whose retrieval source is this corpus; the **object under evaluation**, not my assistant |
| 3 | OpenCC | `t2s` configuration (rule-based, not generative) | Traditional-to-Simplified character conversion |
| 4 | Ordinary software (no AI) | Python 3.14, `markdown`, `python-docx`, `pypdf`, PyMuPDF, Microsoft Edge, Microsoft Word | Statistics, typesetting, PDF/Word rendering and page-count checks |

## 2. Tasks performed with each tool

**OpenCode / DeepSeek V4.1 Flash**

- Harvested 115 volumes of the *Dongpo quanji* plus six other works from Chinese Wikisource, cleaned wiki markup, and converted the corpus to UTF-8 Simplified Chinese.
- Wrote the analysis scripts: section and genre counts, punctuation density, `□` inventory, duplicate-line and overlap detection, and the 25-character window comparison between files.

**HistFig "Su Shi"**

- Answered seventeen turns of questions (six topics). Those answers and their `sources` blocks are the evidence analysed in report.

**OpenCC**

- Converted the cleaned Traditional text to Simplified, iterating to a fixed point so that a second run reports zero changes.

## 3. Representative prompts and session excerpts

Prompts to the evaluated persona (verbatim):

> 东坡先生，请问您对王安石和司马光之争的看法？

> 请问您的《题西林壁》和王安石的《登飞来峰》有何关联，是否有受后者启发？

> What happened around you in 1099

Excerpt from its answer to the last question — the failure case quoted in the report:

> The dossiers you've laid before me tonight are Hangzhou business and court edicts and one sickbed letter — not a line about my coconuts — so this year must come from memory.

Prompts to the coding agent (verbatim, abridged):

> 收集关于苏轼的数字化文本格式（UTF-8）……总共至少100万字符……并附上每个文件来源的简短列表。

> 将内容改成简体字

A representative agent reply after rebuilding the corpus:

> 现在语料库为 **7 个文件、1,150,922 字符（汉字 1,097,559 个）、28,590 行、3.2 MB**，仍远超 100 万字要求。

## 4. Important changes I made

1. **Cut a file from the corpus.** The agent had included *Dongpo shihua*, a vernacular novel attributed to Chen Xiuming; I judged it neither Su Shi's work nor a historical source, and had it removed, together with its README row, manifest entry and collector job.
2. **Replaced a stub with a real source.** The *Dongpo xiansheng nianpu* page was only a 500-character fragment, so I swapped it for Su Zhe's *Wangxiong Zizhan duanming muzhiming*, a substantial contemporary biography.
3. **Kept the variant characters.** I decided not to normalise 㸔/逺/髙/栁, because they are evidence of the Siku text; the consequence (exact search for 远近高低 fails) is documented and logged as E13.
4. **Renumbered and revised the error log.** After adding the Zhou Tong and citation-mismatch entries, all later IDs shifted; I had every cross-reference in the five exchange files updated so that E01–E15 stay consistent.

## 5. How I checked the output

- **Every number was recomputed by script** over the corpus (`tools/make_manifest.py`, `tools/verify.py`); the reported totals were compared with `corpus_stats.json`.
- **Every quotation was matched against the corpus text.** Each quoted phrase was searched in the source files; where a phrase was absent (e.g. 柳七郎, 飞来峰, 潘衡) it was labelled "not in corpus" rather than accepted. This is how E06 and E07 were found.
- **Encoding and cleanliness were re-tested**: all files decode as UTF-8 without BOM, `verify.py` reports no HTML/wiki residue, and `convert_text.py --check` reports zero changes.

## 6. Declaration

The conversation with HistFig was not rewritten or patched; the failure is preserved exactly as it occurred, including its English sentence and the mistransliteration "Suanglang Hut" (see `conversation/Su Shi - conversation.md`, rounds 13–17). All judgements in report are mine; the AI wrote prose and code at my direction, and every factual claim was checked as described above.

Date: 2026-11-24
