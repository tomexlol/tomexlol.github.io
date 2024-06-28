---
layout: default
title: "League of Legends Coaching Tool"
description: ""
category: 
tags: []
---
{% include JB/setup %}



# League of Legends Coaching Tool

## TL;DR

- **Node.js(Express) backend**: Queries, parses, stores, and serves GRID API data for scrim games.
- **Python(Flask) backend**: Queries, parses, stores, and serves Riot API data for solo queue games. Sends progress updates via WebSocket.
- **React frontend**: Displays data with custom-made components designed for optimal utility.
- **Deployment**: Everything runs behind Nginx on an EC2 Ubuntu instance. Cron jobs automatically update data from third-party APIs.
- **Real-world impact**: The tool proved valuable enough that another professional team paid to use it.

## The Details

### Scrim Data
![](../../../assets/images/scrimdata2)
Scrim data comes as JSONs directly from the GRID GraphQL API via a fully automated pipeline.

A cronjob kicks off the entire process every 3 hours by curling to our Node.js (Express) backend. This backend performs a large query for the team's most recent games, followed by smaller queries for the details of each game not yet saved in our database.

It parses the enormous JSON returned for each game into a more manageable format containing everything needed to populate our frontend with data.

Due to time constraints (I coded this entire project in a month while also coaching my team full-time), these smaller JSON files are saved to disk. The Express backend serves as an API for these files, handling the frontend's requests through file operations.

There's already a Redis instance running on the server (currently housing some config data) which could likely accommodate all this game data without issues, but I haven't gotten around to migrating it yet.

And no, I won't use a 'real database' for a web app made for 3 users that stores 300 small JSONs. I don't really think it's needed xd Anyway...

One large file contains every game's "overview", used to build this massive table without reading through hundreds of files. The table features several cool filters, including some custom-made ones. You can toggle columns, and it filters the table in real-time. Yay React!

### Game Details
![](../../../assets/images/gamedetials)
A small file for each game contains its "details", used to build this cool gameDetails view. I crafted all this CSS with my own two hands; it was quite complex. (Pro tip: Don't open it on your phone.)

### Custom Components

I created two specialized components that help you analyze your filtered data. These were born out of my own needs as a professional coach and are specifically designed to aid and streamline the weekly preparation all teams undertake before their competitive matches.

#### Statsbox
![](../../../assets/images/statsbox2)
This component displays the champions played, by us or against us. It sorts them by role and by most played, and includes their win rates. It's an easy-to-use and quick tool for getting an overview of a filtered slice of practice games. I used this component the most out of the entire app.

#### Matchup Explorer
![](../../../assets/images/matchup2)
This shows detailed info about head-to-head champion matchups. It's another valuable tool for gaining a more granular understanding of what worked and what didn't during practice. I would have used this a lot as well if I hadn't coded it after my tournament was over.

### Solo Queue Dashboard

The solo queue dashboard communicates with the Flask backend. I chose Flask because I was already familiar with a Python wrapper for the Riot API, making it faster to ship even if it meant having two separate backends.

When you click 'refresh' for a player, the Flask backend queries the Riot Games API for all games played by that player in the past week. It parses the data, stores it in a shelf file, and serves it from there to the frontend. It works, but it's not ideal - this really should be a database or moved to Redis.

The solo queue dashboard is currently the only part that interacts with our local Redis instance: each player's account names are stored there, configurable through a settings page. (I won't show it because it's a work in progress, but trust me, it exists.)

Many of these features were deprioritized during development to ship as quickly as possible, as it was a tool just for myself and my fellow staff members. I later began polishing them for a more "commercial version", but got sidetracked.

The solo queue dashboard is an excellent way for coaches to track each player's individual practice. It's also designed to foster in-house competition. Players are displayed side by side, showing their rank and games played for all teammates to see. A small 'awards' component at the top of the page highlights the player with the most games played and the highest rank for the week.

Because the backend has to query each game's details and the Riot API has very aggressive rate limiting, I wanted a way to show progress. This dashboard uses socket.io for WebSocket connections. When you click "Refresh Data", the button displays each game's ID number as it loads into the shelf.

### Authentication

I rolled my own auth because I wanted to learn. It's pretty basic, but it gets the job done. The app doesn't even render if you don't have the right JWT - it just shows you a "enter password" box. The password is hardcoded in the backend. Old school, I know, but hey, it works when you have 3 users.

When you get the password right, the Express backend sends back a signed token, which gets tucked away in localStorage. I did my homework and read up on all the fancy stuff - refresh tokens, auth expiry, slapping auth tokens on every API route. But honestly? I got a bit bored and didn't see it as necessary yet. I mean, who's gonna hack a League of Legends coaching tool from a Latin American team, right?

I even read all about OAuth2 and even played a bit with it on a fresh project. But then I remembered I didn't actually have any more clients knocking on my door, so I kept it simple. The basic auth is there just in case some player leaks the link while streaming.

## Closing Thoughts

This project, built under tight time constraints, showcases my ability to quickly deliver a functional, multi-faceted application. It demonstrates my skills in full-stack development, API integration, data parsing and visualization, and creating tailored solutions for specific user needs. While there's room for optimization, the application successfully serves its purpose, providing valuable insights for professional esports coaching teams.

Initially developed for internal use, the tool's effectiveness caught the attention of another professional League of Legends team, who paid for access to it. This not only validated the project's utility but also demonstrated its broader potential in the esports coaching space. The fact that it attracted a paying client despite its rapid development speaks to its practicality and the real-world problem it solves, showing that even in its initial form, the application provided tangible value to industry professionals. It is certainly a project I will continue developing further in the future.