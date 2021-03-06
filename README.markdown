
# Open Microblog

**Version:** 0.5

**Last Updated:** 2015-05-23

================================================================
**Overview**
- What is the problem?
- What does Open Microblog do to solve it?
- Why not just use RSS? It's already there...
- How does it work?
- Why doesn't it have a cool name?
- Terminology

**Feed Features and Layout**
- The Feed
- Reposts
- Private Messaging
- Replies/Mentions
- Blocking
- Following
- Followers	
- Favorites

**Main Feed Layout and Elements**
- Required Channel Elements
- Optional Channel Elements
- Required Item Elements
- Optional Channel Elements
- Notes
- Sample Feed

**Alternate Feed Layout and Elements**
- Required Channel Elements
- Optional Channel Elements
- Required Item Elements
- Sample Feed

================================================================

# Overview

If you have suggestions, comments, or complaints about the Open Microblog standard, get in touch 
via our IRC channel #OpenMicroblog on freenode.

### Notes:

In the coming version update (0.6 -> 0.7) there will be a 'great renaming'. To make the microblog standard more consistent with RSS,
all microblog elements will be reformatted from snake\_case\_ to camelCase.

## What is the problem?

In recent years our internet communication has been increasingly controlled by sole private companies. Facebook and Twitter account for an enormous percentage of online communication. This isn't good. Private companies (that aren't treated as utilities) have a number of factors that prevent them from being pure communications networks. Private companies need to make money, then need to constantly make *more* money, and they need to serve changing needs. They need to upgrade their service and add "features". This means that their goals will never align entirely with the goal of open, untampered internet communication. There's one more problem: Private companies can die. They go away. Twitter and Facebook sit high now, but in 10 years? 5 years even? Will they still be around? Its hard to say. Communications that are tied to services and companies like this will die with the companies that built them. This is the problem that the Open Microblog tries to solve.

## What does Open Microblog do to solve it?

Open Microblogger is an open standard for a distributed, real-time, internet based communications service. Using a model based on RSS, Open Microblogger allows developers and users to host their own microblogging service modeled after Twitter. The biggest upside to Open Microblogger is that it isn't controlled by a central corporation or body thus it remains, like RSS, outside the realm of corporate reach.

Social Networks are the cornerstone of the modern internet world. People all over the world use them to communicate with each other. The importance of this information being tamper-free and delivered in real-time is one thing that services like Twitter have excelled at recently. However services like Twitter and Facebook are controlled by a single entity, the fate's of all the information they carry is dictated by the needs of that particular company needing to make money. This means that our communication, the things we use to share events and news around the world is controlled by a single company and it's pockets.

Information regarding world events, social injustice, and social upheaval is just too valuable to allow a single corporation to control. If Twitter disappeared tomorrow, the world would almost certainly cease to hear about events all over the world and would have to go backward to other means. The idea of Twitter, of instant access to the information you want from the source is too good to let die or be corrupted. That is the mission of Open Microblogger.

## Why not just use RSS? It's already there...

True, but the Open Microblogger Standard allows for more social interaction and functionality than the broadcast-only medium that is RSS. Open Microblogger is meant to supplement RSS as another means of communicating. You'll see further down, there is a lot that Open Microblogger can do that RSS just can't. Plus Open Microblogger aims to be as RSS compliant as possible and should be compatible in most RSS readers already.

## Is there a validator I can use?

Why yes. Included with this repository is a very simple validator written in Python.

## How does it work?

Open Microblog is an extension on the RSS XML format that models a user's interactions and status updates. A user's public data, on any given service, is laid out in 3 XML files: the user's Feed, their block list, and their following list.

Since Open Microblog is an extension of RSS all elements that are specific to the Open Microblog standard are prefixed by the namespace `microblog` and link to [http://openmicroblog.com](http://openmicroblog.com).

### The Feed

The feed is the main file that contains the connections to and information about a given user. The Feed XML file contains a list of the user's most recent posts as well as their profile information such as their username, user id, and bio information as well as a link to the public URL of the feed. At this point the Feed may sound like a typical RSS feed, and that is intended. In addition, the Feed contains a couple of important links to other pieces of the user's information; these elements are the next\_node, blocks, and follows elements. 

The first one, the next\_node element contains a link to the previous set of user posts. For fast and responsive feeds, the main Feed XML file is paginated in a standard way. For more information see Feed Paging.

#### Special Notes about the Feed

A given user wanting to subscribe to, or "Follow" another user needs only the Feed URL of the user in question. From this one URL it is possible to garner all other public information about a given user. The URL of the user's feed is the head node to which all other files link back to and branch off from.

### Blocks

The blocks XML file is linked to from the main Feed URL and contains an itemized list of the users (including user ids, usernames, and user link URLs) that the user has chosen to block. If this list becomes longer than 500 kB or 500 items, then it should be paginated as noted in the Feed Paging section.

### Follows

The follows XML file is linked to from the main Feed URL and contains an itemized list of the users (including user ids, usernames, and user link URLs) that the user has chosen to follow. If this list becomes longer than 500 kB or 500 items, then it should be paginated as noted in the Feed Paging section.

## Why doesn't it have a cool name?

I know, the name is terrible. I haven't thought of a good one yet.

## Terminology

_User_: A person/company who has an account. This may also mean the account that belongs to a given person/company.

_Client_: An application used for viewing, posting, or sharing content on this service (web site, mobile app, etc).

_Service_: An application (usually a web based service) that provides data aggregation and collection capabilities (i.e. the feed crawling servers, etc).

_Standard's Level_: Any functionality or feature-set provided by the standard with no additional work required by the service or client.

_Service's Level_: Any functionality or feature-set provided by a given service. Service Level tasks can be anything from post favoriting functionality, user search functionality, etc. In some cases (see Replies/Mentions) the Standard has formalized a set of guidelines for Service Level features/functionality.

================================================================

# Feed Layout and Features
## The Feed

Each user has a feed. This feed should follow the format listed in section 3. The feed represents the user's post history (what they've posted, reposted, and replies they've sent). This feed also contains certain metadata about the user including their username, user\_id, reply\_to information, and their most recent statuses.

Its important to remember that the feed holds the truth. There is no central database for user data, and all of a user's posts should be able to be found by traversing the feed and following public URLs found in it.

### Feed Paging

Due to the high volume of posts common to microblogs, the feed if left unchecked, could grow to be unwieldy in a short time. To combat this it is recommended that a user's feed be paginated to include no more that the user's 500 most recent posts or the most recent posts for a feed file size of up to 500 KB, whichever comes first. This should keep the file size small enough to be easily transmitted over the web. 

Once a given XML file has surpassed the recommended limits provided, it should be moved, as is (do not remove the header information, simply copy it) to a new public location. A new, empty XML file should be inserted at the user's root URL (the header information should be added at the top of this new page) and a next\_node tag should be inserted with a link to the new location of the old page. This will allow the files to remain as static as possible and the archive of all the user's posts is preserved. In practice, the user would have a multitude of these XML files all linking to each other like a singly-linked-list, each with a URL to the next node and the head node.

### Relocation, What happens if the user changes services?

The crucial feature of Open Microblog is that it is designed to be platform and service independent. Users have the ultimate choice to stop, start, relocate, and remove their data from a service at will. In order to migrate services easily, and without losing followers, services should provide a mechanism for users to input a redirect URL in their feed. This item is denoted with the relocate tag. This tag should _only_ be present if the user is relocating; it should never be empty. The value in that tag is the URL of the user's new feed. This tag should give the followers of said user adequate time to migrate over to the new feed URL before the old service deletes the user's feed.

The relocate tag should only be present in the most recently updated page of the user's main feed. It is not necessary to add the relocate tag to previous pages.

#### Obligation of the old Service

If a user is choosing to leave your service, you have an obligation to keep their old feed public for at least 24 hours to allow the user's new choice of service to import the data.

#### Obligation of the new Service

Conversely, services that the user is migrating to, should prompt the user for a link to their old feed and copy it over to  their system. The old service is not obligated to keep that old feed around for long, so make sure to get it quickly.

#### Obligation of the Client

Clients who come across a \<relocate\> tag should immediately redirect their crawlers at this new feed URL. The old URL can be discarded and is considered dead.

## Reposts

Reposting (similar to Twitter's retweeting) is the posting of someone else's post to your own timeline. The information from the original post is preserved in the reposted tags, and includes data link original user\_id, status\_id, and a link to that user's feed.

## Private Messaging

If a service supports private messaging, they will include a URL in the main feed called `message`. This URL will accept messages and pass them onto the user. The option of receiving either all private messages or only those from accounts that the user follows. 

The `message` element should contain an attribute that informs other users about their messaging policy. If the user wishes to accept messages from everyone then the `message` element should contain an attribute `accept` with a value `everyone`. Else, the element should posses the same attribute with the value `follows`. A third option is for the attribute to contain a value of `selected` if the user wishes to receive messages from selected users that they may not follow.

The `accept` attribute should inform other clients as to whether or not they should attempt to send messages to the user in question. If a client sees a value of `everyone` then the client should feel free to send messages. If the value is `follows` then it should be courteous and do the necessary checks if a user follows them before sending messages. If the value is `selected` the client should attempt the request, *but* should remember the response as to whether or not they are a whitelisted user, so as to not barrage the other clients with unnecessary messages.

### Sending Private Messages

To send messages, the URL in the `message` element should accept `HTTP POST` requests with a request body in the following JSON format:

Messages should contain the following elements:

- `user_id`: The id of the user who is sending the message.
- `username`: The username of the user sending the message.
- `user_link`: The feed URL of the user sending the message.
- `message`: The text of the message. 
- `date`: The date that the message was sent. This should be formatted according to 	RFC 822. However, in practice ISO 8601 and RFC 3339 may also be accepted.

```
POST {URL} HTTP/{version}

{'user_id':'123432', 'username':'someusername', 'user_link': 'http://example.com/someusername/feed', 'message':'hello world!', 'date':'Sat, 16 May 2015 21:32:15 UTC'}
```

### Receiving Private Messages

Upon receiving a private message at a given URL, the service should accept the request and, depending on the user's settings, return the appropriate response.

In the case of an accepted message, the server should respond with a `201 Created` code (optionally a `200 OK`) and no message body.

In the case of a denied message, when a user does not accept requests from the user named, `403 Forbidden` (optionally a `400 Bad Request`) and no message body.	

## Replies/Mentions

In the case that a given user replies to, or mentions another known user, the given user's client should alert the mentioned user's server using the provided `reply` URL inside the item that the user would be responding to (example below). The format for sending, and receiving replies or mentions is described below along with the message format.

### Example 

```xml
<item>
	<guid>3453-3433-dged-sfrf</guid>
	<description>Anyone out there?</description>
	<pubDate>Sat, 16 May 2015 21:32:15 UTC</pubDate>
	<reply>http://example.com/myusername/status/3453-3433-dged-sfrf/reply</reply>
</item>
```

### Sending Replies/Mentions

To send messages, the URL in the `reply` element should accept `HTTP POST` requests with a request body in the following JSON format:

Messages should contain the following elements:

- `user_id`: The id of the user who is sending the message.
- `username`: The username of the user sending the message.
- `user_link`: The feed URL of the user sending the message.
- `status_id`: The id of the status in the user's feed that contains the reply or mention. 

```
POST {URL} HTTP/{version}

{'user_id':'123432', 'username':'someusername', 'user_link': 'http://example.com/someusername/feed', 'status_id':'2345-643f-ggdg'}
```

### Receiving Replies/Mentions

Upon receiving a private message at a given URL, the service should accept the request and, depending on the user's settings, return the appropriate response.

In the case of an accepted message, the server should respond with a `201 Created` code (optionally a `200 OK`) and no message body.

In the case of a denied message, when a user does not accept requests from the user named, `403 Forbidden` (optionally a `400 Bad Request`) and no message body.	

## Blocking

Due to the distributed and therefore uncurated nature of this communication network some users may wish to ban others from following/messaging them. This is a service level feature. Services must implement this feature independently, however a preferred format is provided for listing accounts the user wished blocked from contacting them. This will help the user retain these blocking options if they chose to migrate to another service. An optional tag may be inserted into the user's feed which links to the file that describes the user's blocking preferences. See Additional Feeds.

Services should give the user's the option of making this feature publicly visible or not. Note that if the user's block list is private, they will not be able to transfer it to another service.

## Following 

Just like with RSS, users are given a list of statuses in their timeline of the posts of people/accounts they follow. Similarly to blocking, the implementation of Following a user is up to the service, however a preferred format is provided. An optional tag may be included in the user's main feed which links to the user's followers list. See Additional Feeds.

Services should give the user's the option of making this feature publicly visible or not. Note that, like blocking, if the user's following list is private, they will not be able to transfer it to another service.

## Followers

Followers (just like with Twitter) are people that will receive posts from a given user. A user will often have multiple followers. 

Because of the distributed nature of the system, gauging exact follower counts becomes challenging and as of version 0.1 there is no way to determine exactly the number of followers a given user has.

================================================================

# Main Feed Layout and Elements

## Required Channel Elements

- username: A Twitter-like username. Must no greater than 25 characters. 
- user\_id: A universally unique (or as close as possible) id. This identifies the user. (Think UUID)
- profile: A URL for the user's public page. This should be the human readable profile NOT the XML feed.
- link: URL to the feed. If the feed is paginated, then the link to the most recent page. 
- language: The main language for the feed. 
- lastBuildDate: The date and time that the last item was added to the feed.

## Optional Channel Elements

- docs: URL to documentation of the feed (if available).
- next\_node: A URL to the next previous set of items.
- description: A brief bio of the user. Like Twitter bios (surrounded with <\!\[CDATA\[\]\]> tags).
- user\_full\_name: The user's first and last name. Used for display purposes.
- blocks: A public URL to the XML feed of the user's block list.
	- count: An attribute of the blocks tag that lists the total number of items in the blocked users in the list.
- follows: A public URL to the XML feed of the users that a given user follows.
	- count: An attribute of the follows tag that lists the total number of users that a given user follows.
- message: An public URL that accepts JSON formatted messages (described above).
- header: an background image chosen by the user.
- portrait: a chosen profile picture of the user. 

## Required Item Elements

- guid: A unique, incrementing integer starting from 0 representing the item.
- pubdate: The datetime when the status was posted.
- description: The HTML text of the post (surrounded with <\!\[CDATA\[\]\]> tags).

## Optional Item Elements

### Accepting Replies to Statuses

- reply: The URL for replying to a given status. For more information on sending replies see the above Replies section.
 	
### Replies
 
- in\_reply\_to\_user\_id: The user\_id of the user being replied to.
- in\_reply\_to\_status\_id: The status\_id of the post that is being replied to.
- in\_reply\_to\_user\_link: A link to the feed of the user that is being replied to.

### Reposts
 
- reposted\_status\_user\_id: The user\_id of the user who originally posted the status.
- reposted\_user\_link: A link to the original user's feed.
- reposted\_status\_id: The status\_id of the original user's post.
- reposted\_status\_pubdate: The pubdate of the original user's post.

### Miscellaneous

- language: The language of the status. Recommended if the language is different than the feed language.

## Notes

- Feed should be UTF-8 Encoded.

## Sample Feed

The feed below contains _all_ the possible elements in a single feed. Keep in mind that not all of these elements will be present at one time.
``` xml
<rss version="2.0" xmlns:microblog="http://openmicroblog.com/">
	<channel>
		<!-- User Info -->
		<microblog:username></microblog:username>
		<microblog:user_id></microblog:user_id>
		<microblog:user_full_name></microblog:user_full_name>
		<description><![CDATA[]]></description>
		<microblog:header></microblog:header>
        <microblog:portrait></microblog:portrait>
        <!-- Feed Metadata -->
		<microblog:profile></microblog:profile>
		<link></link>
		<microblog:next_node></microblog:next_node>
		<microblog:relocate></microblog:relocate>
		<microblog:blocks count=""></microblog:blocks>
		<microblog:follows count=""></microblog:follows>
		<!-- Private Messaging URL -->
		<microblog:message accept="everyone"></microblog:message>
		<!-- Misc. -->
		<docs></docs>
		<language></language>
		<lastBuildDate></lastBuildDate>
		<item>
			<!-- Status Update -->
			<guid isPermalink="false"></guid>
			<pubdate></pubdate>
			<description><![CDATA[]]></description>
			<!-- Reply and Mention URL -->
			<microblog:reply></microblog:reply>
			<!-- Replying -->
			<microblog:in_reply_to_status_id></microblog:in_reply_to_status_id>
			<microblog:in_reply_to_user_id></microblog:in_reply_to_user_id>
			<microblog:in_reply_to_user_link></microblog:in_reply_to_user_link>
			<!-- Reposting -->
			<microblog:reposted_status_id></microblog:reposted_status_id>
			<microblog:reposted_status_pubdate></microblog:reposted_status_pubdate>
			<microblog:reposted_status_user_id></microblog:reposted_status_user_id>
			<microblog:reposted_status_user_link></microblog:reposted_status_user_link>
			<!-- Misc. -->
			<language>en</language>
		</item>
	</channel>
</rss>
```
================================================================

# Alternate Feed Layout and Elements

## Required Channel Elements

- username: A Twitter-like username. Must no greater than 25 characters. 
- user\_id: A universally unique (or as close as possible) id. This identifies the user.
- link: URL to the blocks or follows feed. If the feed is paginated, then the link to the most recent page. 
- lastBuildDate: The date and time that the last item was added to the feed.

## Optional Channel Elements

- next\_node: A URL to the next previous set of items.

## Required Item Elements

- user\_id: The id of the user being blocked or followed.
- user\_name: The username of the user being blocked or followed.
- user\_link: A link to the feed of the user being blocked or followed.

## Notes

- Feed should be UTF-8 Encoded.

## Sample Feed

The feed below contains _all_ the possible elements in a single feed. Keep in mind that not all of these elements will be present at one time.
``` xml
<rss version="2.0" xmlns:microblog="http://openmicroblog.com/">
	<channel count="">
		<microblog:username></microblog:username>
		<microblog:user_id></microblog:user_id>
		<link></link>
		<microblog:next_node></microblog:next_node>
		<lastBuildDate></lastBuildDate>
		<item>
            <guid isPermalink="false"></guid>
			<microblog:user_id></microblog:user_id>
			<microblog:user_link></microblog:user_link>
			<microblog:user_name></microblog:user_name>
		</item>
	</channel>
</rss>
```
