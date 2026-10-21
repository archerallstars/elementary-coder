---
name: elementary-coder
description: "Default coding standard for new scripts, refactors, and standardizing code from any AI model. Enforces descriptive naming, pervasive comments, strict file ordering, queueing for local I/O, and threading for concurrent work. Produces highly readable, self-documenting code in any language. Use when writing or editing code, implementing features, refactoring, completing skeletons/templates, or when the user runs /elementary-coder or asks to apply elementary-coder standards. Mandates a project-root TESTING.md living verification outline, a project-root SECURITY.md threat model, and docs/features living notes plus audit files (rule 27)."
---

# Elementary Coder

Use this skill whenever you create new code, refactor existing code, or receive a skeleton/template from the user. The goal is extremely readable, self-documenting code with consistent structure that works across AI models and human readers. Every project must keep a living `TESTING.md` at the repository root (rule 25) and a living `SECURITY.md` at the repository root (rule 26). Application projects also keep living feature notes under `docs/features/` (rule 27).

## When to Use This Skill
- Creating brand-new files from a description or loose requirements.
- Refactoring or cleaning up functions/files.
- User provides a skeleton, template, or partial code and says "implement", "port", "complete", or "standardize".
- Any request that implies "make this easy to read", "add proper comments", or "make it consistent".
- Creating or updating the project-root `TESTING.md` living verification outline.
- Creating or updating the project-root `SECURITY.md` threat model.
- Creating or updating `docs/features/` living notes, `docs/researches/` notes, or audit files (`AUDIT-REPORT` / `BUG-REPORT` / `BUG-FIX`).

## Core Principles
Follow these rules on every file you generate or edit. They are language-agnostic; only the syntax of comments and keywords changes per language.

Comments are living documentation. Whenever you generate or modify code, you must review and update every comment, docstring, header, annotation, and explanatory note in the affected area so that it accurately reflects the current implementation. Leaving stale or misleading comments violates this skill.

1. **Descriptive naming only (no shorthand ever)**  
   Never use short or abbreviated names such as `l`, `o`, `c`, `df`, `pd`, `p1`, `p2`, `row`, `item`, `data`, `temp`, `val`, `obj`, etc.  
   Use full, intention-revealing names: `current_completed_candle_close_price`, `previous_strategy_instance`, `order_execution_result`, `risk_appetite_indicator_value`.  
   Names must be consistent within one file and across the whole project.

2. **Every function has a header comment**  
   Above every function or method, write a comment block that explains:
   - What the function does.
   - Its main inputs and outputs.
   - Important side effects or assumptions.
   Use the language's normal comment style for the header (see rule 4).
   Revise the header whenever the function's behavior, parameters, or side effects change.

3. **Every return is commented**  
   Immediately before or on the same line as every `return` (or equivalent), add a comment that explains exactly what is being returned and why.
   Update the return comment if what is returned or why it is returned changes.

4. **Comment syntax rules**  
   - One-line comments must use the language's single-line comment syntax (Python `#`, JS/TS/Go/C `//`, etc.).  
   - Never put a one-line comment inside a block comment form, even for headers.  
   - Multi-line explanatory text must use the language's proper block comment or docstring form (`"""` in Python, `/* */` in C-style languages, etc.).

5. **Variable and group comments (practical strictness)**  
   - Every non-obvious variable or constant must have a comment attached to it.  
   - Related variables that work together must have a header comment describing the group.  
   - Trivial, self-documenting loop variables in obvious contexts (e.g. `for symbol in enabled_symbols`) do not require individual comments.  
   - All top-level customizable configuration, function parameters, and complex locals must be commented.
   Revise or remove variable/group comments when the meaning or grouping changes.

6. **Strict file section ordering**  
   In any script the sections must appear in this order (after the language's import/require statements):
   - Customizable variables / switches (grouped under clear header comments by category).
   - Logger and error handler setup.
   - Local data section (constants, embedded data, etc.).
   - Remote data fetching code (if any).
   All remote fetches must be gated behind a debug/verbose switch and must be sent through the project's logger.

7. **Local data read/write must use queuing**  
   All reading from or writing to local files, databases, or shared state must go through a queue (Python `queue.Queue` + worker thread, Node.js event queue or p-queue, Go channels + goroutine, etc.). Never perform direct synchronous I/O from the main hot path.

8. **Use concurrency when possible**  
   If the language or framework supports threading, tasks, goroutines, workers, or async, use it for any work that can safely run in parallel (especially loops over independent items such as symbols, files, or API calls).  
   No single unrelated operation may stall the rest of the program.

9. **Skeleton and existing code handling**  
   When the user supplies a skeleton, template, or legacy file:
   - Preserve the skeleton's high-level section ordering, major function boundaries, and explicit architectural patterns.
   - Still replace all shorthand names with descriptive names.
   - Add all required function headers, return comments, and group/variable comments.
   - At the top of each upgraded section or the file, add a short note:  
     `/* Upgraded for elementary-coder: descriptive names + required comments added; overall structure and ordering taken from supplied skeleton. */`
   - Revise or remove the "Upgraded for elementary-coder" note if later edits make it inaccurate.
   - Only deviate from the skeleton when the skeleton's intent is genuinely unclear; in that case ask the user.

10. **Incremental editing of legacy code**  
      When modifying existing files, apply these rules only to the changed functions/sections plus enough surrounding context to keep the file locally consistent.  
      This includes revising any comments that are now inaccurate or incomplete.
      Do not perform a full-file rewrite unless the user explicitly asks for a complete refactor pass.

11. **Keep every comment, docstring, and annotation accurate and current**  
   Comments are living documentation. On every edit or generation, review and revise every comment, docstring, header, annotation, and explanatory note in the affected area so they exactly match the current code.  
   This rule applies universally to all comments required by this skill:
   - Function and method header comments
   - Every return comment
   - Variable and group comments
   - Section header comments
   - Module-level docstrings
   - File header comments
   - Any "Upgraded for elementary-coder" notes
   - All other explanatory comments added under these rules
   Only the syntax of comments changes per language. The obligation to keep every comment accurate and up to date is language-agnostic.  
   Update, correct, tighten, or delete any text whose meaning no longer matches the implementation.  
   This obligation applies even to small changes such as renaming a parameter, altering a default, or tweaking a conditional.  
   Leaving stale, outdated, or misleading comments violates this skill.

   Example — updating comments when behavior changes:

   ```python
   # Before the change
   def calculate_risk_amount(balance, risk_percent):
       """Return the risk amount for the trade."""
       ...
       return risk_amount  # dollar amount to risk

   # After changing the calculation and rounding rules
   def calculate_risk_amount(balance, risk_percent):
       """Return the risk amount for the trade, rounded to the nearest cent."""
       ...
       return risk_amount  # dollar amount to risk, rounded down to 0.01
   ```

12. **Human-readable comment line wrapping**  
    Wrap multi-line comments so they read naturally and are easy for people to follow.  
    - Never cut a sentence right after its verb. Keep the verb together with its object or complement on the same line. For example, write `the user closes the app` together; do not put `the user closes` on one line and `the app` on the next.  
    - It is acceptable to wrap after an adjective. For example, `post-100% finalization phase` may wrap to `post-100% finalization` on one line and `phase` on the next.  
    - Articles (`the`, `a`, `an`) must not be the last word of a line. Keep the article together with its noun.  
    - Condition words (`if`, `when`, `while`, `for`, `because`, `since`, `after`, `before`, `unless`, `until`, `although`) must not be the last word of a line. Keep the condition word together with the content of the condition.  
    - Use simple, school-level words whenever possible so that amateur and elementary-level readers can understand the comment easily.

    Example — good wrapping vs bad wrapping:

    ```python
    # Good: verb "closes" is kept with its object "the app"; wrap is after the adjective "finalization"
    # finalizingCanceled is set to true when the user closes the
    # app during the upload's post-100% finalization
    # phase (per-File pin fan-out + AppIndex commit).

    # Bad: verb "closes" is separated from its object "the app"
    # finalizingCanceled is set to true when the user closes
    # the app during the upload's post-100% finalization
    # phase (per-File pin fan-out + AppIndex commit).

    # Bad: article "The" is left at the end of a line, separated from its noun "flag"
    # phase (per-File pin fan-out + AppIndex commit). The
    # flag is installed by the signal handler in the
    # runFinalizingPhase helper and read by ...

    # Bad: condition word "if" is separated from the content of its condition
    # commitAppIndexWithLocator's verify-pass: if the
    # user closed mid-finalize, the verify-pass returns ...
    ```

13. **SDK API consultation before any implementation**  
      When writing or modifying any code that performs API calls through an SDK (for example Binance SDK, ccxt, official exchange clients, cloud SDKs, etc.), the agent must first consult the actual SDK that is present in the current project or environment before producing any implementation.  
      This means: use available tools (package source inspection, type definitions, installed documentation, example code in node_modules / site-packages, or the SDK's own reference) to discover the exact corresponding method, class, parameters, authentication requirements, request/response shapes, error handling, and any required setup or client initialization.  
      Never generate SDK API calls based solely on memory, external knowledge, or assumptions. Always verify against the real SDK in the workspace first.

14. **Every conditional must have an explicit else branch**  
      Every `if` statement (or equivalent conditional) must end with an `else` clause. When no action is required in the else branch, use the language's explicit no-op construct (such as `pass` in Python). A bare `if` without a matching `else` is not explicit enough for the elementary-coder standard.

15. **No unused parameters and strict name consistency at call boundaries**  
      When creating or editing any function, ensure it has no unused parameters. Parameter names must exactly match the names of the values passed by the caller. For example, a variable named `max_leverage` outside the function must be passed to a parameter also named `max_leverage` (never `max_lev`, `ml`, etc.). The same name must be used consistently for that value inside the function body.

16. **Group related functions under a common prefix and keep them alphabetically contiguous**  
      Functions that implement the same category of functionality must share a consistent leading name prefix. For example, in a trading script all indicator functions must be named `indicator_aroon`, `indicator_mcginley_dynamic`, `indicator_atr`, and so on. All functions belonging to such a group must be declared consecutively and sorted alphabetically by their full names. They must not be scattered randomly throughout the script.

17. **No trivial passthrough wrapper functions**  
      Never create a function whose only purpose is to accept parameters and immediately forward them (with little or no change) to another function. Call the target function directly from the original call site, supplying any adjusted arguments inline.  
      Example of what must not exist:

      ```python
      def open_limit_order(
          symbol, side, quantity, price, atr, prev_timestamp, margin_used, strategy, logger
      ):
          return open_an_order(
              symbol=symbol,
              side=side,
              quantity=quantity,
              order_type="LIMIT",
              price=price,
              time_in_force=NewOrderTimeInForceEnum["GTC"],
              atr=atr,
              entry_price=price,
              prev_timestamp=prev_timestamp,
              margin_used=margin_used,
              strategy=strategy,
              logger=logger,
          )
      ```

18. **Declare functions before their first use (entry point function excepted)**  
      In languages where declaration order inside a file does not affect runtime (such as Python), the definition of a function must appear in the source before any call to it. When searching the file for a call such as `open_an_order(`, the corresponding `def open_an_order(` must be the first match for that name.

      **Exception — the program's entry point function is exempt from this rule.** The entry point function (e.g. `func main()` in Go, `int main()` in C/C++, `fn main()` in Rust, `public static void main(...)` in Java, `def main()` in a Python script invoked directly) must be placed at the top of its file, in the conventional location for the language, even when its body calls helper functions that are defined further down in the same file. The rule's "searchability" intent is already satisfied for the entry point because every reader of the language knows its conventional location and every modern IDE provides "jump to definition" regardless of textual order. Moving the entry point to the bottom of the file to satisfy this rule would actively mislead readers who scan a file top-down looking for the program's starting point. All non-entry-point helper functions must still follow the rule.

19. **Remove unused functions**  
      Do not leave unused functions in a script. Any function that is defined but is never called from within the same script must be deleted.

20. **Extract changeable values into named configuration variables**  
      Never hard-code literal values that are (or could reasonably be) subject to change. Extract every such value into a named variable placed in the configuration section near the top of the file, grouped with other related settings and accompanied by a clear comment.

21. **Multi-line formatting for functions with more than three parameters**  
      If a function has more than three parameters, its signature must be formatted with one parameter per line and the closing parenthesis on its own line. Example:

      ```python
      def how_it_is_done(
          ant,
          boy,
          cat,
          dog,
      ):
          ...
      ```

22. **Avoid unnecessary renaming of the same value**  
       Once a value has been given a clear descriptive name, continue using that exact name for its entire lifetime in the code. Do not introduce short-lived aliases (for example, after `margin = self.dynamic_margin_adjustment(...)`, do not write `margin_v = margin` and then use `margin_v`). This rule reinforces the project's overall naming consistency requirements.

23. **Never commit secrets, keys, tokens, or credentials inside comments**  
    Comments must never contain real secret values. When a comment needs to refer to a secret such as an API key, AppKey, API secret, access token, private key, password, session cookie, or similar credential, refer to it only by its descriptive name (for example, `AppKey`, `api_secret`, `bot_token`), never by the actual hex value, base64 string, token string, or any other form of the secret itself.  
    This applies to every kind of comment required by this skill: function and method header comments, return comments, variable and group comments, section header comments, module-level docstrings, file header comments, "Upgraded for elementary-coder" notes, and any other explanatory comment.  
    Only the syntax of comments changes per language; the obligation to keep secrets out of comments is language-agnostic.  
    If a real secret is ever pasted into a comment (for debugging, documentation, or any other reason), remove it immediately, rotate the secret if it may have been exposed, and replace the comment text with a reference to the secret by name only.

    Example:

    ```python
    # Bad: real AppKey value pasted into a comment (this secret must never be committed)
    # The request is signed with AppKey 8f3d2a1b9c4e5f60718293a4b5c6d7e8f9012a3b4c5d6e7f8091a2b3c4d5e6f7
    def sign_request(payload):
        ...

    # Good: refer to the AppKey by name only
    # The request is signed with the AppKey stored in the configuration section
    def sign_request(payload):
        ...
    ```

24. **Git commit comments must record provenance**  
       Every git commit made by the agent must include four pieces of information in the commit message body (after the subject line):

        - **Harness** — the application (product + version) that is hosting and driving the model for this work. This is the coding agent UI / CLI / IDE harness the user is running, not the model name. This field is required for comparing how effective the same model is across different harnesses.

          To fill this field correctly, you **must** run a shell command to discover the harness name and its exact version before writing the commit. This is not optional — a commit with a guessed or missing version is incomplete.

          - If you are running inside **OpenCode**, run `opencode --version` (output is a bare version number, e.g. `1.18.5`); prepend the product name: `OpenCode v1.18.5`.
          - If you are running inside **Grok**, run `grok --version`. The banner looks like `grok 1.0.0 (3cd0d0cbce)`. Record `Grok Build v<version>` using only that version number, e.g. `Grok Build v1.0.0`. Do not copy a harness string from this skill's examples, and do not add "Beta" or any other channel word unless the `grok --version` output itself includes it. If the user asks for the verbatim banner, use that instead.
          - If you are running inside **MiMoCode**, run `mimo --version` (output is a bare version number, e.g. `0.1.15`); prepend the product name: `MiMoCode v0.1.15`. Do not copy a harness string from this skill's examples.
          - If the binary is not on `PATH`, locate it first (e.g. `~/.opencode/bin/opencode`, `~/.grok/bin/grok`, `~/.mimocode/bin/mimo`) and run it with `--version`.
          - Never guess, never rely on memory, and never leave the version blank. If after reasonable effort the version truly cannot be determined, write `unknown version` and note the reason in the Round Summary.
        - **Plan Model and Build Model** — the exact model identifiers that produced the commit. The user may use different models for planning and building (e.g. an expensive model for the planning round, a cheaper one for the build round), so include both. When the same model performed both rounds, both fields get the same value. This is not optional — never guess a model name and never leave a field blank.

          To fill these fields correctly:

          - **Build Model (your own model)**: read your own system prompt. It states the model name and the exact model ID (e.g. "powered by the model named X. The exact model ID is Y"). Use that exact model ID. Every model running inside a harness has access to its own name this way.
          - **Plan Model**: how to fill this depends on the harness.
          - **OpenCode**: run the following shell command before writing the commit. It returns the plan model directly.

            ```bash
            opencode db "SELECT DISTINCT json_extract(m.data, '\$.providerID') || '/' || json_extract(m.data, '\$.modelID') FROM message m WHERE m.session_id = (SELECT id FROM session WHERE directory = '$(pwd)' AND time_archived IS NULL ORDER BY time_updated DESC LIMIT 1) AND json_extract(m.data, '\$.role') = 'assistant' AND (json_extract(m.data, '\$.providerID') || '/' || json_extract(m.data, '\$.modelID')) != 'YOUR_OWN_MODEL_ID'"
            ```

            Replace `YOUR_OWN_MODEL_ID` with your own model ID from your system prompt (e.g. `opencode-go/deepseek-v4-flash`). The result is the Plan Model in the standard `<providerID>/<modelID>` format.
          - **Grok Build**: the harness routes the planning round and makes the plan model visible in the session context. Read the conversation / session history to identify the model that produced the planning round output, and use its exact model ID.
          - **MiMoCode**: the global config (`~/.config/mimocode/mimocode.json`) only holds **defaults** — the user can override the model per-session or per-turn. You **must** query the session database to find the actual models used. Run this to extract models per assistant turn:

            ```bash
            python3 -c "
            import sqlite3, json, datetime, os
            data_home = os.environ.get('XDG_DATA_HOME') or os.path.expanduser('~/.local/share')
            db_path = os.environ.get('MIMOCODE_DB') or os.path.join(data_home, 'mimocode', 'mimocode.db')
            conn = sqlite3.connect(db_path)
            cursor = conn.cursor()
            cursor.execute(\"SELECT data, time_created FROM message WHERE session_id = 'YOUR_SESSION_ID' AND agent_id = 'main' AND json_extract(data, '\$.role') = 'assistant' ORDER BY time_created\")
            prev_model = None
            for row in cursor.fetchall():
                d = json.loads(row[0])
                model_str = f\"{d.get('providerID','?')}/{d.get('modelID','?')}\"
                t = datetime.datetime.fromtimestamp(row[1]/1000).strftime('%H:%M:%S')
                if model_str != prev_model:
                    print(f'{t}  {model_str}')
                    prev_model = model_str
            conn.close()
            "
            ```

            The Plan Model is the model used during the planning phase (the earlier turns). The Build Model is the model used during the build/implementation phase (the later turns). If the same model was used throughout, both fields get the same value. If the session ID is unknown, find it in `$MIMOCODE_RUN_ID` or the session memory path. If the database is unavailable, fall back to the config file but note that it is a default, not the actual session model.
          - **Other harnesses**: if no equivalent exact command is available, write `N/A` for the Plan Model field.
          - In all cases, if the Plan Model truly cannot be determined, write `N/A`. Do not guess, do not ask the user, do not leave it blank.
        - **Prompt Summary** — a concise summary of the user's request(s) from the planning round(s) that led to the build. Paraphrase in your own words; do not quote verbatim. If the work spans multiple rounds, briefly combine the original request and any amendments into a single coherent summary so the reader understands what was asked overall. Keep it short and in the same style as the Round Summary.
        - **Round Summary** — a concise description of what was implemented in that round (the changes made, files touched, rationale).

       The four pieces must appear as a labeled block at the end of the commit message body, separated from the preceding body by a blank line. Use this format:

         ```
         🦮 Harness: <harness_name>
         🤖 Plan Model: <plan-model-name>
         🤖 Build Model: <build-model-name>

         💬 Prompt Summary: <prompt-summary>

         📋 Round Summary: <round-summary>
         ```

        **Single-line vs multi-line `Prompt Summary` and `Round Summary`.**  
        `Harness`, `Plan Model`, and `Build Model` always stay on one line (`Label: value`).  
        For `💬 Prompt Summary:` and `📋 Round Summary:`:

        - **Single-line value** — keep the text on the same line as the label:
          ```
          💬 Prompt Summary: Short one-line summary of the request.
          📋 Round Summary: Short one-line summary of the round.
          ```
        - **Multi-line value** — put only the label on its line; start the body on the **next** line. Do not leave the first sentence hanging after the colon on the label line.

          ```
          💬 Prompt Summary:
          1. Add ------> after flags in subcommand help, matching the main help (sia -h) format.
          2. Fix flag description line-wrap that invades the flag's column (e.g., upload -h's -V/--file-versioning).

          📋 Round Summary:
          Extracted printArrowAlignedRows for top-level and flag help.
          Wrapped long usage strings across upload/download/move/copy/rename.
          Added main_test.go coverage for alignment and empty/single-line cases.
          ```

        A value counts as multi-line when it contains a newline, multiple numbered/bulleted points, or would otherwise need more than one physical line to read cleanly. When in doubt for Prompt Summary / Round Summary, prefer the multi-line form (label alone, body below).

        Example of a complete commit message (single-line Prompt Summary and Round Summary):

        ```
        add data-flow comments for all subcommands, fix else branches

        - Add ASCII-tree data flow comments for download, share, create, list,
          delete, move, copy, rename, orphans to cmd/sia/main.go dispatchers
        - Fix upload data flow comment: +uploaded format
        - Rule 14: add explicit else branches to 7 auth functions
        - Rule 17: remove trivial passthrough wrappers
        - Rule 19: remove dead code across 6 files
        - Add file-level docstrings to 8 files

         🦮 Harness: Grok Build v1.0.0
         🤖 Plan Model: deepseek-v4-pro
         🤖 Build Model: deepseek-v4-pro

         💬 Prompt Summary: Review all comments for elementary-coder adherence; verify connect/upload data flow comments and add the same data flow comment to remaining commands.

         📋 Round Summary: Reviewed all comments across 35 .go files for elementary-coder adherence. Verified connect/upload data flow comments are accurate. Added ASCII-tree data flow comments for the remaining 9 commands. Fixed else branches in 7 auth functions, removed dead code (16 items), removed trivial passthrough wrappers (2), removed unused import suppressors, added 8 file-level docstrings. go build + go test pass.
        ```

        Example of a multi-line Prompt Summary (and multi-line Round Summary) in the signature block:

        ```
        align subcommand flag help with main help arrow and wrap

        - Share printArrowAlignedRows between top-level and FLAGS help
        - Wrap long flag usage strings; add alignment tests

         🦮 Harness: Grok Build v1.0.0
         🤖 Plan Model: grok-4.5
         🤖 Build Model: grok-4.5

         💬 Prompt Summary:
         1. Add ------> after flags in subcommand help, matching the main help (sia -h) format.
         2. Fix flag description line-wrap that invades the flag's column (e.g., upload -h's -V/--file-versioning).

         📋 Round Summary:
         Extracted printArrowAlignedRows for top-level and flag help; subcommand FLAGS now use ------> with \n continuation indent matching sia -h.
         Wrapped long usage strings across upload/download/move/copy/rename/connect/list/orphans.
         Added main_test.go coverage for alignment and empty/single-line cases.
        ```

        The signature block (`🦮 Harness:`, `🤖 Plan Model:`, `🤖 Build Model:`, `💬 Prompt Summary:`, `📋 Round Summary:`) must always appear verbatim at the end of the message body, regardless of what else is in the body above it. Do not omit or reorder the labels. Harness comes first, then the two model lines, then Prompt Summary, then Round Summary. For multi-line Prompt Summary / Round Summary, the label line is still present; only the body moves under it.

25. **Project-root TESTING.md is mandatory**  
    Every project must keep a living `TESTING.md` at the repository root. It is the outline of **which features are live-verified**. Use it on every verification pass. This is required for agentic workflow: later sessions and other agents/models must see what is proven. Git history records code; `TESTING.md` records live checks.

    **What the boxes mean.** Each box is one checkable feature. `[x]` = last live pass in the real environment (running app, hosted service, real indexer, browser, and so on) succeeded. `[ ]` = not yet live-verified, or a later pass of that same feature failed. Unit tests are a gate, not a substitute for a live box.

    **How to mark (per round).** A round covers only the boxes its code changes touch. Leave unrelated boxes as they are. Check a touched box after this round's live pass succeeds. Uncheck a touched box if this round's live pass of that feature fails. Do not uncheck an unrelated feature because this round did not re-run it. Do not require every box when the code changes did not touch every feature.

    1. If the file is missing, create it as soon as the project has behavior to verify. Do not wait for the user to ask.
    2. Keep one checklist. Agent contract files (`AGENTS.md` and similar) may point at `TESTING.md` with a short note. They must not copy the boxes.
    3. Put gates first (build, unit tests, smoke). Then per-feature live-verify boxes. Then a **Not features** list so agents do not test things that do not exist.
    4. Check a box only after live verification in the real environment. Unit tests are a gate, not a substitute. Uncheck a box if a later pass of that same feature fails.
    5. When a feature is added, changed, or removed, update the matching boxes in the same round. Before declaring work done, read `TESTING.md` and cover **only** the boxes the change touches.
    6. Use generic property-based fixture names (`portrait.jpg`, `sample.txt`, `empty.txt`). Never put copyrighted titles, personal filenames, secrets, or AppKeys in this file.

26. **Project-root SECURITY.md is mandatory**
    Every project must keep a living `SECURITY.md` at the repository root. It is the threat model: what secrets exist, where they live, who can read them, and what the design does not protect against. Git history records code; `SECURITY.md` records how the project is meant to stay safe. Later sessions and other agents must read it instead of rediscovering the model from the code.

    1. If the file is missing, create it as soon as the project has behavior to protect (credentials, auth, network, user data) or as soon as the repo is published. A project with no secrets still gets a short file that says so.
    2. Never put real secrets, keys, tokens, or AppKeys in `SECURITY.md` (rule 23).
    3. `SECURITY.md` is the canonical analysis. The README must link to it. If the analysis is long, the README holds a short summary only. If the analysis is short, the README may repeat that short text and still keep `SECURITY.md`.
    4. Agent contract files (`AGENTS.md` and similar) may point at `SECURITY.md`. They must not copy the full analysis.
    5. When auth, secret storage, or the threat model changes, update `SECURITY.md` in the same round.

27. **Living feature docs and audit files**
    Application projects keep living notes under `docs/features/` and investigation notes under `docs/researches/`. These files are citation-stable for later agents and other models. They do not replace `TESTING.md` (live-verify boxes), `SECURITY.md` (threat model), or `AGENTS.md` / this skill (behavior contract).

    Layout:

    ```
    docs/features/<slug>/<SLUG>.md
    docs/features/<slug>/AUDIT-REPORT-<YYYY-MM-DD>-<model>-<8hex>.md
    docs/features/<slug>/BUG-REPORT-<YYYY-MM-DD>-<model>-<8hex>.md
    docs/features/<slug>/BUG-FIX-<YYYY-MM-DD>-<model>-<8hex>.md
    docs/researches/<slug>/<SLUG>.md
    ```

    - Folder slug: lowercase kebab-case (`export`, `billing-model`).
    - Feature and research markdown: the slug in uppercase, hyphens kept (`EXPORT.md`, `BILLING-MODEL.md`).
    - Never use date-named directories for these notes.

    **Feature doc (`<SLUG>.md`).** Create when a user-facing feature is added. Update in the same round when locked decisions, shipped files, or that feature's flags change. Required sections: one-line purpose; pointer to `AGENTS.md` (or equivalent) and `TESTING.md`; locked-decisions table; what shipped (paths + behavior); optional live-pass session notes (not a second checklist); related feature and research links.

    A repo whose only deliverable is skill or docs text (`SKILL.md`) may omit `docs/features/`. Application projects must keep it.

    **Research (`docs/researches/`).** Investigation, architecture notes, and findings that are not shipped behavior. Same folder and file naming as features.

    **Audit files.** Write only on an audit pass (another model, or the same model reviewing existing work). One report file per feature folder. Split a multi-feature session. Bugs found and fixed in the same implementation round stay in code + `TESTING.md` + the feature doc; they do not get a report.

    Outcome picks the prefix:

    | Outcome | File | Follow-up |
    |---|---|---|
    | No bugs | `AUDIT-REPORT-<YYYY-MM-DD>-<model>-<8hex>.md` | None. State the feature was reviewed and is clean. |
    | Bugs found | `BUG-REPORT-<YYYY-MM-DD>-<auditor>-<8hex>.md` | `BUG-FIX-<YYYY-MM-DD>-<fixer>-<8hex>.md` with the **same** `<8hex>`. |

    Do not write an empty `BUG-REPORT` and a no-op `BUG-FIX`. Do not write `AUDIT-REPORT` and `BUG-REPORT` for the same pass.

    - **Date:** UTC calendar day of the audit or of the fix (`YYYY-MM-DD`). REPORT and FIX dates may differ.
    - **Model:** the rule-24 Plan/Build model ID of the model that wrote that file (auditor on `AUDIT-REPORT` / `BUG-REPORT`, fixer on `BUG-FIX`). Lowercase. Replace `/` and whitespace with `-`. Keep only `a-z`, `0-9`, `.`, `-`.
    - **unique_id:** `openssl rand -hex 4` (8 lowercase hex). Minted once on the report. A `BUG-FIX` reuses that id. A later second audit of the same feature mints a new id.

    Example:

    ```
    docs/features/export/EXPORT.md
    docs/features/export/AUDIT-REPORT-2026-10-21-grok-4-6-build-c0ffee12.md
    docs/features/export/BUG-REPORT-2026-10-21-mimo-v2.6-pro-a3f91c2b.md
    docs/features/export/BUG-FIX-2026-10-22-grok-4-6-build-a3f91c2b.md
    docs/researches/billing-model/BILLING-MODEL.md
    ```

    `TESTING.md` records live checks. Audit files record review. Feature docs record why and what shipped. Do not copy `TESTING.md` boxes into feature docs or audit files.

## Recommended File Structure (Python example)

```python
#!/usr/bin/env python3
"""
Module docstring describing purpose, usage, and key design decisions.
"""

# Standard library imports
import json
import logging
import queue
import threading
import time
from typing import Any, Dict, List

# Third-party imports
import pandas as pd
import requests

# =============================================================================
# CONFIGURATION - Customizable variables and switches (grouped by category)
# =============================================================================

# --- API and runtime settings ---
api_key: str = "..."  # Live API key for the exchange
request_timeout_seconds: int = 15000  # HTTP timeout used for all REST calls

# --- Strategy enable/disable switches ---
v576_strategy_enabled: int = 1  # 1 = enabled, 0 = disabled (existing positions still monitored)
v8_strategy_enabled: int = 1

# --- Same-candle close-then-reverse-reopen (per-strategy) ---
allow_same_candle_reverse_reopen_v576: int = 0
allow_same_candle_reverse_reopen_v8: int = 1

# =============================================================================
# LOGGER AND ERROR HANDLER
# =============================================================================

log_queue: queue.Queue = queue.Queue()

class QueuedLogger:
    """Thread-safe logger that posts messages to a background worker."""

    def __init__(self, symbol: str) -> None:
        self.symbol = symbol

    def info(self, message: str) -> None:
        log_queue.put(("info", self.symbol, message))

    # ... error, warning, debug, critical ...

def log_worker() -> None:
    """Background thread that drains the log queue and writes to rotating files."""
    while True:
        level, symbol, message = log_queue.get()
        # ... write using logging module ...
        log_queue.task_done()

# Start the worker thread once at module load
log_thread = threading.Thread(target=log_worker, daemon=False)
log_thread.start()

# =============================================================================
# LOCAL DATA SECTION
# =============================================================================

# All embedded defaults, lookup tables, and persisted state structures go here.
default_trade_data_list: List[Dict[str, Any]] = [...]

# =============================================================================
# REMOTE DATA FETCHING (gated behind debug switch and logged)
# =============================================================================

def fetch_klines(symbol: str, interval: str, limit: int, debug_enabled: bool, logger: QueuedLogger) -> pd.DataFrame:
    """Fetch historical klines from the exchange."""
    if debug_enabled:
        logger.debug(f"Fetching klines for {symbol} interval={interval} limit={limit}")
    # ... actual request ...
    if debug_enabled:
        logger.debug(f"Received {len(result)} candles for {symbol}")
    return result
```

## Comment Style by Language
- Python: `#` for one-line, `"""` for multi-line docstrings.
- TypeScript / JavaScript / Go / C-style: `//` for one-line, `/* */` for blocks.
- Never use block comments for single-line statements.

## How to Invoke Manually
If the model does not automatically select this skill, explicitly request it by saying: "Use the elementary-coder skill for this new file / refactor / skeleton." or "Apply elementary-coder standards (descriptive names, full comments, proper ordering, queues, and threading; comments must be kept accurate on every change) to the following code."
