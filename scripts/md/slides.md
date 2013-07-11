
title: Reality of Servers
class: big

- There are a lot of servers
- They probably have a lot in common
- They probably have a few things that are not in common

---

title: Every Server Is Disposable
class: big

- Put those servers in place
- Make sure they're configured the right way
- Use tools to print servers
- Don't cry when they disappear

---

title: Chef
class: big

Things we'll cover:

- Chef-y Concepts
- Single Cookbook Workflow
- Dependency Management
- Vagrant Deployments

---

title: Been Around Since 2008
class: It's been under active development, commercial support (if you care about that)

---

title: What is Chef?
content_class: flexbox

<div style="width: 100%">
  <div class="pillar black gray-bg">framework</div>
  <div class="pillar black yellow2-bg">cli tools</div>
  <div class="pillar black red2-bg">community</div>
  <div class="pillar black green-bg">workflow</div>
</div>

---

title: What is Chef not?

A simple script runner

It's not a remote shell command runner with callbacks

(I'm looking at you Capistrano)


---

title: What is Chef?

The kitchen sink for bringing up and managing servers

Commercially supported by OpsCode


---

title: Who Uses Chef?

Travis CI, Facebook, Indiegogo, Cheezburger, Splunk, many others

---

title: Haterade

> “It’s impressive that Opscode has gotten to this scale,” he says, “but we’ve been at this scale for the last three or four years.”

 Luke Kanies - Puppet Labs founder

<footer class="source">http://www.wired.com/wiredenterprise/2013/02/facebook-chef/</footer>

---

title: Haterade

> It’s just Python. It makes it easier for people like me to contribute (Ruby is not necessarily that mainstream among ops) and also means minimal dependency on install (Python is shipped by default with Linux).

http://devo.ps/blog/2013/07/03/ansible-simply-kicks-ass.html

---

content_class: vcenter flexbox dark nobackground

<span class="red" style="font-size: 60pt">LOL</span>

---

content_class: vcenter flexbox dark nobackground

<span class="red" style="font-size: 60pt">Let's get work done<span>

---

title: Killer Chef

 * cross-platform functionality
 * everything in the right place
 * base functionality is rocket fuel
 * official cookbooks are nice
 * share cookbooks with people
 * versioning
 * dependencies
 * chef's ruby is a good fit

---

title: Chef: the organization

| node        | an individual server                   |
| environment | share configuration between servers    |
| role        | dedicate servers for web, db, whatever |
| clients     | SSL cert for a specific box            |

---

title: Chef: Software Components

| name       | purpose|
| cookbook   | a software library |
| recipe     | a script to run in the Chef environment |
| attributes | customization for cookbooks |
| resource   | a struct describing an object on your server |
| provider   | a subroutine which takes resources as params |
| definition | a combination of resources/providers |
| lwrp       | uhhhhh |
| templates  | erubis config files |
| files      | static files you don't change |

---

title: Whew
content_class: flexbox vcenter

---

title: Chef: Software Components
subtitle: Cookbook

Services: nginx, postgresql, redis, ...

Runtimes: rvm, java, python, ...

Utilities: git, build-essential, ...

Find cookbooks on http://community.opscode.com/ or search across github (that's what I do)

---

title: Chef: Software Components
subtitle: Recipe

What recipes does nginx give you?

| nginx::default         | where chef starts           |
| nginx::passenger       | the rack application loader |
| nginx::repo            |                             |
| nginx::source          |                             |
| *lots of others*       |                             |

---

title: Chef: Software Components
subtitle: Attributes

A robust inheritence scheme. Chef has its own rules for determining attributes at runtime

| default | lowest |
| force_default | beats default |
| normal | saved by the node |
| override | 



---

title: Chef the Framework

It gives you a directory structure

*STRUCTURE GOES HERE*

---

title: How is it a framework?

It introduces 

---

title: Code Example

Media Queries are sweet:

<pre class="prettyprint" data-lang="css">
@media screen and (max-width: 640px) {
  #sidebar { display: none; }
}
</pre>

---

title: Once more, with JavaScript

<pre class="prettyprint" data-lang="ruby">
def is_it_ruby?(*args)
  return true
end
</pre>

---

title: Centered content
content_class: flexbox vcenter

This content should be centered!
