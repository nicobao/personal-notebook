## My opinionated software project development guideline

Status: work-in-progress.

Tools: 
- GitHub
- A shared calendar
- StackOverflow
- Matrix/Element/Slack/Mattermost

Rationale:
- Every stakeholder in the development of the project must produce something. Either documentation or code. Documentation is as important as code. Documentation for everything not just code: product, strategy etc.
- Nobody should hold knowledge for themselves and be irreplacable. That's bad for the survival of the project.
- It's not faster to talk than to write. Maybe when writing it feels long. But we save time at the end, and quality. A lot is lost in translation, a lot is forgotten. Most misunderstanding comes from oral discussions. We think the other person understands us but they don't.
- Writing has the added benefit of generating documentation.
- Async first. People aren't in the same timezone eventually. People aren't available at the same time.
- We don't want to make people lose their time. Else they lose momentum and motivation. Give them time to think before responding. The first answer is usually not good.
- We want to allow people to focus.
- We want to empower people, give them responsibilities. Trust them.
- We want to build a process that's openable to externals easily.
- Sync when necessary for a specific purpose and only with the people that are concerned by the issue.
- Motivate each other by organizing _short_ daily calls in the morning.
- In general, follow the [Async Manifesto](http://asyncmanifesto.org/)

Everyone must keep up to date the shared calendar and put there:
- their availibilities for one-to-one meeting
- the time when they are concentrated on work and should not be interrupted
- the time when they are off

Every 3 weeks:
- We do a special start-of-sprint meeting
- We plan the product features we want to get done in a 1-2 hour long meeting
- We assign tasks to people
- We put all this information in GitHub (epic/issues/tags/project tables and boards https://github.com/features/issues )

Every 3 weeks at most (optional - on demand):
- We discuss long-term vision, issues in a 2 hours meeting

Typical day of work:
- Start working at 09:00. Review code of others until 09:30.
- 09:30 => daily meetings. 30 minutes max. 5 persons max per meeting. Someone must be assigned to take notes and put them on GitHub (Discussions?). During daily meetup, we say hi for 2 minutes. Then each participant explains in _5 minutes maximum_:
	- What they did yesterday. Without getting into the dirty details. Sharing important info such as "I chose this architecture, it is documented here. For those who are interested, respond to the issue or let's organize a call later."
	- Where they are blocked if that's the case
	- What they need from others
- 10:00 to end of day => get your shit done. Create a DRAFT PR for your issue. Set up meeting with relevant people if and only if it's not necessary to fix it otherwise
- end of the day => push work-in-progress code. It doesn't matter if it doesn't build. It's important to allow code reviews and for other to find architectural mistakes before too much work has been done.

Usage of instant messaging (Slack/Matrix etc):
- avoid it at all cost for things related to issues/product. Prefer writing in the relevant GitHub issue.
- prefer GitHub discussions for something related to the project that doesn't fit into one particular issue
- use it only when aboslutely necessary.
- When using instant messaging tool, prefer DM ONLY if the things you write aren't interesting for others

Usage of StackOverflow:
- when a question is asked about something unrelated to the specific work at hand, but is rather a question of technology in general, use StackOverflow, and respond there.

Where to put documentation?
- on the GitHub monorepo's READMEs
- on the GitHub wiki page
- link wiki pages in README
