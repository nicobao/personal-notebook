## My opinionated software project development guideline

Status: work-in-progress.

## Tools

- GitHub
- A shared calendar
- StackOverflow
- Matrix/Element/Slack/Mattermost/IRC (some instant messaging tools with public channels, audio and video calls)

## Rationale

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

## Usage of GitHub

Tools like Jira are unecessary. GitHub already contains project/product management tools:
https://github.com/features/issues

- the #1 source of truth
- having too many tools create cognitive overhead (where should I store it? where is the information?). It is counter-productive.
- put maximum info there
- use it for project/product management (handling sprint, tasks, assignments etc)
- use it to store code
- use it for documentation
- only use other tools when absolutely necessary
- this helps with getting the code as close as possible with the reality of the product features
- it helps for following changes, testing
- it helps for automating end-to-end testing
- it helps for generating reports

## Usage of calendar

Everyone must keep up to date the shared calendar and put there:
- their availibilities for one-to-one meeting
- the time when they are concentrated on work and should not be interrupted
- the time when they are off

## Usage of instant messaging (Slack/Matrix etc)

- avoid it at all cost for things related to issues/product. Prefer writing in the relevant GitHub issue.
- prefer GitHub discussions for something related to the project that doesn't fit into one particular issue
- use it only when absolutely necessary.
- When using instant messaging tool, prefer public channels if what you write are interesting for others (which is the case 90% of the time)

## Usage of StackOverflow

- when a question is asked about something unrelated to the specific work at hand, but is rather a question of technology in general, use StackOverflow, and respond there.
- list relevant StackOverflow questions in the documentation on GitHub (on some readme page or in the wiki)

## Where to put documentation?
- on the GitHub monorepo's READMEs
- on the GitHub wiki page
- link wiki pages in README

## Sprints and meetings

Agile development licycle has a lot of upsides. Let's keep that. We work in batches of 3 weeks long sprints.

For every sprints there are:
  - A leader
  - A secretary

The leader role is:
- to make sure the process is respected
- to lead the meetings

The secretary role is:
- to take notes during each meetings and put them on Github Discussions

The identities of the leader/secretary are assigned successively to each team participant in turn.

### Meetings

There are 6 types of meetings, each with their specific purpose.

1. Every 3 weeks at the beginning of the sprint:
	- We do a special start-of-sprint meeting
	- This should be the culmination of the three weeks of work from the product team. Most of the use-case/epics/issues should already have been created.
	- We plan the product features we want to get done in a 1-2 hour long meeting
		- We assign tasks to people
		- We put all this information in GitHub (epic/issues/tags/project tables and boards)
		- We must leave space for uncertainties. Urgent issues may be added to the plan during the sprints.
		- We must take into consideration all the duties that's not directly related to the product. It's easy to forget them (like writing this document, like commuting or installing dependencies on one's computer, fixing issues with one's personal router etc) when planning time.
1. Morning 30 minutes Daily meeting
	- 30 minutes max. 
	- These meetings have two goals:
		- motivate everyone to wake up in the morning, deliver and create a sense of commitment
		- give everyone an overall understanding about what others are doing and their throught process. No discussion about issues neither in terms of implementations or features are expected at all. It's one person then the next who present what they do. Discussions, if necessary, will take place later in GitHub issues or, if really necessary in separate sync meetings.
	- 5 persons max per meeting. 
	- At the beginning of the meeting, we say hi for 2 minutes.
	- Then each participant explains in _5 minutes maximum_:
		- What they did yesterday. Without getting into the dirty details. Sharing important info such as "I chose this architecture, it is documented here. For those who are interested, respond to the issue or let's organize a call later."
		- Where they are blocked if that's the case
		- What they need from others
		- What they are going to do today 
	- At the end of the meeting, we take 2 minutes to talk about what will be discussed tomorrow. 
1. Specific meetings to solve specific issue  when async discussions aren't enough - only with stakeholders who are involved in the task at hand. Those are planned in the shared calendar by the one who need help with a _very precise meeting agenda_. The scope shouldn't be "let's just talk". The person who is planning the meeting is responsible for taking the minutes and putting them in the relevant GitHub Issue/Discussions.
1. "Coffeee break" type of meetings. Usually around 15 minutes - just to relax and chat. It's important to keep healthy socially...
1. Every 3 weeks, at the end of the sprint for 1-2 hours:
	- We close the issues, we close the sprint, we tag in git a stable version and deploy the code in production. We discuss the problems we encountered.
1. Every 3 weeks at most (optional - on demand):
	- We discuss long-term vision, issues in a 2 hours meeting


## Typical day of work

- Start working at 09:00. Review code of others until 09:30.
- 09:30 => daily meeting until 10:00. 
- 10:00 to end of day => get your shit done. Create a DRAFT PR for your issue and link to the issue. Set up meetings with relevant people if and only if it's not necessary to fix it otherwise. Staging must be deployed automatically everytime a PR is merged to main. It must represent the latest development work and tested immediately.
- end of the day => push work-in-progress code. It doesn't matter if it doesn't build. It's important to allow code reviews and for other to find architectural mistakes before too much work has been done.

## What to do when there is an emergency and people are on holiday/unavailable in their calendar?

If there is an emergency and you absolutely need to contact someone who's on holiday or who's unavailable in their calendar: *call them*.

_DO NOT USE TEXT MESSAGE_ or email for emergency. Otherwise it creates an unecessary stress for people who are supposed to either relax or be concentrated to get their task done. With text messages, they'd have to constantly check their messaging app in case of an emergency. It's not healthy at all if you're supposed to be off, it is counter-productive if you need to concentrate, and it leads to burnout.

_USE CALL ONLY_ when there is an emergency. And only call for a _real_ emergency (that's _very_ rare).

When you're away or in "concentrating-mode", mute your messaging app. Only keep your phone in case someone calls you for an emergency. Nobody calls you? Then you can just chill out.

## Can you send people messages when there are unavailable?

YES, you should. For anything that's not an emergency. Open issues, tag them, respond to their GitHub issues. Send them text on instant messaging app if you want. But, don't expect a response *right now*.

It's up to the responsibility of the person that's away to cut off notifications when they are away/concentrating.
