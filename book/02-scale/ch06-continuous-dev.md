#### CICD: Because updates can be pain

So far we've disucssed some tools you can use to get started like Terraform for Droplet provisioning, Ansible for configuraiton management, and Git as your SCM. With those tools we've created a pretty simple WordPress blog with the application being load balanced as well as your database cluster, and we're using DigitalOcean Spaces to store your files. So far life is good and everything is running smoothly. But what are we going to do when there is a change to the application with the modification of your theme or the addition of a plugin for functionality. 

You'll generally start in your development environment, then once you're satisfied with the changes you'll push them up. Now you'll modify any files that specify version numbers to use in one of your roles, run your playbook in your staging environment, then begin testing out the changes. Once that's done and you're satisfied nothing is broken you can go ahead and push the changes out to production. The length of the process really depends on what you're modifying but once you application begins to grow and you start breaking things down even further, you may notice that it becomes a very tedious, time consuming, and error prone practice. Now if everything was done properly, maybe nothing bad happens. But maybe something does happen, and it will at some point, and now you have a bug that you need to work towards fixing very quickly because no one wants to see error pages when they're visiting your site. 

We can usually get away from this type of scenario by setting up some type of CICD pipeline. This will allow you to automate steps from the moment you push your changes up to your repo, all the way into the deployment of your application into your production environment. You should be able to catch bugs early on in the process, ensure consistency across deployments, and make your life easier by automating much of the delpoyment process. 

**Setup**


With this change in deployment style we need to adjust the way we store our terraform state since Jenkins needs to have access to it in order to deploy into staging and production.

We;re going to need a Droplet with Jenkins installed. We're also going to make use of the pipeline plugin. 
