# dataminig_tweeter


# Introduction :
Twitter is an American microblogging and social networking service on which users post and interact with messages known as "tweets".
It is a good ressource to collect data. We can find a few libraries (Python) which allow you to build your own dataset with the data generated by Twitter.

# Steps :
## Creating a developper account
## Importing tweepy and getting authentification keys:

```from tweepy import OAuthHandler
from tweepy import API

# Consumer key authentication
auth = OAuthHandler("qIwnNNJMkjBzFGXbvIfDCx5L1", "qBDTDdhi0PtgPg2SBIdZGOE5KpXqcjjQHdMXIXazbfa6wkxpb2")

# Access key authentication
auth.set_access_token("1338511495739674629-uCRKkRhkSBdUJO45rzkn4am36WEM4w", "889fsak4U775JstxLLgfaUcrEVYoJJS7ikBUUQNaMl5m4")

# Set up the API with the authentication handler
api = API(auth)
```

# Collecting tweets:

```from tweepy import Stream

# Set up words to track
keywords_to_track = ["welcome","today"]

# Instantiate the SListener object 
listen = SListener(api)

# Instantiate the Stream object
stream = Stream(auth, listen)

# Begin collecting data
stream.filter(track = keywords_to_track)
```

In this section we are going to focus on the most important part of the analysis. In general rule the tweet are composed by several strings that we have to clean before working correctly with the data.
# Deleting empty columns:


``` to_drop = ['in_reply_to_status_id','contributors','geo', 'withheld_in_countries', 'place', 'in_reply_to_status_id_str',
           'in_reply_to_user_id', 'in_reply_to_user_id_str', 'in_reply_to_screen_name', 'coordinates','retweeted_status-user-screen_name',
           'retweeted_status-text','display_text_range', 'quoted_status_id','quoted_status_id_str','quoted_status','quoted_status_permalink',
           'quoted_status-text','quoted_status-user-screen_name','extended_entities','withheld_in_countries','extended_tweet'
           ]
df_tweet = df_tweet.drop(to_drop, axis=1)
```

# Deleting pontuations, emojis and URLs
We can find an URL, punctuations and a username of one tweetos (preceded by @). Before the data visualisation or the sentiment analysis it is necessary to clean the data. Delete the punctuations, the URLs

``` import re
import emoji
import nltk
nltk.download('words')
words = set(nltk.corpus.words.words())
def cleaner(tweet):
    tweet = re.sub("@[A-Za-z0-9_:]+","",str(tweet)) #Remove @ sign
    tweet = re.sub(r"(?:\@|http?\://|https?\://|www)\S+", "", tweet) #Remove http links
    tweet = " ".join(tweet.split())
    tweet = ''.join(c for c in tweet if c not in emoji.UNICODE_EMOJI) #Remove Emojis
    tweet = tweet.replace("#", "").replace("_", " ") #Remove hashtag sign but keep the text
    tweet = " ".join(w for w in nltk.wordpunct_tokenize(tweet) \
         if w.lower() in words or not w.isalpha())
    return tweet
df_tweet['text_clean'] = df_tweet['text'].map(lambda x: cleaner(x))
```
# Showing tweets before cleaning
```print(df_tweet['text'])
```
# Showing tweets after cleaning

```print(df_tweet['text_clean'])
```
# Finding the optimal cluster :
```
def find_optimal_clusters(data, max_k):
    iters = range(3, max_k+1, 2)
    
    sse = []
    for k in iters:
        sse.append(KMeans(n_clusters=k, init='k-means++', max_iter=100, n_init=1).fit(data).inertia_)
        print('Fit {} clusters'.format(k))
        
    f, ax = plt.subplots(1, 1)
    ax.plot(iters, sse, marker='o')
    ax.set_xlabel('Cluster Centers')
    ax.set_xticks(iters)
    ax.set_xticklabels(iters)
    ax.set_ylabel('SSE')
    ax.set_title('SSE by Cluster Center Plot')
    
find_optimal_clusters(text, 30)
```
# And finally classify tweets according to their cluster:
```
def get_top_keywords(data, clusters):
    df = pd.DataFrame(data.todense()).groupby(clusters).mean()
    df.head()
    for i,r in df.iterrows():
        print('\nCluster {}'.format(i))
        print(df_tweet[df_tweet['clusters']==i]['text'])
            
get_top_keywords(text, clusters)
```
