# dataminig_tweeter
# Objectifs: 
•Maitriser l’API de twitter pour l’extraction des tweets
•Maitriser la partie NLP (naturallanguageprocessing) avec NLTK en Python
•Appliquer les principes de nettoyage des données
•Classer les tweets

# Introduction :
Twitter is an American microblogging and social networking service on which users post and interact with messages known as "tweets".
It is a good ressource to collect data. We can find a few libraries (Python) which allow you to build your own dataset with the data generated by Twitter.

# Steps :
## Creating a developper account
## Importing twipy and getting authentification keys:

```from tweepy import OAuthHandler
from tweepy import API

# Consumer key authentication
auth = OAuthHandler("qIwnNNJMkjBzFGXbvIfDCx5L1", "qBDTDdhi0PtgPg2SBIdZGOE5KpXqcjjQHdMXIXazbfa6wkxpb2")

# Access key authentication
auth.set_access_token("1338511495739674629-uCRKkRhkSBdUJO45rzkn4am36WEM4w", "889fsak4U775JstxLLgfaUcrEVYoJJS7ikBUUQNaMl5m4")

# Set up the API with the authentication handler
api = API(auth)```



