f5-ansible-examples
=======

Below are a list of roles that I found useful in controlling a BIG-IP F5.  Many of these examples came from the Ansible docs but I found some tips and tricks that made writing these roles easier.

I'll attempt to break down each one of these items in the roles below.
One important item though, because the network equipment requires everything run locally you need to provide connection information per-task.  I hate this.</br>
Because of this I used the *all* group_vars.  This gave me access to the F5 login and IP data.  So if you can't connect check there and *PLEASE* use *ansible vault* to protect that file when in production.

.</br>
├── ad-hoc</br>
├── ansible.cfg</br>
├── dns-config.yml</br>
├── factory-reset.yml</br>
├── group_vars</br>
│   └── all</br>
├── inventory</br>
├── maint-night.yml</br>
├── node.yml</br>
├── pools.yml</br>
├── README.md</br>
├── roles</br>
│   ├── dns</br>
│   │   └── tasks</br>
│   │       └── main.yml</br>
│   ├── example_com</br>
│   │   └── tasks</br>
│   │       └── main.yml</br>
│   ├── example_com_maint</br>
│   │   └── tasks</br>
│   │       └── main.yml</br>
│   ├── example_standard</br>
│   │   └── tasks</br>
│   │       └── main.yml</br>
│   ├── pools</br>
│   │   └── tasks</br>
│   │       └── main.yml</br>
│   └── www_qwerty_io</br>
│       └── tasks</br>
│           └── main.yml</br>
├── save-config.yml</br>
└── web-tier.yml</br>


Roles
=================
   * dns
      * This DNS Role is as simple as it gets.  It sets the DNS for the F5, there's nothing tricky here but it's a good starting point.
   * example_com
      * This *example_com* role is an all in one that I feel is most commonly used.  Create the pool, list each node and toss them in.  This 100% works however with some loops and inventory you can save yourself a decent amount of work.  That's shown in the *www_qwerty_io* role.
   * example_com_maint
      * The *example_com_maint* role I thought was rather cool as it's a simple example that uses a tag and based on that tag pulls nodes out of the pool.  Expanding on this with your patching or the like means no more late night patching sessions.  Set this up and cycle through your systems for mid-day patching and sleep all night.
   * example_standard
      * *example_standard* is that role that should run every hour and first thing when you plug in the F5, setup DNS, NTP and the like.  If the F5 drifts then in an hour allow it to auto-heal.  Using this with some scheduling on *AWX* or *Ansible Tower* could be fantastic.
   * pools
      * *pools* is just an early example of listing all of the resources needed on a F5.  Don't forget that you can also use your inventory to create this list and loop through them dynamically ( see *www_qwerty_io* )
   * www_qwerty_io
      * *www_qwerty_io* was my banging the head on the desk until I got it right.  I didn't want to have to edit every role when I added servers. When you start talking about possible containers being nodes this makes even less sense.  So I found all of my required items to only rely on my inventory file.  If you need 12 more servers in *www_qwerty_io* you add them to it's inventory *loop_group* ( Yea I know... but hey it was 2 AM ).  By adding this and running via *AWX* or *Ansible Tower* you no longer need to configure the F5 by hand.


Requirements
=======
* Ansible 2.5
* pip install bigsuds

Authors
=======
I used at least 1/2 of the Ansible pages provided by Google so if you see something and say "Hey I wrote that" please let me know, I assure you had I know I would have sourced it properly.
The goal here is to give you a few tools so that you can start managing your networks.
Thanks everyone
