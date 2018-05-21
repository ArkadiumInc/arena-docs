This is Aren4 guide should be different for Arena5 use this just as starting point
## Part 1: Client to server

### User authorization:
To authorize user from external system we expect user_id url parameter on any page. Arena saves url parameter value to session level cookies, and keeps user authorized until he closes the browser.
Example: [http://games.yourarenaname.com/?user_id=123456789abcd](http://games.yourarenaname.com/?user_id=123456789abcd)
Game progress event:

``` javascript
<script>
    ark.addListener("arkGameCompletionDataProgress", function(resObj) {
        //any code here
    });
</script>
```


When user starts the game, arena triggers the game progress event each 30 seconds, client can configure that period by setting global variable in the wrapper:

``` javascript
<script>
    window.externalApiEventPeriod = 100;//seconds
</script>
```


The resObj parameter in the event handler will have following structure:

```javascript
resObj = {
    uid: 'user_id from url parameter',
    user_ip: 'user IP',
    gameId: 'game id',
    sessionDuration: 'how many time user spent on the page',
    gameplayDuration: 'how many time user was playing game',
    adsServed: 'how many ads we showed to current user',
    avgRevenuePerSession: 'avg revenue for current gameplay based on analytics',
    avgRevenue: 'avg revenue for current gameplay based on ads impressions'
};
```




### Game end event:

```javascript
<script>
    ark.addListener("arkGameCompletionDataReady", function(resObj) {
        //any code here
    });
</script>
```


When user ends the game arena triggers game end event. The resObj parameter in the event handler will have following structure:
resObj = {
    uid: 'user_id url parameter',
    user_ip: 'user IP',
    gameId: 'game id',
    score: 'scores',
    scoreHash: 'md5(score + _ + secret), by default secret is empty string',
    sessionDuration: 'how many time user spent on the page',
    gameplayDuration: 'how many time user was playing game',
    adsServed: 'how many ads we showed to current user',
    avgRevenuePerSession: 'avg revenue for current gameplay based on analytics',
    avgRevenue: 'avg revenue for current gameplay based on ads impressions'
};

## Part 2: Server to server

### User authorization:
To authorize user from external system we expect user_id and user_name url parameters on any page. Arena saves url parameters values to session level cookies, and keeps user authorized until he closes the browser.
Example: http://games.yourarenaname.com/?user_id=123456789abcd&user_name=someName
### Game end event:
Client should provide a url for game end submits. When user ends the game, arena posts game completion data to the url. The game completion data will have following structure:

```javascript
{"SessionId", "unique id for each gameplay for each user"},
{"UserId", "user id from url parameter"},
{"GameId", "game id"},
{"StartDateTime", "DateTime when user started the game"},
{"EndDateTime", "DateTime when user ended the game"},
{"AvgRevenuePerSession", "Avg revenue for current gameplay based on analytics"},
{"AvgRevenuePerMinute", "Avg revenue for minute for current game" },
{"GameScores", "game scores"},
```

