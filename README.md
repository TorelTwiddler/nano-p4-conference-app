App Engine application for the Udacity training course.

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


## Sessions and Speakers

A Session is a time, place and description that occurs during a conference. A speaker is a StringProperty on the Session.

I did not implement a Speaker model to prevent unnecessary complexity to the app. In the current format, it would be trivial to search for the speaker as a string, where using a Model would add complexity without any benefit at this point. If more logic is requested for handling Speakers, then a Speaker model could be argued for.


## Additional Queries: getSessionsToday and getSessionsTodayInWishlist

getSessionsToday: Gets a list of sessions that are scheduled for today that are in conferences that the user is registered to.

getSessionsTodayInWishlist: Gets a list of sessions that are scheduled for today (and registered) that are on the user's wishlist.

## Task 3: Multiple Inequalities:

Google App Engine Datastore does not support having filters with inequalities on more than one property. This
means that a normal query of something like:

    sessions = Session.query(Session.typeOfSession != 'workshop', Session.startTime < seven_pm).fetch()

will not work.

There are two simple ways to get around this limitation. The first is to perform two queries, and find the intersection
of the results.

    sessions1 = {session.key, session for session in Session.query(Session.typeOfSession != 'workshop').fetch()}
    sessions2 = {session.key, session for session in Session.query(Session.startTime < seven_pm).fetch()}
    keys_in_both = set(session1.keys()).intersection(set(session2.keys))
    sessions = [session1[key] for key in keys_in_both]

The downsides to this method are that it requires putting the entire query in memory which may not work for large
queries.

An alternative solution is to apply the second filter locally.

    sessions = Session.query(Sessions.typeOfSession != 'workshop').fetch()
    sessions = [session for session in sessions if session.startTime < seven_pm]

The downsides to this method are that pagination would be difficult, and that many filters may drastically
slow down the query.

[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
