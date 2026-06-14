# Sprout Social GraphQL Schema

## Overview

This GraphQL schema is a conceptual representation of the Sprout Social REST API surface, modeling the core entities, relationships, and operations available through the [Sprout Social Public API](https://api.sproutsocial.com/docs/). Sprout Social is a social media management platform that enables publishing, monitoring, analytics, messaging, and listening across major social networks including Instagram, Facebook, LinkedIn, TikTok, YouTube, and X.

The schema captures the full domain model: social profiles and accounts, content publishing and scheduling, inbox and messaging, engagement and analytics, reporting, listening streams, tagging, task management, and API credential management.

## Schema Source

Derived from the Sprout Social Public API documentation at https://api.sproutsocial.com/docs/

## Types

### Profile and Account Types

- **Profile** — A Sprout Social profile representing a connected social media account, with network, display name, and metrics.
- **ProfileDetails** — Extended metadata for a profile including bio, follower counts, and network-specific identifiers.
- **ProfileNetwork** — Enum of supported social networks (Instagram, Facebook, LinkedIn, TikTok, YouTube, X, Pinterest, Google Business).
- **SocialAccount** — A user-facing social account object linking an authenticated platform account to a Sprout profile.
- **AccountType** — Enum distinguishing account roles such as business, creator, personal, or page.
- **SocialMetrics** — Aggregate metric snapshot for a social account covering followers, following, posts, and engagement rate.

### Post and Publishing Types

- **Post** — A social media post managed through Sprout Social, with content, status, scheduling, and targeting.
- **PostDetails** — Full detail view of a post including network-specific fields, media, tags, and approval state.
- **PostStatus** — Enum for post lifecycle: draft, scheduled, sent, failed, cancelled, needs_approval, approved.
- **PostType** — Enum for content type: text, image, video, story, reel, carousel, link, poll.
- **PostMedia** — Media items attached to a post including images and videos.
- **MediaItem** — An individual media asset with URL, type, dimensions, and alt text.
- **PostSchedule** — Scheduling configuration for a post including send time, timezone, and auto-scheduling preference.
- **PostAudience** — Targeting parameters for a post such as location, language, age range, and gender.
- **DraftPost** — A post in draft state not yet submitted for scheduling or approval.
- **PublishedPost** — A post that has been successfully delivered to the social network.
- **ScheduledPost** — A post queued for future delivery with a confirmed send time.
- **SentPost** — A post that has been sent, with delivery confirmation and network post ID.
- **FailedPost** — A post that failed to deliver, with error code and retry information.

### Group and Organization Types

- **Group** — A Sprout Social group organizing profiles for team collaboration and reporting.
- **GroupDetails** — Extended group information including member profiles, permissions, and created date.
- **GroupProfile** — The relationship between a group and a constituent profile.

### Stream and Listening Types

- **Stream** — A Sprout Social listening stream monitoring keywords, mentions, hashtags, or accounts.
- **StreamDetails** — Full stream configuration including filters, volume, and alert thresholds.
- **StreamFilter** — Filter criteria applied to a stream such as keywords, languages, locations, and sentiment.
- **FeedMessage** — A message surfaced by a listening stream from monitored sources.

### Inbox and Messaging Types

- **InboxMessage** — A message appearing in the Sprout Social Smart Inbox from any connected social network.
- **MessageType** — Enum for inbox message type: mention, comment, direct_message, review, reply, story_mention.
- **ReplyMessage** — A reply composed in Sprout Social sent in response to an inbox message.
- **MessageDetails** — Full details of an inbox message including author, content, sentiment, and assigned tasks.
- **Author** — The social media user who authored a message or post.
- **AuthorDetails** — Extended author profile including follower count, bio, and network-specific identifiers.
- **AuthorNetwork** — The social network context for an author profile.

### Engagement Types

- **Engagement** — An engagement event on a post or message such as a like, share, comment, or click.
- **EngagementType** — Enum for engagement event: like, share, comment, click, save, reaction, impression, reach.
- **Comment** — A comment on a published post, with author, content, and reply chain.
- **MessageAttachment** — A media attachment within a message or comment.

### Workflow and Collaboration Types

- **Task** — A task assigned to a team member related to a message, post, or contact.
- **Assignment** — The assignment of a task or message to a specific Sprout Social user.
- **Note** — An internal note added to a message, profile, or task visible only to the team.
- **Tag** — A label applied to messages, profiles, or posts for categorization and reporting.
- **TagDetails** — Full tag metadata including color, description, usage count, and created date.

### Sentiment and NLP Types

- **SentimentScore** — Machine-learning sentiment classification for a message: positive, neutral, negative, with confidence score.

### Analytics Types

- **Analytics** — Top-level analytics container for a date range across profiles and networks.
- **ProfileAnalytics** — Analytics metrics scoped to a single profile covering reach, impressions, and engagement.
- **NetworkAnalytics** — Aggregated analytics across all profiles on a given network.
- **PostAnalytics** — Performance metrics for an individual post including impressions, reach, and engagements.
- **EngagementAnalytics** — Breakdown of engagement by type over a time period for profiles or posts.
- **SentimentAnalytics** — Sentiment distribution analytics for inbox messages and listening stream content.

### Reporting Types

- **Report** — A Sprout Social report combining analytics across profiles, groups, and date ranges.
- **ReportDetails** — Full report configuration including sections, metrics, date range, and export format.

### Inbox Management Types

- **SmartInboxFilter** — A saved filter configuration for the Smart Inbox scoping by profile, message type, tag, or assignment.

### Calendar Types

- **ContentCalendar** — The content calendar view of scheduled and published posts across profiles.
- **CalendarView** — A specific calendar view configuration (day, week, month) with profile and group scope.

### Credential and Security Types

- **APIKey** — An API key credential used to authenticate requests to the Sprout Social API.
- **Token** — An OAuth access token for a connected social account or Sprout API session.
- **Webhook** — A webhook endpoint configured to receive real-time event notifications from Sprout Social.

## Queries

- `profile(id: ID!)` — Fetch a single profile by ID.
- `profiles(groupId: ID, network: ProfileNetwork)` — List profiles, optionally filtered by group or network.
- `post(id: ID!)` — Fetch a single post by ID.
- `posts(profileId: ID, status: PostStatus, type: PostType, from: String, to: String)` — List posts with filters.
- `inboxMessages(profileId: ID, type: MessageType, filter: SmartInboxFilter)` — List inbox messages.
- `stream(id: ID!)` — Fetch a single listening stream.
- `streams` — List all listening streams.
- `analytics(profileId: ID!, from: String!, to: String!)` — Fetch analytics for a profile and date range.
- `report(id: ID!)` — Fetch a saved report.
- `tags` — List all tags.
- `groups` — List all groups.

## Mutations

- `createPost(input: PostInput!)` — Create a new post (draft or scheduled).
- `updatePost(id: ID!, input: PostInput!)` — Update an existing post.
- `deletePost(id: ID!)` — Delete a post.
- `sendReply(messageId: ID!, input: ReplyInput!)` — Send a reply to an inbox message.
- `assignMessage(messageId: ID!, userId: ID!)` — Assign an inbox message to a user.
- `addTag(targetId: ID!, tagId: ID!)` — Apply a tag to a message or post.
- `createTask(input: TaskInput!)` — Create a workflow task.
- `addNote(targetId: ID!, content: String!)` — Add an internal note.
