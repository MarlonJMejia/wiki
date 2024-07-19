| Explanation                  | Systemd Timer             |
|------------------------------|---------------------------|
| Every Minute                 | `*-*-* *:*:00`            |
| Every 2 minutes              | `*-*-* *:*/2:00`          |
| Every 5 minutes              | `*-*-* *:*/5:00`          |
| Every 15 minutes             | `*-*-* *:*/15:00`         |
| Every quarter hour           | `*-*-* *:*/15:00`         |
| Every 30 minutes             | `*-*-* *:*/30:00`         |
| Every half an hour           | `*-*-* *:*/30:00`         |
| Every 60 minutes             | `*-*-* */1:00:00`         |
| Every 1 hour                 | `*-*-* *:00:00`           |
| Every 2 hours                | `*-*-* */2:00:00`         |
| Every 3 hours                | `*-*-* */3:00:00`         |
| Every other hour             | `*-*-* */2:00:00`         |
| Every 6 hours                | `*-*-* */6:00:00`         |
| Every 12 hours               | `*-*-* */12:00:00`        |
| Hour Range                   | `*-*-* 9-17:00:00`        |
| Between certain hours        | `*-*-* 9-17:00:00`        |
| Every day                    | `*-*-* 00:00:00`          |
| Daily                        | `*-*-* 00:00:00`          |
| Once a day                   | `*-*-* 00:00:00`          |
| Every night                  | `*-*-* 01:00:00`          |
| Every day at 1am             | `*-*-* 01:00:00`          |
| Every day at 2am             | `*-*-* 02:00:00`          |
| Every morning                | `*-*-* 07:00:00`          |
| Every midnight               | `*-*-* 00:00:00`          |
| Every day at midnight        | `*-*-* 00:00:00`          |
| Every night at midnight      | `*-*-* 00:00:00`          |
| Every Sunday                 | `Sun *-*-* 00:00:00`      |
| Every Friday                 | `Fri *-*-* 01:00:00`      |
| Every Friday at midnight     | `Fri *-*-* 00:00:00`      |
| Every Saturday               | `Sat *-*-* 00:00:00`      |
| Every weekday                | `Mon...Fri *-*-* 00:00:00`|
| Weekdays only                | `Mon...Fri *-*-* 00:00:00`|
| Monday to Friday             | `Mon...Fri *-*-* 00:00:00`|
| Every weekend                | `Sat,Sun *-*-* 00:00:00`  |
| Weekends only                | `Sat,Sun *-*-* 00:00:00`  |
| Every 7 days                 | `* *-*-* 00:00:00`        |
| Every week                   | `Sun *-*-* 00:00:00`      |
| Weekly                       | `Sun *-*-* 00:00:00`      |
| Once a week                  | `Sun *-*-* 00:00:00`      |
| Every month                  | `* *-*-01 00:00:00`       |
| Monthly                      | `* *-*-01 00:00:00`       |
| Once a month                 | `* *-*-01 00:00:00`       |
| Every quarter                | `* *-01,04,07,10-01 00:00:00` |
| Every 6 months               | `* *-01,07-01 00:00:00`   |
| Every year                   | `* *-01-01 00:00:00`      |
