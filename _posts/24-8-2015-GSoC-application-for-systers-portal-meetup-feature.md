---
layout: post
title:  "GSoC Application for Systers Portal Meetup Feature"
date:   2015-08-24
tags:
  - openstack
  - outreachy
  - opw
  - event
---

Holla! Here I am sharing my GSoC 2015 application for **Systers, an Anita Borg Institute Community**, project named - **Systers Portal Meetup Feature**. They have pre-defined format of application so basically everything below is answers of questions present in their application format.  

---

> ###Profile links:  
 
[Website][Website]  
[Github][Github]  
[Twitter][Twitter]  
[Linkedin][Linkedin]  
[Google+][Google+]  
[Launchpad][Launchpad]
  
> Are you a Syster [(http://www.systers.org/)](www.systers.org)? Would you join if you are accepted? (Note: Systers is only open to women in computing; if you are male, you may not join, though you are welcome to join systers-dev, our development list.)

Yes. My join request for Systers is accepted!
Being a tech enthusiast I always take initiative to meet and interact with different people in same field. And Systers being a women community provide more comfort and emotional support too along with tech knowledge!

> How can we reach you (email, IRC, etc.) if we have questions about your application?

To reach me any of the following means can be used(listed priority wise):  
Email address: agrawalshaifali09[AT SPAMFREE]gmail[DOT]com  
IRC nick: exploreshaifali  

>What is your github and/or launchPad username(s):

Github username: [exploreshaifali][Github]  
Launchad ID: [agrawalshaifali09][Launchpad]

___

**Project Specific Questions**

> Which Systers GSoC project are you applying for (please submit a separate application for each project):
[
Systers GSoC project I am applying for - [Meetup Features][4]

> What do you plan to accomplish over this summer for this project? (Please tell us what project you want to work on, how you will approach that project portion, and what your milestones are. You may want to ask for help from the systers-dev@systers.org list if you have not created project milestones before, or if you are unsure what is realistic to accomplish. GSOC divides the summer in half, and at the midpoint, you are paid if you are reasonably close to the milestones you proposed to reach by then, and again for meeting your milestones at the end of the summer.

> If your project proposal is accepted, you will have a chance to work with your mentors to revise these milestones, and we always take into account “unforeseen circumstances”, such as discovering that the code you were going to build on top of is incompatible with mailman. However, being able to realistically estimate how much you will be able to accomplish is an important part of this proposal.)

###Abstract
Systers used Meetup Everywhere for their global community. However, Everywhere was discontinued in December 2014.

This project is aimed to develop a web based meetup management application to provide facilities similar to Meetup Everywhere that serve Systers Global Community.

###Motivation (Project impact on Systers)
We have Systers community portal that help in reducing the communication gap between groups by allowing different groups to share information and get latest news.

A Meetup management application will be helpful not only for meetup organizers in organizing, managing and controlling meetups but also for members to discuss about coming meetups and participate in meetups smoothly as much as possible.

###Goals
The main goal of this proposal is to provide Systers Global Community a web based solution to manage and control all the global and local meetups that they organize.

We have four different stakeholders - Administrators,Organizers, Members and Guest. Each one of them have different roles, authorities and responsibilities. The web application should let all the stakeholders perform all their task and activities efficiently with minimum efforts and lots of ease.

###Technology Stack
Since this project is to extend some features in [systers portal][portal_github], tech stake will be same as that of systers portal, that said: Python/Django and postgreSQL.
For showing map to users, [django_easy_maps][2] can be used, I have used this app in my past project [Help The Needy!][3].

###Project Design Details
The project needs to add some apps to systers_portal project to fulfill all needs of Systers' global community that they want in their meetup application.

We will need to define following database tables:

####Database:
All attributes marked with `*` are mandatory.


For **Chapters(local_meetup_groups)**  

1. **chapters**: 

    -  id* #default primary key
    -  chapter_name*   #unique key
    -  chapter_city*
    -  chapter_country*
    -  status(requested, approved, canceled)*
    -  requested_datetime*
    -  responded_datetime
    -  requested_by_id*    #foreign key from auth_user table
    -  responded_by_id     #foreign key from auth_user table
    -  chapter_description*

2. **chaper_organizers**:

    * id*  #default primary key
    * chapter_id* #foreign key from chapters table
    * organizer_user_id* #foreign key from auth_user table

3. **chapter_members**:

    * id*     #default primary key
    * chapter_id*  #foreign key from chapters table
    * member_user_id*  #foreign key from auth_user table

For **meetups**  
4. **meetups**:  
 
  *  id*     #default primary key
  *  meetup_datatime
  *  meetup_venu
  *  meetup_short_description*
  *  meetup_long_description
  *  chapter_id*     #foreign key from chapters table
  *  meetup_posted_organizer_user_id*     #foreign key from auth_user table
  *  last_updated_datetime*

This was the basic chapter's and meetup's database, as we will add more features in the project, more table will be needed, mentioned below
 
For Syster meetup **visitors or guest**  
5. **users_systers_meetup_guest**:    
For those who are not systers member but want to attend the meetup, can be a visitor or a guest. This can be implemented like SystersMeetupGuest as we have SystersUser in systers_portal.

  *  id*     #default primary key
  *  guest_user_id*  #foreign key from auth_user
  *  meetup_id*  #foreign key from meetups table
  * member_invited_id   #foreign key from auth_user, required only igu est
  *  is_invited    #boolean value
  *  invitation_status(0:invited, 1:accept, 2:reject)*

For **meetup sponsors**  
Since sponsor can be a Syster member(for which we will have a userid) or a non-member(we don't have whose data), if she/he is non-syster first need to register in auth_user. This can be implemented like SystersMeetupSponsor as we have SystersUser in systers_portal.    
6. **users_systers_meetup_sponsor**:

  - id*     #default primary key
  - sponsor_id*     #foreign key from auth_user
  - meetup_id*  #foreign key from meetups table
  - amount*
  - date_of_donation*
  - is_syster-member*   #boolean value, this will map sponsors(systers or non-systers) to the meetup for which they sponsored and also amount they donated.

For **RSVP**  
7. **meetup_rsvp**:

  - id* #default primary key
  - user_id*    #foreign key from auth_user table
  - meetup_id*  #foreign key from meetup table
  - is_coming*  #boolean value

For **discussion area**:  
It will be very similar to the comment model as in blog app of systers_portal.

We will need to add following **groups and permissions** in Django auth system:

Since the need is to design and develop an integrated component of the portal for global community meetups where **Systers administrators can control overall**, but **more permission controls** are given to **local community organizers**, the Systers_admin will have all the privileges, two new groups will be needed:  

**group_name**: permission1, permission2.....  

  - **meetup_organizers**: change chapters, create, change, delete meetups, reate, change, delete members, create, change, delete guest, create, change, delete discussions, create, change, delete sponsors  
  - **meetup_members**: create discussions

Once the database is finalized, I will convert it into Django models.

####New Apps:  
Since Django works on 'multiple apps in a project' policy, I am planning to add following different apps and also stating what functionality each app will deliver. Basically this will be the overall architecture of project meetup-features.

1. **chapters**: 
  Manage local meetup groups data and functionality, cover following features:
 *  Admin - create a new Systers meetup location(Chapter)
 *  Admin - Assign meetup organizers
 *  Admin - Receive notification of all new meetup requests, and will able to approve or reject them
 *  Organizers - Submit initial request for new Chapter(meetup location)
 *  Organizers - Manage Chapter Profile
 *  Organizers - Able to post new local meetup

2. **meetup**: 
Manage data and functionality of all meetups under a chlocal_meetup group). Will cover following tasks:
  * Organizers - Manage Meetup Profile
  * Organizers -  Able to manage meetups other than events in a meetup
  * Organizers - Approve or disapprove invited guests or requestsvisitors to attend the meetup
  * Organizers - Able to maintain sponsors for a particular meetup
  * Organizers - Able to maintain logistics of a particular meetup
  * Member - chapter_member will get mapped with a particular meetup.


3. **discussions**:
   It will be similar to a blogging app with extra features thatrequired like Organizers can approve posts, reply to posts etc.
   * Organizers - manage discussion posts
   * members - Post to Discussion area
   * Guests - View Discussion area  

####Tasks - urls, views, templates  
Since for almost each task/feature, we will need at lest one url, view, template; further let use look into details of how will we achieve each feature that needs to be added and their brief detail:

For **Systers Admin** we need to add:

  -   Manage all organizers and attendees
  -   Create a new Systers Chapter(meetup location)
  -   Assign meetup organizers
        Since Systers Admins will have all permissions to create, change and delete any thing in chapters, meetup and discussions app, all above features can be achieved from Django admin app without any hassle.
  -   Receive notification of all new meetup location requests
        * For this a meetup organizer can fill one form putting details of new local meetup group she is requesting for, then using notifications app all Systers admin will receive notification on their dashboard to take actions accordingly.
        * This will reside in chapters app.
        * A new button on organizer dashboard(template) to request new chapter(local meetup group), when an organizer click on `new chapter` button a form will appear(through a view) to fill the details of new chapter as per the chapters model.
        * Form get filled, organizer click on submit request button on form and then using notification app a notification will be send to all the Systers admin.

For **chapter(local_meetup_group) organizer** following features are needed:

  - Submit Initial Request for new chapter(local_meetup_group)
      *  This is same as of 4th feature for Systers Admin.
  - Manage chapter(local_meetup_group) profile
      *  As like a user can edit his profile in sysers_portal similarly organizers will be allowed to edit a chapters profile.
      *  template to edit similar to [syters_portal_users_edit page][6]
      *  form similar to [systers_portal_user_form][7]
  - Able to post new meetup
      *  `post new meetup` button on organizer dashboard(template)
      *  a form than will need the entries that are in the meetup model.
      *  On submitting form a new meetup get posted
      *  A new page for recently posted meetup will get generated whose link will be available on Systers meetup page and that chapter page for which meetup is posted
  - Manage Meetup Profile
      *  As like a user have option to edit his profile, similarly one `edit meetup profile` button will appear on organizer dashboard.
      *  A new template and form for letting organizers edit meetup profile will need.
      *  It will reside inside meetup app.
  - Identify all members as Systers or Guest
      *  Organizers will able to look at list of all those who are attending(or requested to attend) the coming meetup and in that list they can also see if a member is a Syster or a guest by a lable `S or G` respectively attached with each person's detail.  
        Also organizers can look at list of only those women who are Systers and who are guest separately.
        Also whenever an organizer look at profile of any other member they will see if a person is a Syster or a guest.
        For this one check will be needed that the user is a SysterUser or not.
  - Able to manage local meetup logistics
      *  meetup logistics include - venue, volunteers(meetup members), sponsorship
      *  while adding new meetup or editing meetup profile, an organizer can alter/manage these things
  - Be notified of all incoming join requests and Approve all Meetup join requests
      *  A meetup page is visible to everyone, a visitor/guest can look it, and also request for attending the meetup.
      *  A button on meetup page for visitors to request to attend the meetup
      *  Using notification app all organizers get notified for a new request for joining meetup and they can respond on it.
      *  Accordingly visitors will get notification as per organizer respond.
  - Manage discussion
      *  Discussions can be allowed at chapters page as well as on each coming meetup page.
      *  Very similar to comments in blog app of systers_portal.

 For **chapter(local_meetup_group )members** following features are needed:

  -  Edit Profile
      Being a Syster member they are allowed to edit their profile.
  -  Post to Discussion area
      chapter members will have privilege to post of discussion area, like any other Syster member is allowed to comment on blog.
  -  Invite a guest to meetup
      by clicking on "invite guest" button on meetup page, members are allowed to add details of guest they want to invite
        organizers will get a notification using notification app
        if organizer approved the request, guest will get notification

For **guests/visitors** following features are needed:

  -  Edit Profile
      This can be implemented in similar manner as systers_portal users edit profile work, with only difference of model.
  -  View Discussion area
      As like other systers members they are also allowed to view discussion area

###List of templates

* meetup_organizer dashboard that will contain:
    * button to add new chapter
    * button to add new meetup
    * button to edit chapter profile
    * button to add new members to chapter
    * notifications button to see all notifications
* Systers meetup page
    * show all latest coming meetups of different chapters
    * show list of all meetups help in past in different chapters
* Chapters profile page
    * show list of all coming and past meetups of that particular chapter
    * list of organizers of that chapter
    * discussions related to chapter meetups
* meetup page
    * contain details of a particular meetup
    * will able to take RSVP    
    * list of members of that meetup
    * a button for visitors to request to attend the meetup
    * a button for sponsors to offer sponsorship for that meetup
    * a button for organizer to edit meetup_profile
    * discussions related to a particular meetup
    * invite guests button for organizers and meetup members
* add new meetup/edit meetup profile
* add new chapter/edit chapter profile

###Milestones

1. Front end **mockups**
2. Systers super **admin panel**
    * Providing all CRUD features to super admin for chapters, meetups, chapter organizers
3. Meetup **Organizer Dashboard**
    * organizer should be able to add,edit new chapter
    * organizer should be able to add,edit,delete new meetups
    * organizer should be able to add,edit,delete members to the chapter
4. **RSVP** feature to meetup pages
    * users including admin, organizer, member, guest/visitor will able to RSVP if they are attending the meetup or not
    * result of RSVP will be visible on meetup page for all users
5. **Notification** facility for chapter approval
    * Up till now organizer is able to add new chapter by itself but by adding this feature new chapter will be added only after admin approval
    * Admin get notification of organizer request to add new chapter and will able to approve/disapprove the request
6. Facilitating **visitors/guest** to request/invite to join the meetup
    * A visitor can request to attend a particular meetup and organizer of that particular meetup chapter will able to approve/disprove
    * Meetup member, organizer will able to invite guests to attend meetup
7. Sponsors feature
8. Discussion Area
9. Final Demo

###TIMELINE

<table border="1">
  <tbody>
    <tr>
      <th>Tasks</th>
      <th>Milestone covered</th>
      <th>Week</th>
      <th> Date </th>
    </tr>
    <tr>
      <td><ul><li>Finalizing Database and architecture of project by finalizing different apps needed </li><li> Designing Mockups  (My college exams will be running till May first week, so I will be a bit slow these days)</li></ul> </td>
      <td>Milestone-1 Mockups</td>
      <td>1</td>
      <td>1-7 May</td>
    </tr>
    <tr>
      <td>
        <ul>
          <li>Initial Setup, initializing default apps</li>
          <li>Defining models for chapters and meetup</li>
        </ul>
      </td>
      <td></td>
      <td>
       2
      </td>
      <td>8-14 may</td>
    </tr>
    <tr>
      <td>Defining models for chapters and meetup</td>
      <td></td>
      <td>3</td>
      <td>15 May</td>
    </tr>
    <tr>
      <td>Attending OpenStack summit(if get visa)</td>
      <td></td>
      <td>3-4</td>
      <td>16-25 may</td>
    </tr>
<tr>
    <td class="tg-031e"><ul><li>Writing tests for models</li><li>Admin should be able to perform all CRUD functionality for chapter and meetup apps</li></ul></td>
    <td class="tg-031e">Milestone-2 Systers super admin panel</td>
    <td class="tg-031e">4</td>
    <td class="tg-031e">26-28 May</td>
  </tr>
  <tr>
    <td class="tg-031e">Starting coding for organizers dashboard, since the organizer will be a syster member we don't need to code for login/logout just need to check if a user is an organizer or not, writing code for views and forms for adding a chapter, editing chapter, front end for dashboard</td>
    <td class="tg-031e">Start Mileston 3</td>
    <td class="tg-031e">5</td>
    <td class="tg-031e">29May - 4 June</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>writing tests for views and forms of adding and editing chapters</li><li> writing views, forms for adding, editing a meetup in a chapter</li></ul></td>
    <td class="tg-031e"> </td>
    <td class="tg-031e">6</td>
    <td class="tg-031e">4 - 10 June</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>Front end for adding, editing meetup, generating a new page for each new meetup</li>Documentation<li>Code review</li></ul></td>
    <td class="tg-031e">Half of milestone 3 covered</td>
    <td class="tg-031e">7</td>
    <td class="tg-031e">11-17 June</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>Adding, editing new chapters and new meetups should be ready!</li><li>Filling Gap, reviewing code, fixing test, code, bugs, documentation</li></ul></td>
    <td class="tg-031e">Filling Gap</td>
    <td class="tg-031e">8</td>
    <td class="tg-031e">18-23 June</td>
  </tr>
  <tr>
    <td class="tg-031e">Mid - term Evaluation</td>
    <td class="tg-031e"> </td>
    <td class="tg-031e">8</td>
    <td class="tg-031e">24-June</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>Views, template, test for adding new members to chapters</li><li>code review</li></ul></td>
    <td class="tg-031e">Milestone-3-Organizer dashboard</td>
    <td class="tg-031e">9</td>
    <td class="tg-031e">25June - 1 July</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>First version of organizer's dashboard should be ready.</li><li>Deciding and writing models for RSVP feature</li><li>Test for models</li></ul></td>
    <td class="tg-031e"> </td>
    <td class="tg-031e">10</td>
    <td class="tg-031e">2-8 July</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>View for RSVP feautre</li><li>Modifying meetup template for RSVP</li><li>Test of RSVP views, documentation</li></ul></td>
    <td class="tg-031e">Milestone-4-RSVP feature</td>
    <td class="tg-031e">11</td>
    <td class="tg-031e">9-15 July</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>Implementing notification feature for admin to add new chapter</li><li>Notifications for organizer to approve requests of visitors to attend the meetups</li></ul></td>
    <td class="tg-031e">Milestone-5-Notification feature</td>
    <td class="tg-031e">12</td>
    <td class="tg-031e">16-22 July</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>Views, forms for visitors to attend the meetup</li><li>Template for visitors to fill their details</li><li>Tests, documentation, code review</li></ul></td>
    <td class="tg-031e">Milestone-6-Vsitors/Guest</td>
    <td class="tg-031e">13</td>
    <td class="tg-031e">23-29July</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>Writing model for sponsors, test cases for these models</li><li>View, template for sponsors, test cases for view, documentation</li><li>Models for discussion area, test cases for models</li></ul></td>
    <td class="tg-031e">Milestone-7-Sponsors feature</td>
    <td class="tg-031e">14</td>
    <td class="tg-031e">30July-5Aug</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>View for discussion</li><li>Template for discussion</li><li>Tests and documentation for discussion area, code review</li></ul></td>
    <td class="tg-031e">Milestone-8-Discussion Area</td>
    <td class="tg-031e">15</td>
    <td class="tg-031e">6-13 Aug</td>
  </tr>
  <tr>
    <td class="tg-031e"><ul><li>Tesing, code optimization</li><li>Final testing, Documentation, bug fixing</li><li>Preparing Demo</li></ul></td>
    <td class="tg-031e">Milestone-9 Demo</td>
    <td class="tg-031e">16</td>
    <td class="tg-031e">14-20 Aug</td>
  </tr>
  <tr>
    <td class="tg-031e">Final Evaluation</td>
    <td class="tg-031e">Filing Gap, final evaluation</td>
    <td class="tg-031e">17</td>
    <td class="tg-031e">21-27 Aug</td>
  </tr>
  </tbody>
</table>



**Proofs** that I have **deployed** Portal locally and are able to run the project and the tests:

  * [Systers Portal running locally][8]
  * [Systers Portal Admin running locally] [9]
  * [Tests Running properly][10]

______

> Have you used PHP, MySQL, Python/Django, Ruby/Rails and/or other frameworks? If so, please elaborate on what project/assignment you used these development tools.

Yes! Since I love programming in **Python** and have interest in **web development** the very first web development framework I chose to try and learn was **Django**. First I started with writing scripts for small tasks like database cleaning, searching text in a file using regex, fetching content of a site using request, url lib and many more.

  * One script I wrote in Python for [scraping data of food trucks][5] from web.
  * One script I wrote in Python to demonstrate how [github API works with python][11] to fetch users data.
  * Then I started sharing my knowledge of Python with other, taking workshops, building projects. One of my ongoing Python based project is [Gitoscope][12],  developing it just for fun(and also because I have keen interest in git and github). It will give a horoscope of a user on github, based on github data. It is being developed in Python, D3.js and github APIs.
  * Continuous learning and having keen interest in Python, I got a chance to work for [OpenStack][13]’s Zaqar component as an [Outreachy][14](previously known Outreach Program for women) intern. OpenStack is developed in Python and for my project while the internship too I got a chance to work in Python. I rebuild and reframe Zaqar internals by splitting its control and data layer completely. Brief details about project [here][15]. My contribution for OpenStack can be looked at [OpenStack reviewer][16].
  * Recently I developed a web app for a [HackerEarch][17] women’s day hackathon [Help The Needy!][3] using Django and Twitter Bootstrap. The app basically help users to find NGOs near to them so that they can help the needy!

---

**General Development and Education Questions**

>Why do you think you are a good candidate for these projects? Describe the skills you confidently bring to the project, and what you hope to learn from working on this project, and your interest in the Systers mission.

I have multiple concrete reasons to work on this project:  

####My Interest  
The very first reason is that the project is related to web development( Python+Django+database) that is something I am highly interested in. Working on python Django and databases always makes me happy. Website is a communication medium with others and I think I can communicate with others very well through my coding skills.
Another reason is that this project demands to build something from scratch, it is not something to modify an existing functionality rather add new features so I have to write whole code from 'start to end' and this will let me learn a lot!

####Past Experience
I already have an experience of similar program - [Outreachy][14](previously known as Outreach Program for Women) where I worked on a project with running my college in parallel, it was “winter of code” for me. So being committed to the project(and timeline) and working hard for it is what am aware about very well.

I have build a web app in 4 days [Help The Needy!][3] for a hackathon on world women's day that let users find NGO near to them so that they can help needy. So again committing with time and working hard for project is what I am well aware of.Thus for Systers Meetup Features also I will able to work without hassle.

#### Motivation
I have been working as **Python** Developer and Analyst at [Development Center][18](DC, a section in my college) along with my college studies. There we took lots of initiative to bring people more close to technology. Like I have taken workshop for Python, HTML, CSS, Linux for my college students. Also we went to villages to teach girls about basics of computers. This shows my motivation towards computer science and technology and also towards bringing people(specially women) closer to technology.

####Systers Mission
This is well known that ratio of Women in technology is very less comparative to Men. Reason is not that women are not capable, they are! it is that "they does not participate".
Systers Mission to “increase the number of women in computer science and make the environments in which women work more conducive to their continued participation in the field.” is what I immensely support! As I have mentioned above I took initiative to teach village girls about computers. Looking at the happiness and satisfaction on the face of those girls gave feelings that are something way beyond the imagination. We need to bring more and more women close to technology and computer science.
Women need to believe that they are awesome! They can stand along with any other men in the world and give them tough competition!

> We have various projects in Python, Ruby, Android, iOS and Ushahidi. Describe the largest project you have completed in any of the programming languages mentioned. If you haven't used any of the programming languages, describe the programming experience you have that will allow you to learn a programming language quickly and be successful on this project.

Programming in Python is something I enjoy a lot! Python is my primary language for any dam task I want to code for! I took few workshops on Python because I wanted to share my knowledge of Python with others and want them also know and realize how awesome it is! Being easy, enormous library support, fits in any field you want, strong community it allow students to develop applications easily, convert their ideas into reality without much hassle.

Following are the projects based on Python, I worked for:
  1. [OpenStack][13] Zaqar(being an [Outreachy][14] intern): Zaqar is a messaging and queuing service. I re-architect the storage layer of Zaqar by separating its control and data plane completely. This is my first Open Source contribution and it is in Python. Here are my contributions for at [OpenStack reviewer][16].  Brief [details][15] about Project while internship.
  2. [Help The Needy!][3] web based application, developed for a HackerEarth hackathon in 4 days developed in Python Djago, Twitter Bootstrap, Google Places API.
  3. [Gitoscope][12]: An application to show a user's performance on his/her github account. It is an ongoing project and I work for it in my free time. Tech Stack in which this project is developing are Python, Github APIs, D3.js
  4. I wrote [script][5] to scrap food trucks' data.
  5. I wrote [script][11] to demonstrate who github API can be used using Python without any other library.
  6. My more hacks in Python can be found at my [github profile][Github].

>We use GitHub and LaunchPad for our projects.Do you have experience with any version control software?

Yes! Version controlling systems became a prerequisite for contributing to open source world. One should be aware of it, if she/he want to contribute to open source. All my projects are on github. I am very well familiar with it and with git internals too. In fact I am a big git fan. I use github on daily basis to keep my code version, on cloud so that I can access it from anywhere I want and also to share the code with other team members and the world.

While contributing for OpenStack I got familiar with LaunchPad too.

> Describe any plans you have over the time period of GSOC (including the community bonding period) in addition to GSoC, such as classes, a summer job, vacation plans, master's thesis, etc.

Not any concrete Plan, but since I got travel grant from [OpenStack][13] Foundation to attend a 5 days [design summit][19] in Vancouver Canada, if I will get my Visa approved for Canada I won't be able to work on project for approximately one week from 16-25 May.

I have scheduled the timeline for project accordingly.

###Education: 

>What year are you in school?

I am in fifth year of an integrated(10+2+6) masters program in my college.

>What programming courses have you taken?

Apart from C, C++ and Java,Algorithms, relational databases. Linux courses in my college I have taken various courses on Python, Programming, Databases, DataScience from various MOOCs offering sites like [Coursera][20], [edx][21], [Udacity][22] and self learning tutorials like [codeacademy][23].
    

>What did you like about them? What did you not like? 

The first language that I learned was C. Writing programs in C was full of fun task for me. Then I have learned other languages, various concepts and fundamentals of computer science. I still remember while taking algorithms classes I got to know how a Fibannaci series program can be implemented in different ways using iterative approach, recursive approach, dynamic programming, matrix optimization approach and how the programs' time complexity can vary from exponential to O(n) to Log(n).

Also it feels great when you know/realize power of softwares, very best example I have is of git(I am a big git fan!), for Linus Torvalds it is nothing more than a source control management system. But then things like github came up, they let people share their code with others, practice collaborative development, looking at history of the code and project, participating and contributing for someone else's project. It has changed the way how open source communities work. It put developers/coders/programmers around the globe together!

The thing that I don't like is we as a student are forced to study all the subjects in curriculum(in Indian education). The subjects that I don't like much like Analog electronics, linear electronic systems etc are also part of my studies no matter I like to study about them or not.

>What is your major?

Computer Science

>Why have you chosen that? 

I believe Technology and Computer Science is something that is not just amazing,but also involved in each and every part of our life. It plays a role in how we order/cook food to how we connect to thousands of people in the world. It holds a vital part in almost all the activities we do in our life. It have power to change the world, change the culture, change the thinking of people, making the lives more better.
         
I strongly believe everyone should learn about technology and be a part of tech world. And best part is there is nothing hard in it. If someone is interested in arts they can use technology and CS, if someone interested in share markets they can use technology and CS, if someone interested in cars they can use technology and CS. I believe if a person understand technology and is close to it he/she has a power of changing things  and world for themselves as well as for other people.

So this is the reason why I chose it! I am highly passionate about building the tools/software/applications that people want, that can make their life better and easier.

>Have you done group projects (programming or otherwise)?

Yes. Lot many!
  * Being an [OpenStack][13] intern(under [Outreachy][14] program, my first open source contribution, more detailing is below under my FOSS contributions), I worked with whole Zaqar team and witOpenStack hackers.
  * Developed a mobile responsive web app [Help The Needy!][3] in group of two.
  * Being a part of [DC][18](a section in my college) I have learned importance of a team. I realized that happiness and success of all team members is as much important as of yours. I realized one can't move ahead alone. Its the team that support you, nourish you, help you, myou to keep moving ahead to achieve the goal.
There I have worked in group for following projects:  
[IIPS website][24]  
[Bonafide Automation][25]

>What was your primary contribution to/role in the group? 

Following are the projects in which I worked in group:

<table border="1" cellspacing="0">
  <tbody>
    <tr>
      <td align="left" height="17"><b><font face="Liberation Serif">Project</font></b></td>
      <td align="left"><b><font face="Liberation Serif">My Role</font></b></td>
    </tr>
    <tr>
      <td align="left" height="32"><font face="Liberation Serif">OpenStack Zaqar</font></td>
      <td align="left"><font face="Liberation Serif">Intern</font></td>
    </tr>
    <tr>
      <td align="left" height="47"><font face="Liberation Serif">Help The Needy!</font></td>
      <td align="left"><font face="Liberation Serif">Partner(Every thing from start to end I and my partner decided collaboratively.)</font></td>
    </tr>
    <tr>
      <td align="left" height="17"><font face="Liberation Serif">Gitoscope</font></td>
      <td align="left"><font face="Liberation Serif">Team Leader</font></td>
    </tr>
    <tr>
      <td align="left" height="32"><font face="Liberation Serif">Bonafide Automation</font></td>
      <td align="left"><font face="Liberation Serif">Team Leader</font></td>
    </tr>
    <tr>
      <td align="left" height="17"><font face="Liberation Serif">IIPS Website</font></td>
      <td align="left"><font face="Liberation Serif">Team Member(Database Engineer)</font></td>
    </tr>
  </tbody>
</table>


>What made working in a group better than alone? What made it harder?

  * As I mentioned few above, working in a group/team have lots of benefits.
    * By looking at efforts and work of team mates one get lots of motivation and it inspire to work more.
    * Working in a team let one learn new technical and behavioural things from teammates that is not possible being alone(peer learning). Even one get to learn the skills that he/she didn't know about.
    * One get lots of emotional support from teammates.
    * Sometimes one get chance to lead thus also develop leadership quality.
    * Since more members are involved project can be completed in short period.

  * What make it harder:
  Again being a part of DC I have been involved in many projects(all group projects) that are not completed and left in between or if completed are not in use. Yes all those are our failed projects but we all got to learn how to work in team from those failed projects. The things that make it harder to work in a group/team are:
    * Communication Gap between team members .
    * Different personal goals of each individuals. All group members are not committed to work for common goal.
    * Conflicts/Arguments in the views may occur.
    * Less involvement of one member than other.
    * Authority Struggle, sometimes members struggle for power and authority and thus forget the main goal of developing product.

>Do you have work experience in programming? Tell us about it.

Yes!  
Since the past four years I am studying computer science subjects and practicing programming. Programming in Python is a thing that I enjoy a lot. My past experiences in various projects are already mentioned above but I will put them here again. Here is the list of my past programming experience:

  * I worked as an  [OpenStack][13] intern for Zaqar component under [Outerachy][14] program(previously known as Outreach Program for Women). Look at [brief detail][15] of my project.  Basically I re-architect the storage layer of Zaqar such that its control and data part are fully separate now. My contributions for OpenStack can be glanced at [OpenStack reviewer][16].
  * I build a web app [Help The Needy!][3] for a hackathon on Women’s Day.
  * I am developing a web app [Gitoscope][12] to generate git-horoscope of a user using github data.
  * I wrote various Python script just for fun, like one is for web scraping, [fetching data of food trucks][5].
  * Another one is for using github APIs to [fetch data of github user][11] including their repos, issues they are connected with, lines of code, commits they made etc.
  * My few other hacks can be found at my [github profile][Github].

>Do you have previous open source experience? Tell us what you have done.

Yes!

  *   I am a big FOSS lover, I am a big FOSS lover, in 2012 my college senior(GSoC alumni) introduced me to the new world on Computer Science - “Open Source” since than I was looking forward to write open source code. I am grateful to Gnome Foundation that they run [Outreachy][14] program(previously known as Outreach Program for Women) under which I worked as [OpenStack][13] intern (Outreachy round 9 from November 2014 to March 2015). Brief [explanation][15] about project. During my internship I have learned about the Zaqar's(messaging and queuing service) storage layer implementation and was able to apply my knowledge and ideas for re-architecture of its drivers(data and control driver of storage layer) with the help of [flaper87][26] (my mentor during internship) and other Zaqar developers. While working for Zaqar I got to learn about many new tools, techniques, concepts and practices used in real world software development. My commits for OpenStack can be glanced at [OpenStack Revwier][16].
  *   For [HackerEarth][17] hackathon I along with my partner developed a web app - [Help The Needy!][3] that help users in helping the Needy by finding NGO near to them. We publish this application under open source MIT Licence. Technology stack used for this project are Django1.6, Twitter Bootstrap and Google's Places API. The first version of project is completed and now I work for it more only in my free time.
  *   I am an active Python Developer and Analyst at a section in my college named [Development Center][18](DC). I have explored many different fields of computer science there like Data Science, Cloud Computing, Web and Desktop application Development etc. I have worked on few projects([Bonafide Automation][24] and [IIPS website][25]).

>Tell us one interesting fact about yourself.

Considering the fact that women participate less not only in Technology but in any other Leadership activities too, as a women I feel sad and always dream to change this scenario.

We all know the famous line "Behind every successful man there is a woman". Here Men give credit to Women because they want more support from women. By giving credit to women they ensure that in future also they will get equal or even more support from women for their success. Why can't we do something vice-versa? Let us also give credit to men for a woman’s success. Let us also say - "Women need support of Men to be successful". By saying this somehow we force men to support women and let them work hard to be successful. Now it will be a men's failure if less women are successful because it is men who are lacking at something(supporting element) not only women.


Since Systers mission is to bring women closer to technology, it is something fascinate me, motivate me. Apart from fact of my interest in [Python][5] [Django][3] web development or my [past experiences][16] in Open Source, this is also a reason why I want to work for this project. I will be happy if I could build something that can help a community that want women to become successful in computer science and leadership. Also I would love to continue my contribution for Systers even after GSoC.

[1]: Additional info: https://github.com/systers/portal/blob/develop/docs/requirements/Systers
[Website]: http://exploreshaifali.github.io/  
[Github]: https://github.com/exploreshaifali/    
[Twitter]: http://twitter.com/exploreshaifali  
[Linkedin]: https://www.linkedin.com/profile/view?id=257818581  
[Google+]: https://plus.google.com/u/0/+ShaifaliAgrawal  
[Launchpad]: https://launchpad.net/~agrawalshaifali09 
[2]: https://pypi.python.org/pypi/django-easy-maps
[3]: http://help-the-needy.herokuapp.com/
[4]: https://github.com/systers/ossprojects/wiki/Meetup%20Features
[portal_github]: https://github.com/systers/portal
[5]: http://nbviewer.ipython.org/github/exploreshaifali/Programming/blob/master/web-scraping-for-food-trucks.ipynb
[6]: https://github.com/systers/portal/blob/develop/systers_portal/templates/users/edit_profile.html
[7]: https://github.com/systers/portal/blob/develop/systers_portal/users/forms.py
[8]: http://imgur.com/vRuvVBU
[9]: http://imgur.com/gallery/XQGLME1/new
[10]: https://gyazo.com/7d3e93272666b8b6d71637925920569a
[11]: http://nbviewer.ipython.org/github/exploreshaifali/Programming/blob/master/github%20API%20to%20access%20Public%20Data.ipynb
[12]: https://github.com/curioswati/Gitoscope
[13]: http://www.openstack.org/
[14]: https://wiki.gnome.org/Outreachy/
[15]: https://wiki.openstack.org/wiki/Internship_ideas#Zaqar:_Split_data.2Fcontrol_planes
[16]: https://review.openstack.org/#/q/owner:shaifali,n,z
[17]: https://www.hackerearth.com/
[18]: http://iips.edu.in/dc
[19]: https://www.openstack.org/summit/vancouver-2015/
[20]: https://www.coursera.org/
[21]: https://www.edx.org/
[22]: https://www.udacity.com/
[23]: https://www.codecademy.com/
[24]: http://iips.edu.in/
[25]: https://github.com/iips-dc/bonafied-automation
[26]: http://www.flaper87.com/