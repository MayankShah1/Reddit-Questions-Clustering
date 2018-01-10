
# Data Extraction and Cleaning


I have used the python wrapper for Reddit API ie. [PRAW](https://praw.readthedocs.io/en/latest/) package for extracting the data from Reddit.

The steps are from the [Sentdex's tutorial on PRAW](https://pythonprogramming.net/introduction-python-reddit-api-wrapper-praw-tutorial/).

Steps:

### 1. Importing the libraries

    (a.) praw - wrapper for Reddit API
    
    (b.) time - as Reddit API allows only 60 API requests per minute, hence to avoid overusage and effectively extract information in a batched manner
    
### 2. Creating the Reddit API instance 
    
    Create an app on Reddit and use the credentials for creating the Reddit API instance.
    
### 3. Extract the data

    Extract the data using the subreddit method, and then extract the hot questions using the hot method.
    
    Further the non-stickied questions are extracted and saved in a file


```python
# Importing the libraries
import praw
import time
```


```python
# Creating a Reddit API instance 
reddit = praw.Reddit(client_id= 'client_id',
                    client_secret= 'secret_key',
                    username= 'user_name',
                    password= 'password',
                    user_agent= 'unique_name')
```


```python
# Pulling the data 
# Pulling the hottest 500 questions

subreddit = reddit.subreddit('AskReddit')
top_askreddit = subreddit.hot(limit=500)
```


```python
# Converting to list in order to avoid over usage of Reddit API

top_list = list(top_askreddit)
print('No of questions : {}'.format(len(top_list)))
```

    No of questions : 500
    


```python
cleaned_questions = []
```


```python
_size = 30

for _index in range(0, len(top_list), 30):
    for i in range(_size):
        if i + _index < len(top_list):
            if not top_list[i + _index].stickied:
                cleaned_questions.append(top_list[i + _index].title)
    print('Batch {} completed: '.format(_index))
    time.sleep(60)
```

    Batch 0 completed: 
    Batch 30 completed: 
    Batch 60 completed: 
    Batch 90 completed: 
    Batch 120 completed: 
    Batch 150 completed: 
    Batch 180 completed: 
    Batch 210 completed: 
    Batch 240 completed: 
    Batch 270 completed: 
    Batch 300 completed: 
    Batch 330 completed: 
    Batch 360 completed: 
    Batch 390 completed: 
    Batch 420 completed: 
    Batch 450 completed: 
    Batch 480 completed: 
    


```python
print(cleaned_questions[:20])
```

    ['What are life’s toughest mini games?', 'What are some slang terms a 50 year old dad can say to his daughter to embarrass her?', 'Redditors who were in attendance at a wedding that was called off mid-ceremony, what was the story?', 'What are your best “first date tips” for somebody starting the dating game late in life (late 20’s +)?', 'Chefs of Reddit, what are the biggest ripoffs that your restaurants sell?', 'The year is 2050. How do you think you would complete the sentence: "Back in my day, we didn\'t have ..."?', 'What screams "I\'m emotionally unstable"?', 'What is an imminent danger that nobody seems to be talking about?', 'What is the worst purchase you ever made?', "What's the WORST name for a strip club you can imagine?", 'What skills can a poor 19 y/o learn to help make an income and get further in life?', 'What’s something that you only do because it’s socially standard?', 'What saying from a movie or television show do you find yourself using in everyday life, even though you wind up having to explain it afterwards?', 'What mildly illegal thing do you do?', 'What do you regret doing at university?', 'What song fills you with the most emotions (positive or negative) and why?', 'What game have you spent the most hours playing?', 'Which artist has the fakest public image?', 'Americans, how do high schools as portrayed in Hollywood teen movies compare to your high school in real life?', 'What is a common internet thing that you hate?']
    


```python
reddit_ques = open('reddit_ques.txt', 'w')
for ques in cleaned_questions:
    reddit_ques.write("%s\n"%ques)
```
