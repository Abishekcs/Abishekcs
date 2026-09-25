# [Abishekcs](https://github.com/Abishekcs)/README.md

## Hi there 👋

I'm Abhishek  a systems and backend engineer with a growing interest in compilers, and an active open source contributor. Most of that work lives across two places: [Rage](https://github.com/rage-rb/rage), an async Ruby framework, and the [Wiki Education Dashboard](https://github.com/WikiEducationFoundation/WikiEduDashboard), a Rails app used by 191,399+ educators. Right now I'm heads-down on [Rage's observability project](https://github.com/rage-rb/rage/issues/379), trying to work out how to expose event-loop lag natively from Iodine instead of relying on instrumentation.

### Rage

One of the many project I worked on is [`Rage::OpenAPI::Parsers::Ext::Blueprinter`](https://github.com/rage-rb/rage/pulls?q=is%3Apr+author%3AAbishekcs+blueprinter) static analysis that generates OpenAPI schemas straight from Blueprinter serializer classes. It didn't start out that ambitious. I began with just [a scaffold and some class detection](https://github.com/rage-rb/rage/pull/298), since that piece blocked everything else I wanted to build. From there it grew feature by feature: [basic field parsing](https://github.com/rage-rb/rage/pull/300), extending the [`@response`/`@request` tag syntax](https://github.com/rage-rb/rage/pull/299) to accept serializer options, then [association declarations](https://github.com/rage-rb/rage/pull/326), [blueprint inheritance](https://github.com/rage-rb/rage/pull/302), and [view parsing](https://github.com/rage-rb/rage/pull/335) for `blocks`, `include_view`, and `exclude`. Somewhere in there I also worked out [transformer key detection](https://github.com/rage-rb/rage/pull/317) and cleaned up a couple of rough edges [registry key normalization](https://github.com/rage-rb/rage/pull/340) and [root key support](https://github.com/rage-rb/rage/pull/343) among them.

Once the feature set finally settled, I did something I don't always have the patience for: I went back and threw away my own first approach. The parser had started life walking a Prism AST by hand, which worked but was fragile, so I [rebuilt its core traversal on top of Blueprinter's own Reflection API](https://github.com/rage-rb/rage/pull/323) instead less code, fewer edge cases, and a lot easier for someone else to pick up after me. That whole arc, scaffold first, iterate until it's solid, then simplify once I actually understand the problem, is basically how I like to build anything. It's also what got me invited to join Rage's core maintainer team.

From the parser I drifted into the framework's background task system, adding [native periodic task scheduling](https://github.com/rage-rb/rage/pull/277) to `Rage::Deferred` through distributed leader election, and later making sure [task ID ordering survives an unclean restart](https://github.com/rage-rb/rage/pull/255) instead of silently breaking after a crash. Most recently I've been building out Rage's telemetry layer[`Rage::Telemetry.every`](https://github.com/rage-rb/rage/pull/381) for periodic scheduling and a [`Telemetry::Capacity`](https://github.com/rage-rb/rage/pull/396) module as part of a larger [observability project](https://github.com/rage-rb/rage/issues/379) I'm still in the middle of. The socket and connection metrics are done; what I'm chasing right now is a way to measure event-loop lag natively from Iodine rather than bolting it on through instrumentation, because a framework running in production should be able to tell you what it's actually doing, not just what you hope it's doing.

### WikiEduDashboard

If Rage is where I get to build things from nothing, WikiEduDashboard is where I've spent most of my time chasing down things that were already broken, just not obviously. A lot of my 120+ merged PRs there start the same way: a page is slow, and it turns out to be N+1 queries hiding in surveys, statistics, or article data. One report page went from roughly 20 seconds to under a second once I found and fixed the culprit. The same instinct led me to the platform's automated monitor services — the GA nomination and Discretionary Sanctions checks, among others which were scanning far more of the database than they needed to until I scoped their queries to the right namespaces.

The most satisfying find, though, was tracing the platform's recurring downtime back to a single unbounded pagination query and fixing it with deferred-join pagination instead the kind of bug that's easy to shrug off as "just how the app is" until you actually go looking. Around the same time, I migrated the app's Wikipedia/Wikimedia authentication from OAuth 1.0 to OAuth 2.0 ahead of Wikimedia's own deprecation deadline, so login didn't quietly stop working for anyone.

It's not all performance and plumbing I've shipped a News feed and an admin Notes system that course organizers use directly, and untangled a string of Redux state-mutation bugs that had been quietly corrupting what people saw in the timeline and calendar views.

---


![haikyuu-icegif-29](https://github.com/user-attachments/assets/589d80a8-c27d-4b90-9048-005a2e0cef39)
