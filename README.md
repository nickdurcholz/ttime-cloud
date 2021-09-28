# ttime

ttime is time tracking utility that offers both a web and command line
interface and cloud storage to keep track of all your work hours. 

ttime is intended to be a personal productivity tool, and not a company time
reporting solution. Your data is your private data, and beyond that there are
no access controls or restrictions about what you can edit or do. It shines
when you need a tool to turn a list of timestamps into a report whose only
purpose is to be copied into a more official system.

ttime is also a demo project. It doesn't need to have all of this cloud-native features built into it, but part of the point is to show off some of the skills that were listed on my resume, so if you are looking at it from that angle, skip to 

It was born out of the need to track multiple dimensions of what I had been
working on. For example, perhaps you spend a day in standup for a couple hours
working on a scheduling bug that has number 43998 in the issue tracker, a
document management system feature with story number 44001, and another
scheduling feature with yet another story number. In the real world you may be
asked to report time spent on each individual issue and also on general themes
(scheduling, document management) or even split it down by task such as
meetings or development. When you combine these reporting requirements with
real-world interruptions, meetings, and office chatter, reporting how much
time you spent on anything can be little more than a farcical guessing game.

## Tags

ttime solves this problem by allowing you to record a timestamp of when you
switched tasks and also provide a list of tags that apply to the entry. You may
start a new task with the tags 'dev scheduling 43998', 'meeting standup', or
'dev document-management 44001'. These tags are ordered and should be listed in
the order of most general to most specific in order to get the most out of
reporting, though it is not strictly necessary.

Later, when it is time to record time spent in jira or whatever system your
corporate overlords have purchased for you, it is simple to get a heirarchical
report of how much time you have actually spent on things. A report could look
something like this

```
Report from 2021-09-27 12:00:00 AM to 2021-09-28 12:00:00 AM
meeting standup         0.25
dev                     7.75
    scheduling 43998      2.12
    document-management   5.63
        44001               3.12
        44020               2.51
--------------------------------
TOTAL                   8.00
```

This report says that on 2021-09-27 you spent 0.25 hours in a standup meeting
and 7.75 hours on development. Of the 7.75 hours spent on development, you
spend 2.12 hours working on the scheduling bug and 5.63 hours split between two
different document management stories.

## Reports

Reporting is designed to be very flexible. You can obtain combined report for
an arbitrary period of time or even covering all the data you have in ttime.
Ever wonder exactly how much PTO you took last year? That information is a few
clicks or keypresses away depending on whether you want to use the web or
command line interface. You can get the following very easily:

* A report that only includes specific tags.
* A report covering yesterday, last week, last month, quarter, year, or 
  arbitrary date range
* A different report for every day in a given period

## Managing data

You can easily view or edit historical data and even export it in CSV format.
You can edit the spreadsheet and re-import it later to make mass updates. All
data is stored in a managed cloud service, so it is seamlessly available no
matter which device you are on or which interface you are using.

## Technical bits

### Authentication

The application uses Cognito to identify users and issue tokens 

### Front end
ttime contains a command line interface front end because I actually use it as
a time tracker, and that is what I prefer to use. There is a react front-end
for the rest of the world that uses ??? CSS framework blah blah

TODO Flesh out the front end

### Back end

As an application, its data fits pretty nicely into a NoSQL world, but there
isn't any reason a relational database wouldn't work. In the end, I decided to
go with DynamoDB because billing for the data tier can work out to be nearly
free for a single user.

There is a service layer that can be deployed in one of two ways -- as a
container running on EKS or as a lambda function. EKS has more demo value, but,
again, this is something that I actually use, and the billing for lambda on my
personal AWS account is more attractive long-term.

TBD Service Location / API Gateway

### Ops

There are terraform modules that can be used to stand up a new EKS cluster, and modules to deploy the back-end to either EKS or Lambda.

The front-end is hosted using AWS Amplify.