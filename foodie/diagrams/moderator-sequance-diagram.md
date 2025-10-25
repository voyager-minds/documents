## Sequance Diagram [moderator]

![Alt text](images/moderator-sq.png 'moderator sequance diagram')

#### code for the diagram

```
title Moderator Flow — Pre-Publication Review
autoNumber nested

Customer [icon: user, color: blue]
Web App [icon: monitor, color: blue]
API (Restaurant Review Portal) [icon: globe, color: green]
Content Safety Service [icon: shield, color: orange]
Moderation Service [icon: check-circle, color: purple]
Moderator UI [icon: layout, color: purple]
Review DB [icon: database, color: gray]
Audit Log [icon: file-text, color: gray]
"Email/Notification Service" [icon: mail, color: teal]
"Search/Feed Index" [icon: search, color: teal]

// Customer submits a post
Customer > Web App: Submit post (review or comment) with text + optional photos

// Web App sends to API
Web App > API (Restaurant Review Portal): Submit new post (target type, content, photos)

// API creates post in DB
API (Restaurant Review Portal) > Review DB: Create post (status = PENDING)

// API triggers content safety check (async)
API (Restaurant Review Portal) --> Content Safety Service: Check text (profanity) & images (NSFW) [async]

// API enqueues for moderation
API (Restaurant Review Portal) > Moderation Service: Enqueue for moderation (postId, authorId, status = PENDING)

// Moderator UI fetches moderation queue
Moderator UI > Moderation Service: Fetch moderation queue

// Moderation Service checks for safety results (if available)
Moderation Service > Content Safety Service: Get safety result (if available)

// Moderation Service returns queue items + safety flags
Moderation Service < Moderator UI: Queue items + safety flags

// Moderator opens post details
Moderator > Moderator UI: Open post details

// Moderator UI requests post details
Moderator UI > Moderation Service: Get post details

// Moderation Service reads post and attachments
Moderation Service > Review DB: Read post + attachments

// Moderation Service checks audit log for prior decisions
Moderation Service > Audit Log: Read prior decisions (if any)

// Moderator makes a decision
Moderator > Moderator UI: Decision: Approve or Reject (with reason)

// Decision block
alt [label: Approve, icon: check, color: green] {
  // Approve branch
  Moderator UI > Moderation Service: Approve post (postId, reason)
  Moderation Service > Review DB: Update status = APPROVED; publishAt = now
  Moderation Service > Audit Log: Append immutable record (who/when/why)
  Moderation Service --> "Search/Feed Index": Index published post [async]
  Moderation Service > "Email/Notification Service": Notify author (approved)
}
else [label: Reject, icon: x, color: red] {
  // Reject branch
  Moderator UI > Moderation Service: Reject post (postId, reason)
  Moderation Service > Review DB: Update status = REJECTED
  Moderation Service > Audit Log: Append immutable record (who/when/why)
  Moderation Service > "Email/Notification Service": Notify author (rejected + reason)
}

// Notes (as comments)
// All posts are pre-moderated: nothing is visible until status = APPROVED
// Content Safety runs in parallel; results appear in the queue as flags to assist moderation
// Company/owner replies are handled via a separate sequence (not shown)

```
