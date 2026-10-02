# Security and data handling

## Scope and data flow

The application accepts authenticated chat requests, stores chat/message state, supports document and file routes, and calls a configured model. The chat route enables tool execution. Chat text, uploaded files, retrieved document context, and tool results may be sent to configured model providers or stored in the configured database/blob services.

## Safe handling

- Use synthetic data until provider, database, blob storage, retention, and access controls are reviewed for the deployment.
- Verify authentication and per-user authorization for every chat, document, history, and file route; verify upload size/type limits and storage access.
- Review each enabled tool and its input boundary before deployment. Do not expose tools that can read/write external state without explicit authorization and controls.
- Keep auth secrets, provider keys, database/Redis URLs, and blob credentials in untracked environment configuration. Rotate exposed values.
- Define deletion and retention for chat history, uploaded documents, embeddings, logs, and backups.
- Test abuse limits, prompt injection, cross-user access, and provider failure modes before production use.

## Verification boundary

Repository source shows the routes and tool wiring, but this documentation update did not test authentication, authorization, uploads, provider calls, database isolation, deletion, or deployment controls. This is not a security, privacy, compliance, or production-readiness certification.

Report security issues privately to the repository maintainer; do not include user documents, prompts, or credentials in public issues.
