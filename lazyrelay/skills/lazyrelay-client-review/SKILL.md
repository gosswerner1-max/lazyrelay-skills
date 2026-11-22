---
name: lazyrelay-client-review
description: Use when a freelancer or agency wants a client to approve posts before they go out. Makes a LazyRelay approval link for a batch (the client needs no account), reads the client's feedback, and helps answer it. Approves a post only when the user says so.
---

# LazyRelay client review

For people who post on behalf of someone else. The client looks at the posts and says yes or asks for a change. They do not need a LazyRelay account. Tell the user in plain words what you are doing at each step.

## Before you start

Client review links depend on the plan: the Free plan has none. If `create_review_link` is refused, tell the user it is a plan limit and to check the pricing page on lazyrelay.com. Do not work around it.

## Steps

1. Ask which posts or which week the client should see. Use `list_scheduled_posts` to find them; show a short table and confirm the list.
2. Call `create_review_link` for those posts. If the tool needs the posts passed a certain way, follow what the tool says and show the user what you passed.
3. Give the user the link and a plain sentence to send with it, for example: "These are the posts planned for next week. Open the link, look at each one and tell me if anything should change." **Warn the user:** anyone who has this link can open it, so send it only to your client.
4. Later, when the user asks what the client said, call `get_post_feedback` and `list_review_links`. Show the feedback per post, in the client's own words, with the post it belongs to.
5. If the user wants to answer a comment, draft the reply and show it. Call `reply_to_post_feedback` only after the user approves the exact words.
6. Before `approve_post`, show the user the post's text and the exact time it will go out, and say that approving a waiting post can release it. Call it only when the user says to approve that specific post. Never approve in bulk on your own judgement.
7. If the user wants the link to stop working, call `revoke_review_link` after they say so.

## Rules

- You never speak for the client and never tell the client anything the user has not approved.
- Never change a post's text because feedback seems to ask for it without showing the user the new text first. Use `update_post` only after the user agrees.
- Keep client names and comments inside this conversation. Do not copy them into files unless the user asks.
