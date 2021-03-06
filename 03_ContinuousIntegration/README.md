####Session 3: Continuous Integration with Jenkins

The aim of this session is to set up a Jenkins Continuous Integration Server & Agent that monitors a code repository and automatically runs tests everytime a developer checks in code.

This workshop follows on from last week's session about provisioning environments (using chef & vagrant to manage virtualbox VMs). We'll use some of what you learned there to build the CI server and agent VMs.

Contents:
- What is Continuous Integration (CI) and why is it needed?
- Setting up a CI server (Jenkins)
- Installing plugins
- Configuring the CI Server to monitor a code repo and run tests automatically on every checkin.
- Setting up a Distributed CI environment
- What is Continuous Delivery (CD) and why is it needed?
- Demo using a build pipeline to deploy to a Test Environment (to Heroku?).


####Reading Material

######Martin Fowler's article on Continous Integration:

http://www.martinfowler.com/articles/continuousIntegration.html

######Wikipedia:

http://en.wikipedia.org/wiki/Continuous_integration

######Internal reference:

http://blog.howareyou.com/post/62157486858/continuous-delivery-with-docker-and-jenkins-part-i


####Pre-requisites

#####1. Install the below software. Google them to find installers.

| Tool/Software | Notes |
| ------------- | ----- |
| [Ruby](https://www.ruby-lang.org/en/) | version 2.1.2 preferred |
| [Virtualbox](https://www.virtualbox.org/) | This tool runs the VMs that we'll use for the workshop |
| [Vagrant](https://www.vagrantup.com/) | This tool manages the VMs - download and install them, start them up, shut them down etc. You'll need version 1.6.3 or later. <br><b>NOTE: Windows users, please install vagrant into a folder that does not have spaces in the name, eg 'c:\vagrant' </b> |
| [chefdk](https://downloads.getchef.com/chef-dk) | This installs 'berkshelf', a tool that fetches required chef cookbooks for packages that need to be installed <br> Once installed, run the command `which berks` (on osx or linux). The output should look like `/usr/bin/berk` which should be a link to `/opt/chefdk/bin` (You can check that by running `ls -l /usr/bin/berk`). If this is set incorrect, please tweak your PATH so that your system picks up `berk` from the right location.|
| [vagrant berkshelf plugin](http://berkshelf.com/) | Install it from the commandline by running `vagrant plugin install vagrant-berkshelf` |
| [Git](http://git-scm.com/) | This is the source control tool that we'll use for the workshop |

NOTE: If you attended last week's session on Provisioning Environments, you'd already have all these installed.

#####2. Create an account on [github.com](https://github.com/) if you dont already have one.

#####3. Clone the repository to your local machine

If you dont already have the bootcamp code repo in your local machine, then get it by running in a command prompt or Terminal:
`git clone https://github.com/SydneyTestersBootcamp/sydneyTestersBootcamp --depth 1`
This could take a while. Once done, go into the session folder in your Command Prompt/Termins `cd sydneyTestersBootcamp/03_ContinuousIntegration`.

If you already have it (cloned or forked) in your local machine, then do a `git pull` to fetch the latest versions.

#####4. Set up the CI Server VM that we'll use for the workshop

Go into the folder that has setting for the CI Server VM:
`cd CI_Server`

and run:

`vagrant up`

The first time you run this command, it downloads a Vritualbox VM ~500 MB in size (Centos Linux), installs a few packages onto it, and starts it up. This may take a long time (upto 60 mins, depending on your internet connection speed), so <b>please do this before coming for the session</b>. You may want to do this on a wifi connection due to the large data download.

You will see a flurry of debug messages on your terminal. Ignore any warnings that look like this: "warning: class variable access from toplevel". At the end, you'll see a message "INFO: Chef Run complete in xxx seconds"

Once the above is done, open up a browser and navigate to [http://localhost:9080](http://localhost:9080). You should be able to see the Jenkins admin page.

Now run the command `vagrant suspend` to suspend the VM.

#####5. Set up the CI Agent VM that we'll use for the workshop

Go into the folder that has setting for the CI Agent VM:

`cd ../CI_Agent`

and then:

`vagrant up`

This may take a long time, so <b>please do this before coming for the session</b>. You may want to do this on a wifi connection due to the large data download.

Once the above is done, run `vagrant ssh` to ssh into the VM. Run the command `java -version` and ensure it reports 1.6.xxx.

Now run the command `exit` to exit out of the ssh session. Once back in your local machine command prompt, run `vagrant suspend` to suspend the VM.

####Common issues
- RuntimeError: Couldn't determine Berks version<br>
You would need to add /opt/chefdk/bin at the front of your PATH