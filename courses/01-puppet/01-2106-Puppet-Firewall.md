# Puppet Firewall

###Slide 1
Firewall rule management is a critical task that you can automate with Puppet Enterprise.


###Slide 2
In this course, we look at how to:

*write a simple firewall module.
*use the Puppet Enterprise console to add a class named "my firewall" to your agent nodes.
*write a class to open ports for the Puppet master.
*enforce the desired state of the "my firewall" class.


###Slide 3
Before we get deep into the ""my firewall" module, let's take care of some housekeeping.

If you haven’t already done so, you need to install Puppet Enterprise and NTP. See the system requirements for supported platforms.

Puppet NTP insures that your nodes are synchronous. You can find a link to the NTP Quick Start Guide in the resources section of this lesson.

###Slide 4
It's also important to know is that, by default, the modules you use to manage nodes are located in /etc/puppetlabs/puppet/modules— this includes modules installed by Puppet Enterprise, those that you download from the Forge, and those you write yourself.

Puppet Enterprise also installs modules in /opt/puppet/share/puppet/modules. It's important that you remember that you don't modify anything in this directory or add modules of your own to it.

There are plenty of resources about modules and the creation of modules that you can reference. Check out Modules and Manifests, the Beginner’s Guide to Modules, and the Puppet Forge.


###Slide 5
System administrators define a set of firewall rules that usually manage functions such as application ports, node interfaces, IP addresses and an accept/deny statement. These rules are applied in a “top-to-bottom” approach. For example, if a node has access to a port, and the node belongs to a group that does not have access to that port, the node will inherit the group restriction. 


###Slide 6
Modules are directory trees. The example module used in this course is called "my firewall", and it looks like this:

###Slide 7
The init.pp manifest looks like this:

You can see that it contains the pre and post class declarations.


###Slide 8
The pre.pp manifest looks like this:

Since firewall rules are run top to bottom, the pre rules are the most important. They are applied before any other rules.

The resources evoked in this manifest are:
proto
action
iniface
and port


###Slide 9
The post.pp manifest looks like this:
	
After the pre and service rules have been applied, the post rule is used to drop any packets that did not match a rule that's enforced by pre or the service rule. 


###Slide 10
The Puppet master is just like any other service or application you run from your infrastructure. You need to open ports to ensure you can access the Puppet master correctly.

To do this, you will create another module. We'll name this module "my master".

The init.pp that you create for the "my master" module looks like this:

Since this course deals specifically with firewalls, we are only displaying the firewall attributes of this class.


###Slide 11
The my_master and my_firewall classes both contribute to a composite model that the puppet master enforces as a desired state. In this composite model, you can see the firewall definitions of the pre, my_master, and post manifests in the order that Puppet will apply them.

Now imagine a scenario where a group member changes a local iptable to open a port that is otherwise blocked by this desired state.

Since this firewall policy is already defined and applied to the group, all that's required is to let the scheduled Puppet run begin. Otherwise, if you want to apply the change immediately, go to the Live Management tab in Puppet Enterprise console, click run once, and you're finished.


###Slide 12
In this course, you have seen key concepts to install and maintain the Firewall module

We hope that this brief introduction to Firewalls demonstrates how easy it is to implement firewall rules using Puppet.


###Slide 13
To check your knowledge, click the link on the bottom of this course's page and complete the short quiz. There is also a link to additional learning resources.


## Video ##

## Exercises ##
There are no exercises for this course.

## Quiz ##

1. The firewall prioritizes rules in which order?
	Pre, post
	Service rules, pre, post
	**Pre, service rules, post**
2. Local firewall rules may override group policy under certain conditions. t/**f**
3. The purpose of the post rule is:
	Close any ports that are left open by higher level rules.
	***Drop all packets not explicitly allowed by higher level rules.***
	Open ports for special cases that are closed by higher level rules.
4. The Puppet Master is exempt from firewall rules. t/***f***
5. The firewall policy is comprised of rules included in any of the following classes, if they are present. Select all that apply.
	***Pre***
	***Post***
	My_firewall
	***Any Service Rule***
## References ##
* [Beginner’s Guide to Modules](https://docs.puppetlabs.com/pe/latest/guides/module_guides/bgtm.html)
* [Firewall Quick Start Guide] (https://docs.puppetlabs.com/pe/latest/quick_start_firewall.html)
* [The Puppet Forge](https://forge.puppetlabs.com/)
* [Event inspector docs](https://docs.puppetlabs.com/pe/latest/console_event_inspector)
