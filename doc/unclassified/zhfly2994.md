# Joomscan

> 原文：[https://www.zhihuifly.com/t/topic/2994](https://www.zhihuifly.com/t/topic/2994)

# Joomscan

## 一、漏洞简介

joomscan

## 二、影响范围

## 三、复现过程

> https://github.com/rezasp/joomscan

### usage

```
Usage:	joomscan.pl [options]

–url | -u <URL>                |   The Joomla URL/domain to scan.

–enumerate-components | -ec    |   Try to enumerate components. `–cookie <String>               |   Set cookie.

–user-agent | -a <user-agent>  |   Use the specified User-Agent.

–random-agent | -r             |   Use a random User-Agent.

–timeout <time-out>            |   set timeout.

–about                         |   About Author

–update                        |   Update to the latest version.

–help | -h                     |   This help screen.

–version                       |   Output the current version and exit.` 
```

### usage examples

Do default checks...
`perl joomscan.pl --url www.example.com`
or
`perl joomscan.pl -u www.example.com`

Enumerate installed components...
`perl joomscan.pl --url www.example.com --enumerate-components`
or
`perl joomscan.pl -u www.example.com --ec`

Set cookie
`perl joomscan.pl --url www.example.com --cookie "test=demo;"`

Set user-agent
`perl joomscan.pl --url www.example.com --user-agent "Googlebot/2.1 (+http://www.googlebot.com/bot.html)"`
or
`perl joomscan.pl -u www.example.com -a "Googlebot/2.1 (+http://www.googlebot.com/bot.html)"`

Set random user-agent
`perl joomscan.pl -u www.example.com --random-agent`
or
`perl joomscan.pl --url www.example.com -r`

Set proxy
`perl joomscan.pl --url www.example.com --proxy http://127.0.0.1:8080`
or
`perl joomscan.pl -u www.example.com --proxy https://127.0.0.1:443`

Update Joomscan...
`perl joomscan.pl --update`