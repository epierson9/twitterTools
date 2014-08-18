twitterTools
============

Tools for studying Twitter. At present this contains only mineTweets.py, which allows you to monitor Twitter for multiple sets of words or hashtags, collecting all data for live-streaming Tweets. So, for example, if I want to collect all Tweets containing “woody allen” and simultaneously and separately collect all tweets containing “#icebucketchallenge”, this program will allow me to do that. 

Usage: 

1. Figure out what sets of hashtags or words you’re interested in. 
2. Create a folder in your base directory for each set of hashtags/words you want to monitor. Set the base directory argument in the program using the “BASE_DIR” argument (line 14). 
3. Get a Twitter API key at https://apps.twitter.com/app/new and enter the appropriate information in lines 10 - 13. The consumer keys can be found on your application's details page located at https://dev.twitter.com/apps (under "OAuth settings"). The access tokens can be found at https://dev.twitter.com/apps under “Your access token”. 
4. Install tweepy (http://www.tweepy.org/). 
5. Run “python mineTweets.py folder_names use_hashtags use_non_hashtags hashtag_sets”
where you replace the underscored variables as follows: 

folder_names: The names of the folders for each dataset, separated by commas (no spaces). 

use_hashtags: True for each dataset if you want to search for hashtags, False if you just want to search for words. (Ie, if I want “#lol”, I put “True”; if I just want “lol”, I put “False”). Put commas between each True and False. 

use_non_hashtags: True for each dataset if you want to search for words, False if you just want to search for hashtags.  (Ie, if I want “lol”, I put “True”; if I just want “#lol”, I put “False”). 

hashtag_sets: a list of comma separated words to search for (no spaces; if there are spaces in what you want to search for, replace them with underscores. Ie, if I want to search for “robin williams”, I would put “robin_williams”). Capitalization does not matter. If you have multiple sets you’re interested in, put a space between each set. 

Examples: 

I am interested in a collecting single set of Tweets relating to the Ferguson shooting. I want both words and hashtags. 

python mineTweets.py ferguson True True
ferguson,iftheygunnedmedown,dontshoot,handsupdontshoot,handsup

I am interested in collecting Tweets related to basketball stars and separately collecting Tweets relating to chess stars. 

python mineTweets.py basketball,chess True,True True,True
lebron_james,kobe_bryant garry_kasparov,judit_polgar

I am interested in collecting lol but not #lol because the latter turns out to often be pornographic and the former is more commonly used. 

python mineTweets.py lol False True lol

Other things: 

Beware that if you are monitoring a hashtag which is frequently used, you can collect a lot of data very quickly -- several gigabytes an hour. Also, Twitter will rate-limit you if you attempt to stream more than 1% of the total volume of Tweets -- my program will print out messages to let you know if this is happening. If you want to reduce the number of tweets you actually collect, you can change the “downsample_fracs” argument (line 17). For example, if I only want to collect 1/10 of Tweets for the “ferguson” dataset, I can put “downsample_fracs = {‘ferguson’ : .1}”. This will not help you with Twitter’s rate limit, though. 

Also, to help you monitor the program I print output to the console -- this is useful to see how things are going but if you’re getting a ton of Tweets it might conceivably slow things down, so feel free to comment out or reduce print messages. 

Output: 

Tweets will be dumped in groups of 1000 to the folders you specify. For each Tweet, the program just dumps all the information Twitter returns as a JSON, which is several KB per tweet. You can read in the JSONs one line at a time using json.loads(line) and then decide what fields you’re interested in.

Robustness: 

I have found this program to be robust -- ie, you can run it and ignore it and it is unlikely to crash and if it does it will restart itself. If you shut off the program, you can start it running again and it will continue where it left off without overwriting old data. I run it on an academic cluster and thus far the limiting factor has been that the cluster crashes. 

Bugs: 

Please let me know if you find any at emmap1@alumni.stanford.edu. In some cases, Twitter will return Tweets that do not contain the strings you’re searching for. I’m not sure why Twitter does this and I just discard these Tweets at present. 

This has been moderately well-tested where by “moderately” I mean “I have used this code for my own purposes for a few months and it seems to yield good results”. But I would still recommend looking at the outputted results because even when the code does what it’s supposed to, I have learned that the TwitterVerse can be very weird. 


