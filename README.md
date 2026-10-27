# Nietzschian Debugger skill

A portable debugging coach adapted from
[DilawarShafiq/nietzschian-debugger](https://github.com/DilawarShafiq/nietzschian-debugger).
It asks focused questions about evidence and assumptions so you discover the
cause yourself. Three styles are available: `socrates` (gentle), `nietzsche`
(direct, the default), and `zarathustra` (rigorous challenges).

The package uses the [Agent Skills format](https://agentskills.io/specification).
`SKILL.md` is the complete runtime instruction set. It has no external scripts,
dependencies, API keys, model selection, tool bindings, or harness-specific
frontmatter. Use it in a harness with Agent Skills support, or load the Markdown
manually in any assistant that accepts instructions. Automatic discovery and
invocation syntax depend on the host; the skill cannot add missing host features.

## Install

Clone this repository directly into your host's skill directory, using
`nietzschian-debugger` as the folder name.

For Codex:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/sebmellen/nietzschian-debugger-skill.git ~/.agents/skills/nietzschian-debugger
```

For Claude Code:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/sebmellen/nietzschian-debugger-skill.git ~/.claude/skills/nietzschian-debugger
```

Alternatively, download the repository and copy its files into a folder named
`nietzschian-debugger` in your host's skill directory. The folder must contain
`SKILL.md` directly. Keep `LICENSE` with redistributed copies. Do not overwrite
an existing installation without reviewing it.

- **Codex:** `~/.agents/skills/nietzschian-debugger/` for personal use, or
  `.agents/skills/nietzschian-debugger/` inside a repository. For a harness that
  exposes `$CODEX_HOME/skills` or `~/.codex/skills`, use that location instead.
  See the [Codex skill documentation](https://developers.openai.com/codex/skills/).
- **Claude Code:** `~/.claude/skills/nietzschian-debugger/` for personal use, or
  `.claude/skills/nietzschian-debugger/` inside a repository. See the
  [Claude Code skill documentation](https://code.claude.com/docs/en/skills).
- **Other hosts:** Use the host's documented Agent Skills directory or import
  flow. There is no universal discovery path.
- **Without skill support:** Attach or paste `SKILL.md`, then ask the assistant
  to follow it for the current debugging session. Provide relevant code excerpts
  if that host cannot read your files.

On systems with symlink support, Codex and Claude Code can share one source folder
through directory symlinks in their discovery locations. Otherwise copy it.

## Invoke

Codex:

```text
$nietzschian-debugger intensity=socrates
Coach me through why this test passes alone but fails in the full suite.
```

Claude Code:

```text
/nietzschian-debugger intensity=zarathustra My API fails intermittently; challenge my assumptions.
```

Any host, after loading the skill:

```text
Use the Nietzschian Debugger in nietzsche mode.
Help me find why login fails for some users without giving me the answer.
```

Use `solved` or `found it` to finish, `I give up` for one lifeline question, and
`exit`, `quit`, or `stop` to end. Change intensity in normal language. Ask for a
direct explanation or fix to leave coaching mode. Ordinary requests to fix a bug
should not activate this skill on their own.

If a newly installed skill does not appear, start a new session in the host.

## Adaptation and limits

The source is pinned to
[`94e452d3fc83acb6360ac2918562961766c9753a`](https://github.com/DilawarShafiq/nietzschian-debugger/tree/94e452d3fc83acb6360ac2918562961766c9753a).
The adaptation draws on its `llm/prompts.py`, `llm/validator.py`,
`core/session_loop.py`, and `core/context_manager.py` under
`nietzschian_debugger/`.

It preserves question-led debugging, the three intensity levels, use of actual
code, a lifeline, session closure, and concise continuity notes. The host handles
file access and conversation context. CLI rendering, model routing, automatic
session storage, growth-score computation, and the quote database are not bundled.
Responses are guided by instructions rather than checked by the original Python
validator; behavior still depends on the host model and its higher-priority rules.

Two intentional changes make it suitable as a reusable skill: a later request
for direct help exits coaching, and the strongest intensity challenges reasoning
without personal attacks. Feedback is based on observed behavior rather than
invented numerical scores. No project files are edited during coaching.

The upstream MIT copyright and permission notice are preserved in [LICENSE](LICENSE).
