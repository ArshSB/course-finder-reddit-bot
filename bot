import praw
import time
from bs4 import BeautifulSoup
import requests

def run_bot(reddit):
    
    sub = reddit.subreddit("ubco").new()   #get the latest posts on the subreddit
    
    for post in sub: #looping through all the latest posts 
        if ("!find" in post.selftext) and (not post.saved):  # checking if the bot is called in that post and if the bot hasn't already replied 
            reply_post(post)
        
        for comment in post.comments.list(): #looping through all the comments (even replies to other comments)
            if ("!find" in comment.body) and (not comment.saved):
                reply_comment(comment)
            
            
# not using sub.stream.comments() or sub.stream.submissions() as that will be much more complicated to check for both new comments and posts continuously
# so it is best to do it manually and have the bot run once every certain minutes as the subreddit itself isn't very active


def reply_comment(comment):
    bot_request = comment.body.upper().split("!FIND", 2)[1].strip()  #get only the 2 words after !find which represent the course name & code
    request = bot_request.split(" ")  #separate the course name and code 
    course_name = request[0].strip() 
    course_code = request[1].strip()
    bot_reply = get_info(course_name, course_code)
    comment.reply(bot_reply + "\n\n**I'm a bot and this was performed automatically. If you have any concerns or questions, please contact my creator [here](https://www.reddit.com/message/compose?to=CanadianSorryPanda&subject=&message=)**")
    comment.save()  # save the comment so the bot doesn't reply to it multiple times
    print("Replied to a comment")
    
def reply_post(post):
    bot_request = post.selftext.upper().split("!FIND", 2)[1].strip()
    request = bot_request.split(" ")
    course_name = request[0].strip()
    course_code = request[1].strip()
    bot_reply = get_info(course_name, course_code)
    post.reply(bot_reply + "\n\n**I'm a bot and this was performed automatically. If you have any concerns or questions, please contact my creator [here](https://www.reddit.com/message/compose?to=CanadianSorryPanda&subject=&message=)**")
    post.save()
    print("Replied to a post")
  
  
def login_bot(): 
    
    reddit = praw.Reddit(client_id = "",  
                         client_secret = "",
                         password = "",
                         user_agent = "",
                         username = "")
    return reddit


def get_info(course_name, code):
    
    url = "http://www.calendar.ubc.ca/okanagan/courses.cfm?go=name&code=" + course_name  #get the database for the course name
    req = requests.get(url)
    soup = BeautifulSoup(req.content, 'html.parser')
    
    text_dt = soup.find_all("dt")  # get all course names from the website
    if not text_dt: # if empty, this means the course name doesn't exist 
        return "Sorry, I couldn't find the course you were looking for :("
    
    else:
        i = 0;   
        track_course_order = 0  
        #since the course description does not have any HTML attributes that link it to the course name, I am using this tracker to link them together
        intro = "" # this will hold the course name
        
        for course in text_dt:  # loop through all the courses until the the correct course is found
            i += 1 
         
            if code in course.get_text():
                intro = "*Course name:* " + course.get_text() 
                track_course_order = i   # this will help us loop the course descriptions until we find the correct description for the respective course name
                break
    
        text_dd = soup.find_all("dd")  # get all the course descriptions
        counter = 0
        desc = ""  # this will hold the course description
        
        for course in text_dd:  # loop through the descriptions until the correct description for the course name is found using the track_course_order variable
            counter += 1
            if (counter == track_course_order):
                desc = "\n\n*Course description:* " + course.get_text()
                break
            
        if not desc:  #if a valid course name is received but not a valid course code, then send a linked reply with the database for that specific course name
            return "Sorry, I couldn't find the exact course. [Here](" + url + ") is the database if you would like to check other " + str(course_name) + " courses."
            
        else: 
            return (". ".join((intro, desc)) + "\n\n^(*eg. first number represents the number of credits given by the course. [3 - 2 - 0] are lecture, lab and tutorial duration in hours for that course, respectively.*)")
    

def main():  
    
    reddit = login_bot()
    
    while True: 
        try:
            run_bot(reddit)
            print("Sleeping")
            time.sleep(120) # bot checks for new posts or comments once every 2 minutes
        except praw.exceptions.PRAWException as e:
            print("PRAW error: " + e)
            print("Waiting")
            time.sleep(3600) # rest for 1 hour if PRAW related error, longer wait-time is fine as the subreddit is not very active
        except Exception as e:
            print("Error: " + e)
            break  
        # if a non-PRAW error occurs then stop the program, if you want to run the bot indefinitely simply replace this line with time.sleep()
    


if __name__ == "__main__": # for Python interpreter if you want to run the bot from there as a py file
    main()
  
    
main()


    
