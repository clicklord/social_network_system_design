# social_network_system_design
System Design социальной сети для курса по System Design

### Functional requirements:

- публикация постов из путешествий с фотографиями, небольшим описанием и привязкой к конкретному месту путешествия
- оценка и комментарии постов других путешественников
- подписка на других путешественников, чтобы следить за их активностью
- поиск популярных мест для путешествий и просмотр постов с этих мест
- просмотр ленты других путешественников


### Non-functional requirements:

- 10 000 000 DAU/year
- availability 99,95%
- only CIS
- the workload depends on the holiday season
- upper bound on response time ~ 1c
- rate limits per user
    - create posts 7 rpm
    - read posts 300 rpm
    - create reactions 300 rpm
    - read reactions 300 rpm
- web/mobile/mobile site
- average usage by user
    - create 2 post per day
    - read 10 posts per day
    - create reactions 5 per day
    - read reactions 10 per day

## Basic calculations

RPS (create posts):

    DAU = 10 000 000
    RPS = 10 000 000 * 2 / 86 400 ~= 240

RPS (read posts):

    DAU = 10 000 000
    RPS = 10 000 000 * 10 / 86 400 ~= 1200

RPS (create reactions):

    DAU = 10 000 000
    RPS = 10 000 000 * 5 / 86 400 ~= 580

RPS (read reactions):

    DAU = 10 000 000
    RPS = 10 000 000 * 10 / 86 400 ~= 1200

Opened connections:
DAU = 10 000 000
Connections = 10 000 000 * 0.1 = 1 000 000

Data types in storage per post:
title = 100b
location = 16b
created_at = 8b
author = 8b
reaction = 2b
= 134b

    foto ~= 3Mb

    total = 3 000 134 b

Required memory:

    Replication factor = 3
    Service operation time = 5 years
    DAU per year = 10 000 000
    Created posts for 5 years = 150 000 000 * 2 * 365 ~= 10 950 000 000
    Required memory for 5 years = 134 * 10 950 000 000 ~= 146 730 000 0000 ~= 1467300 MB ~= 1.5 Tb
    With photos = 3 * 10 950 000 000 + 1467300 ~= 32 851 467 300 Mb ~= 33 Pb
    With all replics ~= 99 Pb
