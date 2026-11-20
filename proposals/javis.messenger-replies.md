# Proposal: Natural Messenger reactions and replies

## Problem

The existing Meta Ads Graph pack provides read-only advertising tools. Its Meta app can be reused by Messenger, but the pack does not change Page inbox behavior.

A sales bot needs two native Messenger behaviors:

1. React to the customer's exact inbound message when the AI selects an appropriate reaction.
2. Anchor the first text response to the exact inbound message with `reply_to.mid`.

This matters when customers send several short messages in sequence and expect a clear, natural response.

## Proposed scope

- Facebook Page private inbox only.
- Exclude Facebook comments.
- Exclude Instagram.
- Reuse the existing `[CAM_XUC:<name>]` output contract.
- Send `sender_action=REACT`, an emoji `payload`, and the inbound `message_id`.
- Add `reply_to: { mid }` to the first text response.
- Support reaction-only turns without an empty text bubble.
- Reuse the existing Page access token and Graph endpoint.
- Add no new credentials or external services.

## Packaging decision requested

Please choose one of these locations:

1. Implement in `javis-os` as shared Messenger transport behavior.
2. Publish a separate Store code pack named `javis.messenger-replies`.

The behavior should not be added silently to `javis.meta-ads-graph`. Installing an Ads reader should not implicitly change chatbot runtime behavior.

## Compatibility and safety

A working prototype is narrowly gated to Javis `>=0.611.0,<0.612.0`. As a code package, it remains disabled until the owner reviews and enables it.

## Validation completed

- Private inbox reply is anchored to the correct inbound MID.
- Reaction targets the correct inbound MID.
- Reaction-only output creates no empty bubble.
- Facebook comments receive neither reaction nor `reply_to`.
- Instagram is unchanged.
- Zalo is unchanged.
- Store validation passes with 388 packages and 388 index entries.

A complete local package, release zip, index entry, and regression test are ready. The release artifact can be submitted after the publisher confirms whether this belongs in core or in the Store.
