App Engine application - a cloud-based API server to support a provided conference organization application that exists on the web.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.

##Task 1
###Explain in a couple of paragraphs your design choices for session and speaker implementation.

###Sessions
Session model object is created to represent session entry in the datastore.
SessionForm message object is created to handle outbound form message (user input).

Session entities are created as a child of Conference entities. Each Conference
entity has a ancestor relationship with session entities.

###Speaker
Speakers are created as entities.

##Task 3
###Describe the purpose of 2 new queries and write the code that would perform them
The two new queries I have created are "getConferenceSessionsByLocation" and
"getSessionsInWishlistByType". "getConferenceSessionsByLocation" is used to
get all sessions by area. For eg if a conference is happening in a city we
could have multiple sessions and they might be happening at different
locations. If you are attending a session at one of the session centers you
could query for all the sessions happening near-by and attend if they are
of interest to you. "getSessionsInWishlistByType" is used to get all the
sessions in whishlist by type. User can query for type of sessions that
they are interested in.

###How would you handle a query for all non-workshop sessions before 7 pm?
1. Filter query by typeOfSession for all non-workshop sessions
2. Filter query by startTime of the session to less than and equal to 7 pm

###What is the problem for implementing this query?
The Datastore enforces some restrictions on queries. Using inequalities for
multiple properties are currently disallowed. Therefore you cannot filter
by typeOfSession and startTime.

###What ways to solve it did you think of?
I have created "getConferenceSessionsByQuery" endpoints method for this query.

1. Firstly, I filter typeOfSession for all non-workshop sessions and fetch
the results.

2. Then I use a for loop to iterate over the result and create a list of
sessions for all the sessions before 7pm

[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
