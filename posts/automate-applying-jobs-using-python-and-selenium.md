---
title: "Automate applying jobs using Python and Selenium"
description: "A personal Python project in which I build a system to automate searching and applying jobs."
date: "2021-10-13"
headerImage: "https://www.simplilearn.com/ice9/free_resources_article_thumb/Advantages_and_Disadvantages_of_Automation.jpg"
tags: ["projects"]
---

Let's admit it: A lof of us have gone through this point. Either we're recent graduates or those who look for new job opportunities, we spend hours on many websites looking for openings and submitting applications. This doesn't seem to be tidious until a person tries to apply for hundreds of openings. So I thought to myself: How about I try automating this application process. After several hours scripting and fine-tuning, I've concluded the final version of this project. This project covers two of the most popular platform for job seekers: Indeed and Glassdoor

### Indeed
Everything goes well until I try to submit applications There are a lot of compilation on Indeed website. The first is that there are two ways to apply for a job on Indeed: apply directly or apply on the company website. If the "Apply Now" button is visible for a post listing, you can apply for the job on Indeed. If the "Apply on Company Site" button is visible, I have to go to the company's website to apply. Because of this complication, I decided to skip all the listings that require users to visit companies' websites to apply.
Working further into the process, I notice that Indeed inconsistently, for some reason, uses iFrames on the job listings pages. To be more specific, the div containing "Apply Now" button sometimes uses iFrames, which requires more steps. Because of this, I need to check if the iFrames exists. If yes, I need to perform more steps to locate the frame, select the frame in order to click the elements within the frame.
Another issue arises while I automate the process on Indeed. When applying directly on Indeed, I was prompted to fill out all information in a pop-up modal. Modals of different job listings pose different questions and surveys, which makes it almost impossible to automate. For that reason, I decide to stop the automation after the modal pops up. Automating any steps beyond that time will require a lot of resource, which i figured it would not worth it.

### Glassdoor
A number of improvements have been made on this website:
1. Put all the variables into a separate file, which will be called within the function. All the variables are put into a dictionary. Orignially, I wanted to put the dictionary in the same glassdoor.py file. However, I wanted to have fun with opening/closing and reading data files. As a result, I ended up in separate file.
2. Like Indeed, some job listings allows users to apply directly on the website while others navigates users to other pages. I decide to focus on those can be applied on Glassdoor directly. This time I change the logic a little bit. Instead of clicking individual listings, I grabbed all the links embedded in the listings (which redirect users to another page) and put them in a set. If the redirected page is still on Glassdoor, I continue to automate the process by uploading resume and filling out all information.
3. As I planned to go beyond page 1, I had to code the project to change pages. Intuitively, I would click the "Next Page" button at the bottom of the page. However, I noticed that, from page 2, the URLs seem to have the same structure. The only difference is the page number. So I use regular expression to search for patterns and replace the page number to go around the pages.

### What I learned from this project
1. I was able to play around with a lot of modules: getpass allows users to input their passwords without showing them. working with lists, dictionaries and sets boosted my confidence in handling and managing data structures.
2. Until I worked on this project, I realized how little I'd known about regular expressions. This is where I strugggled the most as I had to figure out the patterns of urls of those websites. Thanks to this project, I learned that I need more practice on regex.

[View Project](https://github.com/hhphu/automate-apply-job)

