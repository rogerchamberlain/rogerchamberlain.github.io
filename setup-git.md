---
layout: default
title: Setting Up Git
author: Doug Shook
---
{% include nav.html %}

# {{ page.title }}

![========](images/line.gif)

The first thing you will need to do is set up a <a href="http://www.github.com">GitHub</a> account. Go to GitHub and register for an account. Please use your @wustl.edu email address when registering.

For each assignment we will provide a GitHub link that contains the starter code. When you click on the link it should take you to a Team Registration Page, like the one seen here:

![team registration page]({{ "/images/18.png" | relative_url }})

One person in your team will need to create the team. Please use the last names of everyone in your team as the team name. Once the team has been created, the other members of the team should be able to see it and click the "Join" button to join the team.
This will create a repository for you with the code that you need. The next thing you need to do is create a clone of your repository on your local machine. First, go to the GitHub page for your repository (your repository should contain CSE132, the name of your assignment, and the name of your team) and copy the link:

![Github page for repo]({{ "/images/19.png" | relative_url }})

Next, open Eclipse and go into your workspace:

![Eclipse workspace]({{ "/images/8.png" | relative_url }})

Go to File -> Import. Then select Git project from the list:

![Git project]({{ "/images/9.png" | relative_url }})

Next, select "Clone URI":

![Clone URL]({{ "/images/10.png" | relative_url }})

Paste the link that you copied from GitHub into the box at the top. Fill in your GitHub login (not your WUSTL key!) at the bottom.
However, don't type in your GitHub password.  GitHub now requires a token instead of a password.  Follow the instructions [here]({{ "/files/githubEclipseAuth/githubEclipseAuth.html | relative_url }}) to setup your token.  Use your token for the password whenever accessing GitHub via Eclipse.

![Github login]({{ "/images/11.png" | relative_url }})

Eventually you will see the following screen. If you are on your laptop, then you do not need to do anything on this screen. **If you are on a lab computer, make sure to choose a location that is on the H:\ drive.** You can do this by typing in a location, or by clicking the "Browse" button and selecting the H:\ drive.

![H: drive]({{ "/images/17.png" | relative_url }})

Keep clicking next until you see the following screen. Make sure the box next to the CSE132 project is checked:

![CSE132 project]({{ "/images/12.png" | relative_url }})

Once you click "Finish", you should see the CSE132 project in your workspace. It should contain all of the materials for labs, studios, and extensions.

![========](images/line.gif)

## Committing your work

Committing your work is equivalent to turning something in. When you are ready to submit your work, right click the project and select Team-> Commit:

![Team -> Commit]({{ "/images/13.png" | relative_url }})

Type in a descriptive commit message then push the "Commit and Push" button at the bottom. Make sure to push! If you only commit (without the push) we will not be able to grade your work.

![Commit and Push]({{ "/images/14.png" | relative_url }})

You may commit/push as many times as you want. You should commit often! Think of it as saving your work.

![========](images/line.gif)

## Updating your repository

From time to time we might add files to your repository (such as grades). To do this, you need to update your repository. This can be accomplished by right clicking the CSE132 project and selecting "Pull":

![Pull]({{ "/images/16.png" | relative_url }})

This will get any changes that have been made to your repository. It is a good habit to run a synchronize before you make any changes to your repository, especially if you are working with a partner.

{% include footer.html %}
