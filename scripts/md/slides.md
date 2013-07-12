
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
- Don't cry when they disappear
- Print servers

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
content_class: vcenter flexbox dark nobackground


> “It’s impressive that Opscode has gotten to this scale,” he says, “but we’ve been at this scale for the last three or four years.”

> Luke Kanies - Puppet Labs founder

<footer class="source">
  http://www.wired.com/wiredenterprise/2013/02/facebook-chef/
</footer>

---

title: Haterade
content_class: vcenter flexbox dark nobackground

> “It’s just Python. It makes it easier for people like me to contribute (<em>Ruby is not necessarily that mainstream among ops</em>) and also means minimal dependency on install (<em>Python is shipped by default with Linux</em>).“

> Vincent Viallet - devo.ps



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
 * bootstraps itself (omnibus)
 * ruby is a good fit

---

title: How You'll Break Down Your Environment

<table>
  <tr>
    <td>node</td>
    <td>an individual server</td>
  </tr>
  <tr>
    <td>role</td>
    <td>dedicate servers for web, db, whatever</td>
  </tr>
  <tr>
    <td>environment</td>
    <td>usually locks cookbook verions</td>
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

Utilities: *git*, *application*, *build-essential*, ...

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
    <td>if you want to build it from source</td>
  </tr>
</table>

---

title: Chef: Software Components
subtitle: Cookbooks: What attributes does <em>nginx</em> use? (I)

<pre class="prettyprint" data-lang="ruby">
# nginx/attributes/default.rb

default['nginx']['version'] = "1.2.6"
default['nginx']['package_name'] = "nginx"
default['nginx']['dir'] = "/etc/nginx"
default['nginx']['log_dir'] = "/var/log/nginx"
default['nginx']['binary'] = "/usr/sbin/nginx"
</pre>

---

title: Chef: Software Components
subtitle: Cookbooks: What attributes does <em>nginx</em> use? (II)

<pre class="prettyprint" data-lang="ruby">
# nginx/attributes/default.rb

case node['platform']
when "debian","ubuntu"
  default['nginx']['user']       = "www-data"
  default['nginx']['init_style'] = "runit"
when "redhat","centos","scientific","amazon","oracle","fedora"
  default['nginx']['user']       = "nginx"
  default['nginx']['init_style'] = "init"
  default['nginx']['repo_source'] = "epel"
else
  default['nginx']['user']       = "www-data"
  default['nginx']['init_style'] = "init"
end
</pre>

---

title: Chef: Software Components
subtitle: Cookbooks: What attributes does <em>nginx</em> use? (II)

<pre class="prettyprint" data-lang="ruby">
# nginx/attributes/default.rb

default['nginx']['keepalive']          = "on"
default['nginx']['keepalive_timeout']  = 65
default['nginx']['worker_processes']   = node['cpu'] && node['cpu']['total'] ? \
      node['cpu']['total'] : 1
default['nginx']['worker_connections'] = 1024
default['nginx']['worker_rlimit_nofile'] = nil
default['nginx']['multi_accept']       = false
default['nginx']['event']              = nil
default['nginx']['server_names_hash_bucket_size'] = 64
default['nginx']['sendfile'] = 'on'
</pre>

---

title: Chef: Software Components


Rules for determining attributes at runtime

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

title: Standard Lib
subtitle: Consistent Cross-Platform Resources

<table>
  <tr>
    <th>Resource Name</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td>directory</td>
    <td>Create directories and manage ownership/permissions</td>
  </tr>
  <tr>
    <td>remote_file</td>
    <td>Fetch a file using http</td>
  </tr>
  <tr>
    <td>template</td>
    <td>evaluate an erubis template and put it somewhere</td>
  </tr>
  <tr>
    <td>link</td>
    <td>Symlink files, pretty strict about it too</td>
  </tr>
  <tr>
    <td>package</td>
    <td>Use native tools to install a precompiled package</td>
  </tr>
</table>

<footer class="source">http://docs.opscode.com/resource.html#chef-resources</footer>

---

- apt_package
- bash
- breakpoint
- chef_gem
- cookbook_file
- cron
- csh
- deploy
- directory
- dpkg_package
- easy_install_package
- env
- erl_call
- execute

---

- file
- freebsd_package
- gem_package
- git
- group
- http_request
- ifconfig
- ips_package
- link
- log
- macports_package
- mdadm
- mount
- ohai
- package

---

- pacman_package
- perl
- portage_package
- powershell_script
- python
- registry_key
- remote_directory
- remote_file
- route
- rpm_package
- ruby
- ruby_block
- scm
- script

---

- service
- smartos_package
- solaris_package
- subversion
- template
- user
- yum_package

---

title: Code Example
subtitle: How to create a directory

<pre class="prettyprint" data-lang="ruby">
directory "/put/a/dir/here" do
  owner "xavier"
  group "rubyist"
  mode 00777

  recursive true
  action :create
end
</pre>

Works on OS X, Linux, Windows, Solaris, etc.

---

title: Code Example
subtitle: How to remove a directory

<pre class="prettyprint" data-lang="ruby">
directory "/dont/want/it" do
  action :delete
end
</pre>

---

title: Code Example
subtitle: How to download a remote file

<pre class="prettyprint" data-lang="ruby">
remote_file  "/usr/local/src" do
  source   'ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p448.tar.gz'
  checksum 'a893cff26bcf351b8975ebf2a63b1023'
end
</pre>

Does the right thing if the local file exists

---

title: Full-Blown Example

 - Berkshelf
 - Ruby on Rails 4.0 app
 - git
 - unicorn
 - runit
 - PostgreSQL
 - nginx
 - vagrant local deploy

---

title: Berkshelf
subtitle: Bundler for Chef Cookbooks

<p style="text-align: center">
  <img src="/images/gersberms.jpg" height="520"/>
</p>

---

title: Initializing the Project

<pre class="prettyprint" data-lang="bash">
gem install berkshelf --no-rdoc --no-ri

berks cookbook sdruby-prism
</pre>

You can follow along

  - https://github.com/xrl/sdruby-chef
  - https://github.com/xrl/sdruby-app

---

title: Folder structure

<pre class="prettyprint" data-lang="bash">
xavierlange ~/code/sdruby-prism$ tree . -I vendor
.
├── Berksfile
├── Berksfile.lock
├── Gemfile
├── Gemfile.lock
├── LICENSE
├── README
├── README.md
├── Thorfile
├── Vagrantfile
├── attributes
│   └── default.rb
├── chefignore
├── definitions
├── files
│   └── default
│       └── ruby
│           └── gemrc
</pre>

---

title: Folder structure

<pre class="prettyprint" data-lang="bash">
xavierlange ~/code/sdruby-prism$ tree . -I vendor
.
├── libraries
├── metadata.rb
├── providers
├── recipes
│   ├── database.rb
│   ├── default.rb
│   ├── deploy.rb
│   └── ruby.rb
├── resources
└── templates
    └── default
</pre>

---

title: metadata.rb

<pre class="prettyprint" data-lang="ruby">
name             'sdruby-prism'
maintainer       'Xavier Lange'
maintainer_email 'xrlange@gmail.com'
license          'All rights reserved'
description      'Installs/Configures sdruby-prism'
long_description IO.read(File.join(File.dirname(__FILE__), 'README.md'))
version          '0.1.0'

depends "git"
depends "application"
depends "application_nginx"
depends "application_ruby"
depends "postgresql"
depends "database"
</pre>

---

title: Berksfile

<pre class="prettyprint" data-lang="ruby">
site :opscode

metadata

cookbook "application_nginx", path: '../application_nginx'
</pre>

---

title: recipes/default.rb

<pre class="prettyprint" data-lang="ruby">
#
# Cookbook Name:: sdruby-prism
# Recipe:: default
#
# Copyright (C) 2013 Xavier Lange
# 
# All rights reserved - Do Not Redistribute
#

include_recipe "sdruby-prism::ruby"

include_recipe "sdruby-prism::database"
include_recipe "sdruby-prism::deploy"
</pre>

---

title: recipes/ruby.rb

<pre class="prettyprint" data-lang="ruby">
apt_repository "brightbox-ruby-ng-experimental" do
  action       :add
  uri          "http://ppa.launchpad.net/brightbox/ruby-ng-experimental/ubuntu"
  distribution node['lsb']['codename']
  components   ["main"]
  keyserver    "keyserver.ubuntu.com"
  key          "C3173AA6"
end

apt_package "ruby1.9.1"
apt_package "ruby1.9.1-dev"
</pre>

---

title: attributes/default.rb

<pre class="prettyprint" data-lang="ruby">
force_default[:build_essential][:compiletime]     = true

force_default[:postgresql][:password][:postgres]  = 'supersecret'

force_default[:prism][:database][:name]     = 'sdruby-prism'
force_default[:prism][:database][:username] = 'sdruby'
force_default[:prism][:database][:password] = 'chef'
</pre>

---

title: recipes/database.rb

<pre class="prettyprint" data-lang="ruby">
include_recipe "postgresql"
include_recipe "postgresql::server"
include_recipe "postgresql::ruby"
include_recipe "database"

conn = {:host => "127.0.0.1",
        :port => 5432,
        :username => 'postgres',
        :password => node['postgresql']['password']['postgres']}

postgresql_database_user node[:prism][:database][:username] do
  connection conn
  password node[:prism][:database][:password]
  action :create
end

postgresql_database node[:prism][:database][:name] do
  connection conn
  owner node[:prism][:database][:username]
  action :create
end
</pre>

---

title: recipes/deploy.rb

<pre class="prettyprint" data-lang="ruby">
include_recipe "application"
include_recipe "git"
include_recipe "runit"

application "prism" do
  path "/home/ubuntu/app"
  repository "https://github.com/xrl/sdruby-app.git"

  owner "ubuntu"
  group "ubuntu"

  # snip

  nginx_load_balancer do
    application_port 3000
    static_files ({"/assets" => "public/assets"})
    hosts [node]
  end

end
</pre>

---

title: Some Ruby-Specific Declarations

<pre class="prettyprint" data-lang="ruby">
  rails do
    db_name = node[:prism][:database][:name]
    db_user = node[:prism][:database][:username]
    db_pwd  = node[:prism][:database][:password]

    database do
      adapter  'postgresql'
      host     'localhost'
      database db_name
      username db_user
      password db_pwd
    end

  end

  unicorn do
    port "3000"
  end
</pre>

---

title: Now Bring It To Life (locally)

<pre class="prettyprint" data-lang="bash">
  cd ~/code/sdruby-prism
  vagrant up

  open http://33.33.33.10/
</pre>

---

title: Chef Flavors

You have to choose between

 - Chef-Solo
 - Hosted Chef
 - Opscode Chef

---

title: Deploying: Knife
subtitle: Perform Chef Operations on Remote Servers

Chef comes with Knife to help you

 - Upload cookbooks
 - Bootstrap servers
 - Interact with cloud services

---

title: Cloudtacular Deployment
subtitle: AWS/EC2

<pre class="prettyprint" data-lang="bash">
  cd ~/code/sdruby-prism
  gem install knife-ec2

  knife ec2 flavor list -A $AWS_ACCESS_KEY
  ID           Name          Architecture RAM     Disk     Cores
  c1.medium    High-CPU Med  32-bit       1740.8  350 GB   5
  c1.xlarge    High-CPU Ext  64-bit       7168    1690 GB  20
  cc1.4xlarge  Cluster Comp  64-bit       23552   1690 GB  33.5
  cc2.8xlarge  Cluster Comp  64-bit       61952   3370 GB  88
  cg1.4xlarge  Cluster GPU   64-bit       22528   1690 GB  33.5
  hi1.4xlarge  High I/O Qua  64-bit       61952   2048 GB  35
  m1.large     Large Instan  64-bit       7680    850 GB   4
  m1.medium    Medium Insta  32-bit       3750    400 GB   2
  m1.small     Small Instan  32-bit       1740.8  160 GB   1
  m1.xlarge    Extra Large   64-bit       15360   1690 GB  8
  m2.2xlarge   High Memory   64-bit       35020.8 850 GB   13
  m2.4xlarge   High Memory   64-bit       70041.6 1690 GB  26
  m2.xlarge    High-Memory   64-bit       17510.4 420 GB   6.5
  t1.micro     Micro Instan  0-bit        613     0 GB     2
</pre>

---

title: Cloudtacular Deployment
subtitle: Prepare Hosted Chef

<pre class="prettyprint" data-lang="bash">
  # make sure the environment exists on the chef server
  knife environment from file roles/production.json

  # put main cookbook (and dependencies) on chef server
  berks upload sdruby-prism

  # lock versions on the chef server
  berks apply production
</pre>

---

title: Cloudtacular Deployment
subtitle: Find an AWS image

 - http://cloud-images.ubuntu.com/
 - choose an EC2 region/availability zone
 - choose a distribution
 - choose an architecture
 - choose ebs-enabled yay/nay
 - create a security group
 - create an ssh key

---

title: Cloudtacular Deployment
subtitle: Create EC2 instance

<pre class="prettyprint" data-lang="bash">
bundle exec knife ec2 server create \
  --node-name=prism                        \
  --environment=production                 \
  --flavor=m1.large                        \
  --image=ami-72072e37                     \
  --identity-file=~/.ssh/sdruby-key        \
  --groups sdruby-prism                    \
  --ssh-key=sdruby-prism                   \
  --ssh-user=ubuntu                        \
  --json-attributes-file nodes/prism.json  \
  --run-list=sdruby-prism
</pre>

---

title: Cloudtacular Deployment
subtitle: Node Seed File

<pre class="prettyprint" data-lang="bash">
{
  "name": "prism",
  "chef_environment": "production",
  "normal": {
  },
  "run_list": ["recipe[sdruby-prism]"]
}
</pre>

---

title: Cloudtacular Deployment
subtitle: Make server accessible

<pre class="prettyprint" data-lang="bash">
include_recipe "dnsimple"

dnsimple_record "sdruby-prism-dns" do
    name "www"
    content node[:cloud][:public_ipv4]
    type "A"
    domain   "sdruby-prism.org"
    username node[:dnsimple][:username]
    password node[:dnsimple][:password]

    action :create
  end
</pre>