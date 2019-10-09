---
layout: post
title:      "**Journey through Scraping**"
date:       2019-10-09 17:25:09 -0400
permalink:  journey_through_scraping
---


Scraping a website seemed daunting, mostly because scraping doesn't always work how I think it will (or think it should).  Unfortunately, finding the correct data via scraping was the least of my issues.   Much like going through the Kubler-Ross 5 Stages of Grief, I went on an emotional scraping journey trying to complete my CLI Data Gem Project.

![](https://i.pinimg.com/236x/5f/2e/2d/5f2e2d08a900eb1e3e126eda3d52bbfc--funny-happy-birthday-meme-funny-happy-birthdays.jpg)

It started off all fine and good.  I decided I want to do a list of Colorado 14ers (for those people that aren't into climbing, hiking or just aren't from the west - 14ers are mountains with peaks higher than 14,000 feet). I found a website that appeared to have readable HTML and the data I was searching for, I wrote an outline and was testing out methods with "dummy" data.  Now onto scraping!  I find scraping infinitely easier using pry so I got to work starting big and working through CSS selectors to slowly narrow down the data to get what I want.  I learned useful tactics like using "puts" while working in pry and using ".text" or ".value" (quite liberally) to see information clearly (and getting the actual information I want!).  There are 50+ peaks that I'm scraping information for and each one has it's own page, so if you're counting along, that's a lot of pages to scrape - luckily there's iteration!  Each peak's name and href are in the same spots in the HTML so I got the information I want and iterated through it.  I should be able to do this for all the information I want to scrape so my daughting amount of data seems much more manageable.  
```
peaks = Nokogiri::HTML(open(@@main_webpage)).css("table.data_box4 td a")
peaks.collect {|peak| peak.text}
```

I got my first page scraped and moved onto my next page.  And then I came across my first real issue - getting the url for the second page I wanted to scrape (the page with almost ALL the information I want).  I wanted to get to it by going through a dropdown menu that appears when I hover over a certain part of the page.  Wouldn't that be easy?  I just needed those urls and I'm good to go...or so I thought.  Well, despite my best efforts (and the efforts of my cohort lead who I reached out to for help), Javascript was doing some behind the scenes work that is actually changing the HTML when I hover.  While I'm sure that there's a solution, that solution is likely beyond my beginner programmer skills so it was time for plan B: scrape to an interum page where I could get the url I wanted for my final page.  Sure it's wasn't plan A, but it's definitely doable.  So I got the scrape for my second page written and working. 

Now to my third page - and my next issue - the href I wanted is in an unordered list and I needed the 9th item down.  This wasn't in any of the lessons or labs!  How am I only going to grab the one thing I need?  

![](https://github.com/kseggelke918/co_14ers_cli/blob/master/scraping%20lis.jpg?raw=true)

I stared messing with pry and came up with:
```
second_page.css("div.sidebar_content ul.sectionlist li")
```
but I still needed to to get to to the href in the 9th li.  After searching around online, I got an assist from my cohort lead.  What if I looked for the item like this:
```
second_page.css("div.sidebar_content ul.sectionlist li")[8]
```
and it worked!  Next, I just needed the children:
```
second_page.css("div.sidebar_content ul.sectionlist li")[8].children
```
Then I was able to iterate to get the value of each href attribute.

After I got those couple scrape methods working, I was elated!  I packed up for the day and told myself I'll work on my last scrape method that weekend.  Or so I thought.

I moved onto the next level of scraping (remember - the page with the majority of the data I wanted).  I started coding and jumped into pry.   And that was when Ms. Kubler-Ross really showed up (uninvited).


![](https://github.com/kseggelke918/co_14ers_cli/blob/master/403error.jpg?raw=true)

**403 Forbidden (OpenURI::HTTPError)**  
I didn't know what this meant but I did know that I sure didn't like the sound of "Forbidden."  Everything was working fine!  Why did this start happening?  After some research, it seemed like my chosen website is  unhappy with how often I'm using it.  Trying not to get too far into the anger stage, I packed it in for the night.  I had a 1:1 the next morning.  Maybe I'd get guidance then.

![](https://miro.medium.com/max/1200/0*Bj_O1jRFzZjKxzi4.jpg)

Enter stage 3 - bargaining.  What do I need to do to make sure this all works?  To make sure that I don't need to start over?  I had my 1:1 and apparently this has happened before.  Phew!  There's a playbook for this!  I couldn't change the way the website is reacting (despite my best efforts) but I could change what my gem could do.  I couldn't go as deep as I wanted to, but I could still meet the requirements for the project.  Skip step 4 and onto step 5 - acceptance.  I had a plan and I didn't need to start from scratch.  I tweeked my code and my object attributes to accept the data I could get and added an attribute for the url I could scrape for - I'll be providing this to the user instead of all the attributes I was hoping to get.

It's been a wild ride, but I think the biggest take-away here is that there is always a way to do it, there's always a work around or a plan B (or C or D...or even Z).  When life throws you 403 Forbidden errors, you code some lemonade or whatever.


