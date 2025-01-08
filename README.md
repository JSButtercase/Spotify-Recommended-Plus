# Spotify-Recommended-Plus
Finds all of a given users 'Daily Mix' and 'Weekly Mix' playlists &amp; merges them with Liked Songs playlist, saves them in a database and then displays them to a private localhost page with breakdowns on bpm, mood, genre etc and recommendations

A general step by step of the process is below

#  1) Login to any spotify account via the associated API 
#  2) Find and Store 'My Daily Mix' playlists & 'Liked Songs' playlist in a NoSQL database
#  3) Database will have columns approximately based on the below, as stripped from the songs themselves:
#        rowId, Song name, genre, length, artist(s), album, bpm, mood, liked (yes/no) etc
#  4) This information will then be displayed to the user on a localhost webpage with filtering available to group songs
#
# Once the database(s) are populated with scraped user song data, stage two is a separate tool that does the following:
#   The purpose is to find new songs similar to a subset of songs and of a specific bpm / genre / mood specified by the user
#   1) Receive a user input and break into two lists of songs and list of requirements (for blues genre, 130-140 bpm, released within last 6 months, etc)
#   2) Return a playlist of potential interesting new songs that match >x% of the requirements, where the strictness of the search is configurable (at least 30% of requirements met, at least 80%, 100%):
#       a) The first step is to search the given songs list and determine data analytics on the collective. Bpm variance, how many match x mood, average length. 
#          Priority weighting given to the analytics that match with the features of the second list of requirements (ie if the user wants blues under 120bpm, this has higher weighting than length, artist, etc)
#       b) The method of finding music will be using the 'playlist radio' feature - Take the list of songs and for each song in the list, every combination of 3songs in the list and the entire list, run radio and filter
#          according to x requirements met. 
#       c) The user can dynamically like or dislike new songs in the 'potential playlist' to update the system if the songs found were in line with what the user wanted. 

#   3) Change liked / unliked to sliding scale from 0-100 in increments of 5, where 25-40 is disliked, 40-60 neutral, 60-75 liked, and 75+ or 25- indicates a stronger like / dislike 
#       a) Liked songs stay in liked, greatly liked songs are weighted stronger (slightly)
#       b) Disliked songs will be checked against requirements and used to train the system. Greatly Disliked songs will also be able to be purged from all playlists, and won't be added to future potential lists
#       c) Neutral songs will be left alone
