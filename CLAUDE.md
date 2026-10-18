# Claude adapter

This file only routes a fresh Claude or Claude Code session to this repository's canonical Project Memory. It holds no project facts.

1. Read [PROJECT.md](PROJECT.md) and follow its load map, starting with Tier 0. The format is [Project Memory Contract v1](https://github.com/CaffeinatedDad/agent-governance/blob/main/docs/PROJECT-MEMORY.md).
2. Verify the repository, canonical branch and HEAD read-only before relying on project state. Do not fetch, pull or otherwise mutate Git just to prove freshness; report anything you cannot verify.
3. Return the Project Memory hydration receipt before recommending or executing project work.
4. This adapter and Project Memory grant no execution authority. Master review, owner approval and every separate gate still apply.
5. Provider-native memory and chat history are not canonical project truth; the repository is.
