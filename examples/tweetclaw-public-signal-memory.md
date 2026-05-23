# TweetClaw Public Signal Memory

Use Context Bridge with TweetClaw when an OpenClaw workspace needs to remember public X/Twitter signals across multi-day work. TweetClaw handles X/Twitter automation through the official `@xquik/tweetclaw` OpenClaw plugin. Context Bridge keeps the reviewed decisions, summaries, follow-ups, and source references searchable after the live search work is done.

This recipe is useful for launch monitoring, support triage, competitor tracking, user research, community operations, and giveaway postmortems.

## Tool Boundary

| Tool | Owns | Should Not Store |
| --- | --- | --- |
| Context Bridge | Reviewed summaries, decisions, follow-up tasks, public tweet URLs, query names, and campaign notes | API keys, signing keys, session files, webhook payloads, direct message bodies, raw timelines, or unreviewed post text |
| TweetClaw | Search tweets, search tweet replies, user lookup, follower export, monitor tweets, media workflows, webhooks, giveaway draws, and reviewed post tweets or post tweet replies | Long-term project memory or team handoff notes |

## Install

Install Context Bridge first, then add TweetClaw as the X/Twitter tool source:

```bash
openclaw plugins install @xquik/tweetclaw
openclaw config set plugins.entries.tweetclaw.config.apiKey "$XQUIK_API_KEY"
```

Use the npm package as the canonical install source. The ClawHub page is useful for browsing, but npm is the runtime install path for TweetClaw.

## Workflow

1. Define a context name before searching.

```bash
echo "save context as: launch-x-signals - Public X/Twitter signals for the launch window" | openclaw agent
```

2. Ask the agent to collect only the public evidence needed for the decision.

```text
Use TweetClaw to search tweets and tweet replies about "{product or campaign}" from the last 24 hours.
Return public tweet URLs, author handles, engagement counts when available, recurring objections, recurring praise, and any follow-up questions.
Do not store API keys, direct messages, cookies, webhook payloads, or unreviewed draft posts in memory.
```

3. Convert raw search results into a compact memory entry.

```text
Save this to Context Bridge as launch-x-signals:
- signal type: launch feedback
- queries used:
- public tweet URLs:
- summary:
- decisions:
- follow-ups:
- redactions applied:
```

4. Review actions separately before posting.

```text
Draft possible post tweets or post tweet replies from the reviewed signals.
Do not publish anything until the operator explicitly approves the final text.
After approval or rejection, save only the decision and public URL references to Context Bridge.
```

## Memory Shape

Keep each saved entry short and searchable:

```json
{
  "source": "tweetclaw",
  "context": "launch-x-signals",
  "observed_at": "2026-05-23T00:00:00Z",
  "queries": ["product launch", "product support"],
  "public_urls": ["https://x.com/example/status/123"],
  "author_handles": ["example"],
  "summary": "3 recurring setup questions and 1 pricing concern.",
  "decisions": ["Add setup screenshot to docs."],
  "follow_ups": ["Reply to public setup question after approval."],
  "redactions": ["No direct messages, secrets, cookies, or webhook payloads stored."]
}
```

## Useful TweetClaw Jobs

- Search tweets for launch, support, market, or incident terms.
- Search tweet replies to understand objections around a specific public post.
- Run user lookup for public profile context before a support handoff.
- Export followers for approved audience analysis.
- Monitor tweets and send reviewed summaries into Context Bridge.
- Use webhooks for event-driven triage, storing only sanitized summaries.
- Run giveaway draws and save the public audit summary.
- Draft post tweets or post tweet replies only after operator review.

## Review Checklist

- The query is narrow enough to support a decision.
- Every saved URL is public.
- Stored notes exclude secrets, cookies, direct messages, raw session files, and raw webhook payloads.
- Draft posts and replies remain drafts until explicitly approved.
- Context Bridge stores the final decision, not every raw TweetClaw result.

## Links

- [TweetClaw GitHub repo](https://github.com/Xquik-dev/tweetclaw)
- [TweetClaw npm package](https://www.npmjs.com/package/@xquik/tweetclaw)
- [TweetClaw ClawHub browsing page](https://clawhub.ai/plugins/@xquik/tweetclaw)
- [Xquik API docs](https://docs.xquik.com/)
