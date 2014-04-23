# An Iterative Approach to Service Oriented Architecture

---

[diagram of one big app]

---

[diagram of many apps talking to each other]

---

* Isolate data
* Isolate interface
* Extract database adapter into client library
* Launch service layer
* Switch client to use service adapter

---

# Why should you listen to me?

[twitter avatar]

* Application developer background
  * ~6 years with Rails
* Recent focus in operations
* Such acronyms, so buzzword
  * TDD, Agile, DevOps, SOA, Cloud, IaaS
* Work at Wanelo
* Broken a surprising amount of things in a short amount of time
* My apartment smells of rich mohagany

---

# Wanelo, why?

* Social network for shopping
  * check it out!
* Databases with billions of records
* Iterated from one large codebase with everything to one central
  codebase with multiple supporting services

---

# Wanelo, why?

* Actually doing a good job of putting all the above buzzwords into practice
  * Very small team able
  * Accomplish much more, much faster than larger companies I've worked at
* Have done much larger things than we've done elsewhere without any real problems
* Done things successfully in Ruby that some people say Ruby can't handle
* DevOps --> http://www.youtube.com/watch?v=LAP1zaXUvAE

---

# But success is not why we're here

* We're not here for the happy ending
* What went wrong?
* What keeps you up (or wakes you up) at night?
* What should I do differently?

---

# Rewind to another company

* Won't name names, but a friend and former co-worker recently
  named it "the Anti-Pattern Goldmine"
* One giant, completely tangled Rails 2 codebase
  * vendored Rails with custom non-forward-compatible changes
* Code terminally tangled
* Five teams all cramming features in as fast as they could write code
  * Code branches diverging for months at a time in complete isolation
  * People working on completely unrelated features were almost guaranteed to
    generate code conflicts

---

# Releases a nightmare

* Monthly when I started
* Eventually weekly
* Change list for each deploy had to be distributed to and explained to executives
* Deploys often took multiple hours
* Annual code-freeze where deployments stopped for three months

---

# Even worse...

* Rough estimates would turn into deadlines with specific dates
* Massive shortcuts were the only way to meet deadlines
* As soon as a project launched, new projects and deadlines would split up a team
* Crazy deadlines and tangled code means code only every gets worse
* Estimates based on worse-case timelines are never worse-case enough, deadlines
  always create more technical debt

---

# When work is a nightmare...

* When writing code is not fun, you're DOING IT WRONG
* Team (especially me) latches onto SOA as THE SOLUTION TO ALL OUR PROBLEMS
* I decide that DevOps is the SOLUTION TO OUR PROCESS

---

# Role change

* I move into the operations team, which was renamed to pretend that it wasn't an
  operations team
* I start working with systems automations software as well as working much
  moar deeply in the OS
* "DevOps" seems to mean that team over there who deploy my code
* SOA is what everyone knows we need to do, but can't get permission from product managers

---

# SOA or nothing

* If you can't succeed...
  * take a firm stance
  * deep a breath
  * cry like a baby
  * ALSO: if there's one thing I've learned from my cats, it's how to throw things on the floor
    [animated gif of cat pushing things off a ledge]

---

# But really...

* At a certain point, we said no more
* New major feature involving login code would be done as a separate application, or not at all
* Meanwhile... internal product sourcing implemented as a separate application
  built around a data service
  * fix data inconsistency between Enterprise software package and home-grown
    Rails app
* "This will be hard and we'll screw up, but we have no other option"

---

# Okay, so what is SOA again?

* Site is composed of multiple applications
* Everything else is arguable
* Sometimes helpful to distringuish
  * internal vs external
  * user interface vs data service

---

# For the purposes of this talk

* application = has a user interface that a human interacts with
* data service = no UI, only an API that other applications or services communicate with
* internal = only accessible to people/programs within the company
* external = consumed or interacted with by users/programs outside of the company

---

# Okay, so why SOA again?

* Scale organization
  * Each team can manage its own codebase
  * Each team can deploy their own changes
  * Outsource portions of functionality without granting keys to the kingdom
* Scale tests
  * Small test suite runs faster
* Scale codebase
  * Performance tuned tightly to workload
  * Hide complexity behind clean interfaces
  * Use language/framework best suited to task
  * Isolation between service layers CAN enforce code isolation

---

# Okay, so back to our protagonists

* data service developed in isolation for 8 months
* required complex state transitions and interactions between multiple applications
* launch was complicated and caused data problems, but since it was internal
  everything could be fixed
* internal applications ambiguously successful
  * they depend on the data service, so yay?
  * data service doesn't entirely solve the core data inconsistencies

---

# What's this 8 month data service thing?

* 6 engineers for 8 months
* Engineering: Yay we didn't mess up the site! Success!!!!
* Product: I don't understand, but we didn't have those resources for 8 months. FAIL!!!!!

---

# But what about that new external thing?

* 2-4 engineers for 3 months + the "DevOps" team
* staging servers available within 2 weeks
* discovered in month 2 that no-one had ever tried deploying the app
* took 2 months to completely refactor our Chef code to configure things "perfectly"
  * engineers on app team did not want to learn Chef, nor Capistrano
* spent an extra 2 weeks just to try to break things, add better error handling

---

# But then it deployed to users!

* Worked exactly as intended
* Major new functionality around login was successful
* So well engineered, we could restart databases in the middle of the day
  * handful of users **might** see an error message with a link to retry
* Engineering: Massive success!!!!1!!1!
* Product: I don't understand, but this took 4 engineers 3 months? FAIL!!!!!!

---

# Was this successful, or not?

* At the time I insisted that the projects were a success
* Now I think they were not successful
  * How could we possibly justify silo-ing 8 engineers for months or years at a time
    without any indication of whether a project will be successful or not?
  * How do we even measure success?
* In retrospect, I don't think we could have done things any differently

---

# Let's go deeper

* Why did we need to do SOA in the first place?
* Engineering did not trust that we could continue developing the site without
  writing new code as separate services
* Engineering did not trust that if we took shortcuts we would be able to fix
  our mistakes later
* Our "Agile" company was planning 12 months in advance with improbable deadlines attached
* Because deployments were painful, product and engineering delayed deployment for
  as long as possible

---

# And even deeper?

* Product did not trust engineering to deliver on promises
* Very rough estimations were turned into fixed dates to try to force accountability
  from engineering
* Product was accountable for features being delivered, not the quality of the features
  nor the engagement of users on the site
* How demoralizing is it to work harder than you've ever worked in your life to
  deploy code on a specific arbitrarily-chosen date, cutting every corner you
  can find, and then not see any change in user engagement?

---

# TRUST

---

* How can you build a world changing product if no one building the product trusts
  one another?
* How can you trust your estimates if you can't trust that you can write correct
  code quickly?

---

# So what did we learn?

* Fuck it, next company we're doing all services all the time, starting from scratch
  * NO!
  * Thankfully I have a boss that is both reasonable and practical, and is extremely
    convincing in arguments
  * Split out services when it is painful, or just before

---

# What did we really learn?

* Real Agile is not a constant sprint until everyone passes out
* Agile is about being thoughtful with each iteration, and using data to guide
  your next iteration
* Red-Green-Refactor is not complete until you refactor
* Service Oriented Architecture will not solve deeper organizational problems
* SOA is a way of describing various tools at our disposal, which we can use to
  solve our pain points

---

# Major releases are never the answer

* If someone comes to you with a project plan that involves 3 months of
  writing code without a single release, you....
  * Nod respectfully
  * Laugh
  * Slap them in the face

  * You say NO

---

# Iteration

* Many small refactorings to allow service extraction
* Feature flags
* Deploy early and often
* Each step prioritized and completed when necessary

---

# When is right to split out or start a new service?

---

# 1) Performance

* Code runs too slowly
* Resource contention
  * Databases
  * Locks

---

# 2) Operational complexity requires code complexity

* For example: sharding PostgreSQL tables in Ruby

---

# 3) New code is completely unrelated to old code

* By unrelated, I mean completely unrelated

---

# With the caveat...

* May go into old codebase short-term for practicality
  * Deploy today is better than deploy next week
  * Requires TRUST that you'll be given the time to fix this when
    it becomes a problem

---

# So... performance

* Site getting slower
* Databases becoming I/O bound on disk
* User growth is crazy

---

# Linear regressions

* One database table is growing completely differently from others
* Reads from one table kill caches for other tables
* We're in the **cloud**, so have limitations on max size of a single server

---

# Linear regressions break down

* Exponential growth means the line is different today vs tomorrow
* Problems **always** crop up before your regression leads you to predict

---

# Decisions

* Without change, the site will stop working
* How to solve the problem without entire engineering dept grinding to a halt
  * 10 engineers at the time

---

# Step 1: Isolate Data

---

# Remove ActiveRecord relations

[code comparison with relations and without]

* Each change is deployable
* Tests!

---

# Pretend it's in a different database

[database.yml]
[establish_connection in model]

* Uses the same database as everything else
* Helpful to deploy this at this point
* Watch out for database connection count

---

# Switch to using a different database

* Create a read-only database replica
* Switch site to maintenance mode
* Push new database.yml
* Promote replica to be a master
* Restart unicorns
* Bring up site
* Sometime later -> remove unused tables

---

# Step 2: Isolate the interface

---

# How many ways are you accessing the table?

* What is your long term growth?
* What are your long term goals?

---

# What does success mean at the database layer?

* Is one database enough?
* Horizontal sharding?

---

# How would you shard your data if you could shard your data?

* Wanelo == Saves
* Saves are viewed by:
  * User
  * Product

---

# Refactor to create an API

[controller calling where on model]
[change to class method on model]

---

# Refactor to reduce the API

* Remove redundancy
* Add tests
  * You will break things very soon!

---

# Every step can be deployed

---

# Step 3: Extract database adapter into a client library

---

# Finders move into a module

*
