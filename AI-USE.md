# AI-USE

This statement follows the five points required by the course: which tools and models were used, what they were used for, representative prompts and excerpts, the important changes I made, and how I checked the output.

## 1. Tools and models used

| # | Tool | Model / version | What it is |
| --- | --- | --- | --- |
| 1 | OpenCode (coding agent) | DeepSeek V4.1 Flash (`deepseek-v4.1-flash`, provider `opencode-go`) | An LLM agent that can run shell commands and edit files in this repository |
| 2 | HistFig, persona "Su Shi" | Model not disclosed by the platform | A web-based RAG chatbot whose retrieval source is this corpus; the **object under evaluation**, not my assistant |
| 3 | OpenCC | `t2s` configuration (rule-based, not generative) | Traditional-to-Simplified character conversion |
| 4 | Ordinary software (no AI) | Python 3.14, `markdown`, `pypdf`, Microsoft Edge | Statistics, PDF typesetting and page-count checks |

Tool 2 is listed because the assignment evaluates it. Its answers are data, not help: I did not ask it to improve my report, and nothing it wrote is presented as my own analysis.

## 2. Tasks performed with each tool

**OpenCode / DeepSeek V4.1 Flash**

- Harvested 115 volumes of the *Dongpo quanji* plus six other works from Chinese Wikisource, cleaned wiki markup, and converted the corpus to UTF-8 Simplified Chinese.
- Wrote the analysis scripts: section/genre counts, punctuation density, `□` inventory, duplicate-line and overlap detection, and the 25-character window comparison between files.
- Generated `corpus_stats.json`, `SOURCES.md`, the error-log table, and the Markdown-to-PDF pipeline (`tools/build_pdf.py`).
- Drafted and revised the prose of `report.md`, and translated `AI-USE.md` and `error-log.csv` into English on my instruction.

**HistFig "Su Shi"**

- Answered seventeen turns of questions (six topics). Those answers and their `sources` blocks are the evidence analysed in `report.md` and `exchanges/`.

**OpenCC**

- Converted the cleaned Traditional text to Simplified, iterating to a fixed point so that a second run reports zero changes.

## 3. Representative prompts and session excerpts

Prompts to the evaluated persona (verbatim):

> 东坡先生，请问您对王安石和司马光之争的看法？

> 请问您的《题西林壁》和王安石的《登飞来峰》有何关联，是否有受后者启发？

> 请问1099年您身边发生了什么事

> What happened around you in 1099

Excerpt from its answer to the last question — the failure case quoted in the report:

> The dossiers you've laid before me tonight are Hangzhou business and court edicts and one sickbed letter — not a line about my coconuts — so this year must come from memory.

Prompts to the coding agent (verbatim, abridged):

> 收集关于苏轼的数字化文本格式（UTF-8）……总共至少100万字符……并附上每个文件来源的简短列表。

> 将内容改成简体字

> 删除"东坡诗话"的相关内容

> 请根据 text 文件夹和 Su Shi - conversation.md，完成以下任务：Essay report: 1,500–2,000 characters in chinese……

A representative agent reply after rebuilding the corpus:

> 现在语料库为 **7 个文件、1,150,922 字符（汉字 1,097,559 个）、28,590 行、3.2 MB**，仍远超 100 万字要求。

## 4. Important changes I made

1. **Cut a file from the corpus.** The agent had included *Dongpo shihua*, a vernacular novel attributed to Chen Xiuming; I judged it neither Su Shi's work nor a historical source, and had it removed, together with its README row, manifest entry and collector job.
2. **Replaced a stub with a real source.** The *Dongpo xiansheng nianpu* page was only a 500-character fragment, so I swapped it for Su Zhe's *Wangxiong Zizhan duanming muzhiming*, which is a substantial contemporary biography.
3. **Kept the variant characters.** I decided not to normalise 㸔/逺/髙/栁 to modern forms, because they are evidence of the 四庫 text; the consequence (exact search for 远近高低 fails) is documented and logged as E13.
4. **Corrected the essay length and argument.** The first draft was about 1,100 characters; I asked for expansion to 1,664 and for the addition of the variant-character evidence, which I had verified myself before allowing the claim into the text.
5. **Renumbered the error log.** After I added the Zhou Tong and citation-mismatch entries, every later ID shifted; I had all cross-references in the five exchange files updated so that E01–E14 remain consistent.
6. **Fixed the citation, not just the claim.** When the "successful" Wang Anshi answer turned out to cite the wrong chunk for the "linfeng/chuying" passage, I kept the verdict "success" but recorded the mismatch as E04, rather than silently deleting it.

## 5. How I checked the output

- **Every number was recomputed by script** over the corpus (`tools/make_manifest.py`, `tools/verify.py`); the reported totals were then compared with `corpus_stats.json`.
- **Every quotation was matched against the corpus text.** I searched the source files for each quoted phrase; where a phrase was absent (e.g. 柳七郎, 飞来峰, 潘衡) it was labelled "not in corpus" rather than accepted. This is how E03, E06 and E07 were found.
- **Encoding and cleanliness were re-tested**: all files decode as UTF-8 without BOM, `verify.py` reports no HTML/wiki residue, and `convert_text.py --check` reports zero changes.
- **The PDFs were inspected visually** (page screenshots of the cover, an exchange, the error-log table and AI-USE) and their page counts checked with `pypdf` (4 pages for the essay edition, 21 for the full one).
- **The English translation was checked phrase by phrase** against the Chinese original; IDs, part numbers and file names were kept identical.

## 6. Declaration

The conversation with HistFig was not rewritten or patched; the failure is preserved exactly as it occurred, including its English sentence and the mistransliteration "Suanglang Hut" (see `conversation/Su Shi - conversation.md`, rounds 13–17). All judgements in `report.md` are mine; the AI wrote prose and code at my direction, and every factual claim was checked as described above.

Date: 2026-11-24
