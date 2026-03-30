# Support Agent App — Completed

## Projects
- FAQs: i8FQRPsSe5fTEXNz
- Tickets: 6NuCHBAPtKyXM5pH
- Resolutions: grC1rXxwT3mb7Tnj
- Escalations: itit81ZRyTtZ8TLV

## Agent
- Support Agent: 01KMWTQ15FJPRQ209PM5JSZK5R (public)
- Public URL: https://staging.taskade.dev/a/01KMWTQBR2H17NNQ8TSJSWJTWG
- Knowledge: FAQs + Resolutions connected

## Automations
- Ticket Form — Route & Notify: 01KMWTQGHXTQP9Q57CV0358P77
- Daily Unresolved Queue: 01KMWTRHHKTA81TGPG3748GVM7 (daily 9AM SGT)

## App (Genesis)
- 3 views: ChatView, TicketInbox, KnowledgeBase
- Agent Chat SDK used for chat
- All APIs integrated
- Status: COMPLETE

## Bug Fixes

### Fix: "No conversation available" error on chat start
**File:** `app/src/components/ChatView.tsx`
**Date:** 2026-03-29

**Root Cause:**
When a starter suggestion was clicked, `handleStart()` created a new conversation and then called `sendMessage()` inside a `setTimeout`. The SDK's SSE stream had not yet connected by the time `sendMessage` fired, causing the error "No conversation available. Create a conversation first."

**Fix:**
- Removed the `setTimeout` approach.
- Added a `pendingMessageRef` to store the initial message after conversation creation.
- Added a `hasSentPendingRef` to prevent double-sends.
- Added a `useEffect` watching `isConnected + conversationId` — the pending message is only sent once the stream confirms it is connected.
- Reset both refs in `handleNewConversation()` to avoid stale state across sessions.
