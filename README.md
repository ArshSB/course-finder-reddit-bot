# course-finder-reddit-bot
a reddit bot that helps users find course information for specific classes

## Purpose
this bot monitors the UBCO subreddit and replies to users with their requested course description and prerequisites from the UBC database [here](http://www.calendar.ubc.ca/okanagan/courses.cfm?go=name)

## Usage
either through commenting or posting in the subreddit, simply call the bot like this
```
!find *course_name* *course_code*
```
make sure to leave a space between *!find, course_name and course_code*

## Getting started
1. [register](https://ssl.reddit.com/prefs/apps/) an app with Reddit for authenticatication. Instructions [here](https://praw.readthedocs.io/en/latest/getting_started/authentication.html)
2. input your **client information, username and password** in *login_bot()* 
3. if desired, change or remove how often the bot checks for new posts and comments in *main()* 
4. run *main()* or the py file in the interpreter to start the bot!

## Built With
* [Python 3.8.3](https://www.python.org/downloads/)
* [PRAW 7.0.1](https://praw.readthedocs.io/en/latest/index.html) - for working with Reddit API
* [BeautifulSoup 4.9.0](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) - for UBC course database parsing
