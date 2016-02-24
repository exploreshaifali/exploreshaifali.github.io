---
layout: post
title:  "GSoC15 Application for SugarLabs"
date:   2016-02-24
tags:
  - gsoc
  - git
  - git_internals
  - sugarlabs
---

Hello! I am back with another GSoC 2015 application, for **Sugar Labs[1]**, project named - **Git Backend**. They have pre-defined format of application so basically everything below is answers of questions present in their application format.  

___  

> ###About Me:

* **Name**: Shaifali Agrawal
* **Email address**: agrawalshaifali09@gmail.com
* **Sugar Labs wiki username**: Shaifali_Agrawal
* **IRC nickname**: exploreshaifali
* **First Language**: Hindi(I am also comfortable in English)
* **Where are you located, and what hours (UTC) do you tend to work?**  
I live in India( UTC + 5:30). Though I don’t have any time constraints but mostly I work from afternoon 2PM till night 2AM as per IST.  

* **Previous Experience with Open-Source:**  
I am a big FOSS lover, in 2012 my college senior(GSoC alumni) introduced me to the new world on Computer Science - “Open Source” since than I was looking forward to write open source code. I am grateful to Gnome Foundation that they run [Outreachy][Outreachy] program(previously known as Outreach Program for Women) under which I worked as [OpenStack][OpenStack] intern (Outreachy round 9 from November 2014 to March 2015). Brief explaination (about project)[2] During my internship I have learned about the Zaqar's(messaging and queuing service) storage layer implementation and was able to apply my knowledge and ideas for re-architecture of its drivers(data and control driver of storage layer) with the help of [flaper87][flaper87] (my mentor during internship) and other Zaqar developers. While working for Zaqar I got to learn about many new tools, techniques, concepts and practices used in real world software development. My commits for OpenStack can be glanced here[3].    
For HackerEarth[4] hackathon I along with my partner developed a web app - (NGO Locator)[5] that help users in helping the Needy by finding NGO near to them. We publish this application under open source MIT Licence. The first version of project is completed and now I work for it more only in my free time.

> ###About Project

* **Name** : Git Backend

* **Describe your project in 10-20 sentences. What are you making? Who are you making it for, and why do they need it? What technologies (programming languages, etc.) will you be using?**   

**Brief Explanation:**  
The project is aimed to facilitate versioning, cloning, forking, merging, pull requests from Journal, to the end users. Basically Journal database(sugar-datastore) will be rebuild to be a database based on git, so that the features that are needed to improve(versioning, forking etc) can be achieved easily. All the users who practice/learn development using Sugar(turtle blocks)will get benefit from this. For developing git based backend I have two choices:

1. Using some API that provide similar functionality like github provide [git data] but this will create dependency of database on the API and will force to develop the database in the way API wants. Also we have to bear certain limitations if API have, like the git API limits the support of blobs up to 100 megabytes in size[6], number of requests per hour are limited(60 requests for unauthenticated requests[7]. So it is better to opt option 2.

2. **Developing backend from scratch:**  
Developing git based backend from scratch needs a deep knowledge of git. I am reading [Pro Git Book][Pro Git Book] for that.  
Git itself at a core, is a key-value datastore. The files that we save in git system, it generates a key for that file and return it. So later whenever we want to access the contents of that file we can access them using the returned key.For example in a git repository if we want to insert a file(blob) we use following command:  
`git hash-object -w <filepath> #it will return a sha key`  
"sha" is nothing but a hash of contents of file, that is if we change the content our sha key will change again.
So here we have a problem, each time we change our data our key will also change. This is something we don’t want. To overcome this, git allow us to set reference over a file, such that even if we change contents of file, sha will change but new sha can be referenced with same previously defined reference. Below is a shell script that make user given "key" as a reference of a file given by user.  
```echo Please, enter file path
read filepath  
echo Please, enter the key  
read key  
blob_sha=$(git hash-object -w $filepath)  
git update-index --add --cacheinfo 100644 $blob_sha $key  
tree_sha=$(git write-tree)  
commit_sha=$(git commit-tree -m 'adding 1' $tree_sha)  
git update-ref refs/heads/master $commit_sha  
echo 'All Done'  
# git show master:$key   
```




[1]: https://www.sugarlabs.org/
[2]: https://wiki.openstack.org/wiki/Outreachy/Ideas#Zaqar:_Split_data.2Fcontrol_planes
[3]: https://review.openstack.org/#/q/owner:shaifali,n,z
[4]: http://hackerearth.com/
[5]: http://help-the-needy.herokuapp.com/
[6]: https://developer.github.com/v3/git/blobs/
[7]: https://developer.github.com/v3/#rate-limiting
[Outreachy]: https://wiki.gnome.org/Outreachy/
[OpenStack]: http://openstack.org/
[flaper87]: http://www.flaper87.com/
[git data]: https://developer.github.com/v3/git/
[Pro Git Book]: http://git-scm.com/book/en/v2