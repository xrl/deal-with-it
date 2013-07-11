
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

title: Chef-y Concepts
class: big

Things we'll cover

- Single Cookbook Workflow
- Dependency Management
- Vagrant Deployments

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

It's not a *simple* script runner

It's not a remote shell command runner with callbacks

(I'm looking at you Capistrano)

---

title: Who Uses Chef?

- Travis CI
- Facebook
- Indiegogo
- Cheezburger
- Splunk

many others

---

title: Haterade

> “It’s impressive that Opscode has gotten to this scale,” he says, “but we’ve been at this scale for the last three or four years.”

 Luke Kanies - Puppet Labs founder

<footer class="source">
  http://www.wired.com/wiredenterprise/2013/02/facebook-chef/
</footer>

---

title: Haterade

> It’s just Python. It makes it easier for people like me to contribute (Ruby is not necessarily that mainstream among ops) and also means minimal dependency on install (Python is shipped by default with Linux).


<footer class="source">
  http://devo.ps/blog/2013/07/03/ansible-simply-kicks-ass.html
</footer>

---

content_class: vcenter flexbox dark nobackground

<span class="red" style="font-size: 60pt">LOL</span>

---

content_class: vcenter flexbox dark nobackground

<span class="blue" style="font-size: 60pt">Let's get work done<span>

---

title: Killer Chef

 * cross-platform functionality
 * everything in the right place
 * base functionality is rocket fuel
 * official cookbooks are nice
 * share cookbooks with people
 * versioning
 * dependencies
 * ruby is a good fit

---

title: How You'll Break Down Your Environment

<table>
  <tr>
    <td>node</td>
    <td>an individual server</td>
  </tr>
  <tr>
    <td>environment</td>
    <td>usually locks cookbook verions</td>
  </tr>
  <tr>
    <td>role</td>
    <td>dedicate servers for web, db, whatever</td>
  </tr>
  <tr>
    <td>clients</td>
    <td>SSL cert for a specific box</td>
  </tr>
</table>

---

title: Chef: Software Components

<table>
  <tr>
    <td>cookbook</td>
    <td>a software library</td>
  </tr>
  <tr>
    <td>recipe</td>
    <td>a script to run in the Chef environment</td>
  </tr>
  <tr>
    <td>attributes</td>
    <td>customization for cookbooks</td>
  </tr>
  <tr>
    <td>resource</td>
    <td>a struct describing an object on your server</td>
  </tr>
  <tr>
    <td>provider</td>
    <td>a subroutine which takes resources as params</td>
  </tr>
  <tr>
    <td>definition</td>
    <td>a combination of resources/providers</td>
  </tr>
  <tr>
    <td>templates</td>
    <td>erubis config files</td>
  </tr>
  <tr>
    <td>files</td>
    <td>static files you don't change</td>
  </tr>
</table>

---

title: Chef: Software Components
subtitle: Cookbooks: Example

Services: *nginx*, *postgresql*, *redis*, ...

Runtimes: *rvm*, *java*, *python*, ...

Utilities: *git*, *build-essential*, ...

more on the [opscode community site](http://community.opscode.com/)

or search across [github](https://www.github.com) (that's what I do)

---

title: Chef: Software Components
subtitle: Cookbooks: What does <em>nginx</em> give you?

<table>
  <tr>
    <td>nginx::default</td>
    <td>where chef starts</td>
  </tr>
  <tr>
    <td>nginx::passenger</td>
    <td>the rack application server</td>
  </tr>
  <tr>
    <td>nginx::repo</td>
    <td>if you want it fast</td>
  </tr>
  <tr>
    <td>nginx::source</td>
    <td>if you want to warm up a CPU</td>
  </tr>
</table>

---

content_class: vcenter flexbox

<span class="red" style="font-size: 60pt">Whew</span>

---

title: Chef: Software Components
subtitle: Attributes

A robust inheritence scheme. Chef has its own rules for determining attributes at runtime

<table>
  <tr>
    <td>default</td>
    <td>lowest, what you usually see in attribute files</td>
  </tr>
  <tr>
    <td>force_default</td>
    <td>useful when wrapping cookbooks</td>
  </tr>
  <tr>
    <td>normal</td>
    <td>the node's config</td>
  </tr>
  <tr>
    <td>override</td>
    <td>used sparingly in recipe files</td>
  </tr>
  <tr>
    <td>force_override</td>
    <td>used to override attributes from role or env</td>
  </tr>
  <tr>
    <td>automatic</td>
    <td>gathered by ohai, read-only</td>
  </tr>
</table>

---

title: Framework Conventions

- Common Folder Structure
- Big Standard Library
- Configuration in Attributes
- Actions Invoked in Recipes

---

title: Standard Lib
subtitle: Consistent Cross-Platform Resources

<table>
  <tr>
    <th>Resource Name</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td>directory</td>
    <td>template</td>
    <td>symlink</td>
  </tr>
</table>

<footer class="source">http://docs.opscode.com/resource.html#chef-resources</footer>

---

content_class: notitle

<pre class="prettyprint" data-lang="bash">
xavierlange ~/code/sdruby-prism$ tree .
.
├── Berksfile
├── Gemfile
├── LICENSE
├── README.md
├── Thorfile
├── Vagrantfile
├── attributes
├── chefignore
├── definitions
├── files
│   └── default
├── libraries
├── metadata.rb
├── providers
├── recipes
│   └── default.rb
├── resources
└── templates
    └── default
</pre>

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
