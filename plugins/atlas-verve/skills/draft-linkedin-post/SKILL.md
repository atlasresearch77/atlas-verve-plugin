---
name: draft-linkedin-post
description: Draft a LinkedIn post in the user's own voice through Atlas Verve (Atlas Connect), revise it, and send it to the user for approval. Use when asked to write, draft, improve or prepare a LinkedIn post, to list or check their Atlas Verve drafts, or to get a draft ready for review.
---

# Draft a LinkedIn post with Atlas Verve

Atlas Verve is a human-approved LinkedIn content studio. You can read the user's writing settings and drafts, create and
revise drafts, and send a finished draft to the user for review. **You can never approve, schedule or publish a post.** The
user does that themselves in Atlas Verve after signing in. Never say or imply that you published or approved anything.

## Workflow

1. **Understand the topic.** If the user has not given a clear topic, ask for one sentence. Do not invent a topic.
   If they gave only a topic, ask once for a concrete view, example or story they want in it (a sentence is enough) and
   pass it as `perspective`. If they decline, go ahead without one. Do not ask again.
2. **Read their voice once per conversation** with `get_voiceprint`. Use it to judge tone, audience and length. Do not quote
   private details back unnecessarily.
3. **Create the draft** with `create_draft`:
   - `topic`: one or two sentences. Put the user's own point of view in `perspective` when they gave one.
   - `idempotency_key`: a fresh unique value for this intended action (for example `draft-<short-topic>-<timestamp>`).
     Reuse the same key only to retry the *same* call after a timeout or error, never for a new draft.
4. **Say it started, then wait.** `create_draft` returns at once. Immediately tell the user the `post_id` and `title` and
   that the draft is being written (usually 20 to 40 seconds). Then call `get_draft` with `wait_seconds` set to `25`; the
   server waits for you and answers as soon as the draft is ready. Repeat if `status` is still `GENERATING`. Never use a
   shell `sleep`. Stop and report if it becomes `GENERATION_FAILED`, or if two minutes pass.
5. **Show the result.** Give the `title`, the full `text`, and a one-line summary of `claim_review` and `confidence_review`
   (a count of items to check). Point out any claim the user should verify before approving.
   Do not share a review link yet: it comes from `request_approval` once the user is happy.
6. **Revise on request** with `regenerate_draft`. Pass the `draft_version` from your most recent `get_draft`; if the user
   edited the draft meanwhile you will get a `version_conflict`: read it again with `get_draft` and tell the user.
   Use a new `idempotency_key` for each new revision request.
7. **Hand it over** with `request_approval` when the user is happy. Give them the `review_url` and say clearly that they must
   open it, sign in, review and approve it themselves.

## Listing and checking drafts

Use `list_drafts` (filter with `status`, for example `PUBLISHED` or `DRAFT`) and show each item's `title` and status.
Use `get_draft` for the full text of one post. Draft text belongs to the user; do not summarise it to anyone else.

## When something is refused

Read the `message` in the error and tell the user in plain words:

- Atlas Connect is off for their account (`not_enabled_by_user`): ask them to open Atlas Verve **Settings** and turn on
  **Atlas Connect**, then retry.
- Atlas Connect is unavailable or paused (`feature_disabled`) or switched off by an administrator (`revoked_by_admin`):
  tell them, and do not try to work around it.
- `rate_limited`: wait for the time given, then retry once.
- `needs authentication` or a 401: ask them to run `/mcp`, choose **atlas-verve** and authenticate.

## Rules

- Never claim a post is approved or published. At most say it is "ready for your review".
- Do not create drafts the user did not ask for, and do not create several drafts to "try" topics.
- Do not paste secrets, tokens or the user's private writing prompt into drafts or replies.
- If the user asks you to approve or publish, explain that only they can, in Atlas Verve, and share the review link.
