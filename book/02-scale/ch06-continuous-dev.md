#### CICD: Because updates can be pain

recap
So far we've disucssed some tools you can use to get started like Terraform for Droplet provisioning, Ansible for configuraiton management, and Git as your SCM. With those tools we've provisioned some Droplets and deployed a WordPress blog with the application being load balanced as well as your database cluster, and we're using DigitalOcean Spaces to store your files. So far life is good and everything is running smoothly. But things do change. I'm not talking about catastrophic failures right now, I referring to changes you make. You could end up not liking the way something looks and decide it's time to try a new theme. or maybe you want to update, or create a new plugin. On the more serious side of things it could be time to update the WordPress core because there's a serious security vulnerability that was just patched. So what does your process or workflow look like for making these types of changes.

Normally you would start in your development environment, then once you're satisfied with the changes you'll push them up to you remote repo. Now you'll modify any files that specify version numbers to use in one of your roles, run your playbook in your staging environment, then begin testing out the changes. Once that's done and you're satisfied nothing is broken you can go ahead and push the changes out to production. The length of the process really depends on what you're modifying but once you application begins to grow and you start breaking things down even further, you may notice that it becomes a very tedious, time consuming, and error prone practice. Now if everything was done properly, maybe nothing bad happens. But maybe something does happen, and it will at some point, and now you have a bug that you need to work towards fixing very quickly because no one wants to see error pages when they're visiting your site. 

You'll want to catch errors as early in the process as possible by setting up some automation for testing and deployment. There are quite a few different choices you can go with, but some popular choices include Jenkins, CirleCI, TravisCI, GitLab CI, GoCD, and Concourse. The list is quite extensive with features varying throughout the bunch. If you have some time I recommend checking out the following DigitalOcean community article which gives a thorough breakdown of the different types of CI/CD tools previously mentioned. 

https://www.digitalocean.com/community/tutorials/ci-cd-tools-comparison-jenkins-gitlab-ci-buildbot-drone-and-concourse

We're going to use Jenkins for our example as it has a large plugin catalog allowing users to automate basically any task under the sun, however, regardless of what you choose you should be able to set up some automated testing at minumum. This will allow you to catch any changes that could cause an issue before it gets to production.

Another thing to note is we're making a change that we mentioned in the last chapter. Previously we were using Terraform and storing its state to your local file system. That was fine since we were just launching a small cluster and working solo, but now that we're tossing Jenkins into the mix it's going to need some information and the best way to provide that is by using a backend for Terraform's remote state. So we're going to set up a Consul server to store all the info about your environments. 

**Setup**



With this change in deployment style we need to adjust the way we store our terraform state since Jenkins needs to have access to it in order to deploy into staging and production. 

We're going to need a Droplet with Jenkins installed. We're also going to make use of the pipeline plugin to build, test, and store our artifacts.


