---
layout: post
title:      "Ruby Web Scraping CLI Data Gem"
date:       2018-01-23 07:19:46 +0000
permalink:  ruby_web_scraping_cli_data_gem
---


Upon seeing the instructions for this project, I have to admit I was intimidated. However, after watching Avi's CLI Data Gem Walkthrough video and a provided example gem on Github, I quickly became more comfortable, and even very excited to do the project. 

The perfect website for scraping came to mind - www.zrankings.com, a ski mountain rating/ranking website. The home page is a list of ski mountains and summary information, with the option to click on them to learn more. The requirements for this project were to show the user a list of of available data, and be able to drill down into a specific item. I knew of this website from being an avid skier, but did not know it would come in handy when learning to code. 

Writing the program was a pretty smooth process. Using Avi's video as a guide, I created the program to execute using a bin file, which would call a CLI class, which would scrape the website using a Scraper class and a Resort Class. Below is a summary on how each class operates. 

CLI Class: 
- prints out initial list of resorts to pick from 
- asks user for input and handles the input
- calls Scraper and Resort methods

Scraper Class: 
- opens the zrankings.com webpage and stores as a variable
- uses resort data on the web page to instantiate resorts

Resort Class: 
- stores data for each resort
- each resort is instantiated with a name, location, rank, url, multipass membership and description. Each of these pieces of data are scraped upon the creation of each instance
- separate instance methods scrape additional information: average annual snowfall, peak elevation, vertical drop, and acreage

Although this program is rather simple, I look forward to using these skills for more complex uses in the future. 

https://github.com/dtow55/cli-data-gem-project




