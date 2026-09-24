# Vimarsh — UPSC/GPSC answer and essay feedback

Vimarsh reads a handwritten UPSC/GPSC mains answer or essay in Gujarati or English, then gives a score and practical suggestions. You can use it in a normal **ChatGPT or Claude chat on a browser or phone**. No coding, terminal, API key, or special installation is needed for that route.

The score is practice feedback, not an official UPSC/GPSC mark. Clear photos or scans help the assistant read your writing accurately.

## Choose how to use it

| Option | Best for | Setup |
| --- | --- | --- |
| [One chat](#start-with-one-chat-easiest) | Trying Vimarsh now, including on a phone | Attach one instruction file and your answer |
| [Reusable ChatGPT Project](#save-it-in-a-chatgpt-project) | Regular practice in ChatGPT | Set it up once, then start new chats inside the Project |
| [Reusable Claude Project](#save-it-in-a-claude-project) | Regular practice in Claude | Set it up once, then start new chats inside the Project |
| [Claude Code or Codex](#developer-setup) | People who use coding assistants | Use the native skill files in this repository |

## Start with one chat (easiest)

These steps work in the ChatGPT or Claude website and mobile app.

1. Save [vimarsh-chat-instructions.md](chat/vimarsh-chat-instructions.md) to your phone or computer. If you are viewing it on GitHub, open the file and use its **Download raw file** option. You can also copy all its text if saving a file is difficult.
2. Open a **new chat** in [ChatGPT](https://chatgpt.com/) or [Claude](https://claude.ai/).
3. Tap or click the **+ / paperclip** beside the message box. Attach `vimarsh-chat-instructions.md` and clear photos or a PDF of **all pages** of your answer or essay. If you copied the instructions instead, paste them into the chat, then attach your answer.
4. Send this message (change the choices if you wish):

   > Follow the attached Vimarsh instructions. This is a mains answer. Research the topic on the web. Give feedback in Gujarati with English technical terms. Please ask if you cannot read any page.

   For an essay, change “mains answer” to “essay.” If you do not want web research, say “Evaluate only what I wrote.” You can ask for Gujarati-only or English feedback.

5. Check that the assistant read **every page** and identified the question or essay topic correctly. If it missed a page or a word, send a clearer photo or type the missing text before accepting the score.

You can ask follow-up questions in the **same chat**, such as “Why did I lose marks for structure?” For a fresh chat, attach the instruction file again. To carry your progress forward, copy the **Memory update** lines from the previous evaluation into the new chat; chat history alone is not a guaranteed record of those strengths and weaknesses.

If the app does not offer web search or file upload on your account, ask for feedback without research or paste a typed version of the answer. The assistant should say when it cannot read a page or verify a fact.

## Save it in a ChatGPT Project

A Project keeps the instructions and rubric available for future chats. [ChatGPT’s Project guide](https://help.openai.com/en/articles/10169521-projects-in-chatgpt) explains the current menus and account availability.

1. Save both [vimarsh-chat-instructions.md](chat/vimarsh-chat-instructions.md) and [reusable-assistant-instructions.txt](chat/reusable-assistant-instructions.txt).
2. In ChatGPT, open the sidebar and choose **New project**. Name it “Vimarsh.”
3. In the Project, add `vimarsh-chat-instructions.md` as a **project file/source**. Open the Project's **••• → Project settings** and paste the text from `reusable-assistant-instructions.txt` into **Project instructions**.
4. Start each evaluation as a **new chat inside that Project**. Attach your answer pages and say whether it is an answer or essay, whether you want web research, and which feedback language you prefer.

On a phone, use the ChatGPT app or `chatgpt.com` in your browser. Menu labels may vary slightly by device. If Project setup is unavailable in your app, set it up in a browser, then use the Project in the app. Copy the **Memory update** into the next chat when you want the evaluator to consider your earlier pattern; Project memory and plan settings can affect what past chats it sees.

**Optional custom GPT:** If your ChatGPT Business, Enterprise, or Edu workspace allows new GPTs, create one on the **web**, paste `reusable-assistant-instructions.txt` into its **Instructions**, and upload `vimarsh-chat-instructions.md` as **Knowledge**. Enable web search if offered, then test it with a sample answer. You can use an existing GPT on mobile, but [new GPT creation is currently unavailable on personal accounts and in mobile apps](https://help.openai.com/en/articles/8554397-creating-and-editing-gpts).

## Save it in a Claude Project

[Claude Projects](https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects) let you reuse instructions and files across chats. Current Claude guidance says free accounts can create a limited number of Projects.

1. Save both [vimarsh-chat-instructions.md](chat/vimarsh-chat-instructions.md) and [reusable-assistant-instructions.txt](chat/reusable-assistant-instructions.txt).
2. Open **Projects** in Claude and choose **New Project**. Name it “Vimarsh.”
3. Add `vimarsh-chat-instructions.md` to **Project knowledge**. Choose **Set project instructions** and paste the text from `reusable-assistant-instructions.txt`; save it.
4. Start each evaluation as a **new chat inside that Project** and attach your answer pages. Tell Claude whether it is an answer or essay, whether to research, and the feedback language.

You can open Claude in its mobile app or phone browser for evaluations. If you cannot find the Project setup controls on your phone, do the one-time setup at `claude.ai/projects` in a browser. Claude says separate Project chats do not share all their context automatically, so paste your previous **Memory update** into the next chat if you want it used.

## Developer setup

This repository also contains native skill files for [Claude Code](https://claude.com/claude-code) in `.claude/skills/vimarsh/` and for Codex in `.agents/skills/vimarsh/`, with their respective agent definitions. Those versions can use local files, separate transcription/research/evaluation agents, `runs/` artifacts, and local `memory/` profiles. The chat version above handles the same evaluation within one conversation and gives you a copyable Memory update.

For Claude Code, open a terminal in this repository, run `claude`, then ask “Use the vimarsh skill to evaluate `answer.pdf`.” For Codex, open this repository as the workspace and ask the same. Place the PDF or images inside the workspace or attach them in the app.

The rubrics are in `.claude/skills/vimarsh/rubrics/` and `.agents/skills/vimarsh/rubrics/`. The browser/mobile instruction file contains both rubrics, so readers using ordinary ChatGPT or Claude do not need to open these folders.
