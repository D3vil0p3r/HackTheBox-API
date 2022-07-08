# HackTheBox-API
The purpose of this project is to list all the Hack The Box v4 APIs and show examples of their usage.

Remember to set your API key in a variable like `$appkey`.

The output of HTTP response is JSON. For accessing to a specific field of JSON structure, use `jq`, for example, if you need to access to a nested field named `id` inside `info` structure of the machine profile, pipe `jq .info.id`.

## User
### Get basic information of a user
```bash
curl --location --request GET "https://www.hackthebox.com/api/v4/user/profile/basic/651314" -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "profile": {
    "id": 651314,
    "sso_id": 637931,
    "name": "D3vil0per",
    "system_owns": 3,
    "user_owns": 3,
    "user_bloods": 0,
    "system_bloods": 0,
    "team": null,
    "respects": 2,
    "rank": "Script Kiddie",
    "rank_id": 2,
    "current_rank_progress": 0,
    "next_rank": "Hacker",
    "next_rank_points": 0.644,
    "rank_ownership": "3.22",
    "rank_requirement": 5,
    "ranking": 459,
    "avatar": "/storage/avatars/11ce42b0477abb4d0493250a85881662.png",
    "timezone": "UTC",
    "isVip": false,
    "isDedicatedVip": false,
    "public": false,
    "country_name": "Unknown",
    "country_code": null,
    "points": 1,
    "university": null,
    "university_name": null,
    "description": null,
    "github": null,
    "linkedin": null,
    "twitter": null,
    "website": null,
    "server": "EU Free 1"
  }
}
```
## Machine
### Get information about the **RouterSpace** machine
By name:
```bash
curl --location --request GET "https://www.hackthebox.com/api/v4/machine/profile/RouterSpace" -H "Authorization: Bearer $appkey" | jq
```
By ID:
```bash
curl --location --request GET "https://www.hackthebox.com/api/v4/machine/profile/444" -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "info": {
    "id": 444,
    "name": "RouterSpace",
    "os": "Linux",
    "active": 1,
    "retired": 0,
    "ip": "10.10.11.148",
    "points": 20,
    "static_points": 20,
    "release": "2022-02-26T19:00:00.000000Z",
    "user_owns_count": 4189,
    "root_owns_count": 4009,
    "free": true,
    "authUserInUserOwns": true,
    "authUserInRootOwns": true,
    "authUserHasReviewed": false,
    "stars": "3.2",
    "difficulty": 38,
    "avatar": "/storage/avatars/4aee57cc02f0181b22f4ccd43775f7ac.png",
    "feedbackForChart": {
      "counterCake": 341,
      "counterVeryEasy": 499,
      "counterEasy": 1148,
      "counterTooEasy": 1032,
      "counterMedium": 794,
      "counterBitHard": 273,
      "counterHard": 136,
      "counterTooHard": 54,
      "counterExHard": 11,
      "counterBrainFuck": 59
    },
    "difficultyText": "Easy",
    "isCompleted": true,
    "last_reset_time": "5 hours before",
    "playInfo": {
      "isSpawned": null,
      "isSpawning": null,
      "isActive": false,
      "active_player_count": null,
      "expires_at": null
    },
    "maker": {
      "id": 550483,
      "name": "h4rithd",
      "avatar": "/storage/avatars/fbdcb50407810e387030e8c598fd033d.png",
      "isRespected": false
    },
    "maker2": null,
    "authUserFirstUserTime": "19D 22H 10M",
    "authUserFirstRootTime": "19D 22H 59M",
    "userBlood": {
      "user": {
        "name": "szymex73",
        "id": 139466,
        "avatar": "/storage/avatars/e4bc1f843595a4f4654f328790237298.png"
      },
      "created_at": "2022-02-26 21:19:37",
      "blood_difference": "0H 19M 37S"
    },
    "userBloodAvatar": "/storage/avatars/e4bc1f843595a4f4654f328790237298.png",
    "rootBlood": {
      "user": {
        "name": "jkr",
        "id": 77141,
        "avatar": "/storage/avatars/ea81f81b11e714a38499f14923213694.png"
      },
      "created_at": "2022-02-26 21:37:02",
      "blood_difference": "0H 37M 2S"
    },
    "rootBloodAvatar": "/storage/avatars/ea81f81b11e714a38499f14923213694.png",
    "firstUserBloodTime": "0H 19M 37S",
    "firstRootBloodTime": "0H 37M 2S",
    "recommended": 0,
    "sp_flag": 0,
    "lab_server": null
  }
}
```
### Spawn a machine
```bash
curl --location --request POST "https://www.hackthebox.com/api/v4/machine/play/444" -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "message": "Playing machine."
}
```
### Stop the active machine
```bash
curl --location --request POST "https://www.hackthebox.com/api/v4/machine/stop" -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "message": "Stopped playing machine."
}
```
### Reset a machine
The machine must be active on your session.
```bash
curl --location --request POST "https://www.hackthebox.com/api/v4/vm/reset" --data '{"machine_id":478}' -H "Content-Type: application/json" -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "message":"Carpediem will be reset in 1 minute."
}
```

or

```json
{
  "message":"Faculty was not reset. User has already voted for resetting a machine."
}
```

or

```json
{
  "message":"Too many reset machine attempts. Try again later!"
}
```

### Submit a flag
The machine must be active on your session (maybe).
```bash
curl --location --request POST "https://www.hackthebox.com/api/v4/machine/own" --data '{"flag":"your-flag","id":480,"difficulty":50}' -H "Content-Type: application/json" -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "message":"Incorrect flag!",
  "status":400
}
```

### Submit a machine review
```bash
curl --location --request POST "https://www.hackthebox.com/api/v4/machine/review" --data '{"id":444,"stars":5,"headline":"Very funny machine","review":"It is not so immediate but it is very funny!"}' -H "Content-Type: application/json" -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "message":[
    {
      "id":444,
      "reviewed":true
    }
  ]
}
```

### Current active machine
```bash
curl --location --request GET https://www.hackthebox.com/api/v4/machine/active -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "info": {
    "id": 444,
    "name": "RouterSpace",
    "avatar": "/storage/avatars/4aee57cc02f0181b22f4ccd43775f7ac.png",
    "expires_at": null,
    "voting": false,
    "voted": false,
    "isSpawning": null,
    "type": "Free",
    "lab_server": "free_lab"
  }
}
```
or
```json
{
  "info": null
}
```

### List playable machines
```bash
/home/athena $ curl --location --request GET https://www.hackthebox.com/api/v4/machine/list -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "message":[
    {
      "id": 480,
      "name": "Faculty",
      "os": "Linux",
      "points": 30,
      "static_points": 30,
      "release": "2022-07-02T16:00:00.000000Z",
      "user_owns_count": 430,
      "root_owns_count": 382,
      "authUserInUserOwns": null,
      "authUserInRootOwns": null,
      "isTodo": false,
      "authUserHasReviewed": false,
      "stars": "4.6",
      "difficulty": 49,
      "feedbackForChart": {
        "counterCake": 4,
        "counterVeryEasy": 4,
        "counterEasy": 13,
        "counterTooEasy": 28,
        "counterMedium": 76,
        "counterBitHard": 20,
        "counterHard": 18,
        "counterTooHard": 2,
        "counterExHard": 0,
        "counterBrainFuck": 1
      },
      "avatar": "/storage/avatars/8e579b42d4f769a16ca5dde6feae8df4.png",
      "difficultyText": "Medium",
      "playInfo": {
        "isSpawned": null,
        "isSpawning": null,
        "isActive": false,
        "active_player_count": null,
        "expires_at": null
      },
      "free": true,
      "maker": {
        "id": 36994,
        "name": "gbyolo",
        "avatar": "/storage/avatars/24065805b9772892677a4c724c0a3e38.png",
        "isRespected": false
      },
      "maker2": null,
      "recommended": 0,
      "sp_flag": 0,
      "easy_month": 0,
      "ip": "10.10.11.169"
    },
    <SNIP>
  ]
}
```

### List retired machines
```bash
/home/athena $ curl --location --request GET https://www.hackthebox.com/api/v4/machine/list/retired -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "message":[
    {
      "id": 469,
      "name": "BackendTwo",
      "os": "Linux",
      "points": 0,
      "static_points": 30,
      "release": "2022-05-02T07:00:05.000000Z",
      "user_owns_count": 265,
      "root_owns_count": 187,
      "authUserInUserOwns": null,
      "authUserInRootOwns": null,
      "isTodo": false,
      "authUserHasReviewed": false,
      "stars": "5.0",
      "difficulty": 45,
      "feedbackForChart": {
        "counterCake": 15,
        "counterVeryEasy": 17,
        "counterEasy": 48,
        "counterTooEasy": 55,
        "counterMedium": 84,
        "counterBitHard": 41,
        "counterHard": 18,
        "counterTooHard": 7,
        "counterExHard": 3,
        "counterBrainFuck": 4
      },
      "avatar": "/storage/avatars/34306c6f7380f2c2c86b18b0be502326.png",
      "difficultyText": "Medium",
      "playInfo": {
        "isSpawned": null,
        "isSpawning": null,
        "isActive": false,
        "active_player_count": null,
        "expires_at": null
      },
      "free": false,
      "maker": {
        "id": 3769,
        "name": "ippsec",
        "avatar": "/storage/avatars/ebf175f982582aae191f6f34d7bce8ec.png",
        "isRespected": false
      },
      "maker2": null,
      "recommended": 0,
      "sp_flag": 0,
      "easy_month": 0,
      "ip": "10.10.11.162",
      "tags": []
    },
    <SNIP>
  ]
}
```

### List release-scheduled machines
```bash
/home/athena $ curl --location --request GET https://www.hackthebox.com/api/v4/machine/unreleased -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "data": [
    {
      "id": 481,
      "name": "RedPanda",
      "os": "Linux",
      "avatar": "/storage/avatars/0ba23d9bbfea967268e284e85e0837ff.png",
      "release": "2022-07-09T19:00:00.000000Z",
      "difficulty": 25,
      "difficulty_text": "Easy",
      "firstCreator": [
        {
          "id": 25507,
          "name": "Woodenk",
          "avatar": "/storage/avatars/f57fba21af53f3bf45e0163352c7028e.png"
        }
      ],
      "coCreators": [],
      "retiring": {
        "difficulty_text": "Easy",
        "avatar": "/storage/avatars/4aee57cc02f0181b22f4ccd43775f7ac.png",
        "os": "Linux",
        "name": "RouterSpace",
        "id": 444
      }
    },
    {
      "id": 482,
      "name": "Extension",
      "os": "Linux",
      "avatar": "/storage/avatars/c7a8b48f7e95745413c32fcb99cc1f1a.png",
      "release": "2022-07-16T19:00:00.000000Z",
      "difficulty": 75,
      "difficulty_text": "Hard",
      "firstCreator": [
        {
          "id": 476556,
          "name": "irogir",
          "avatar": "/storage/avatars/6f0ae07db6e33c60145bfdccb66aa4d7.png"
        }
      ],
      "coCreators": [],
      "retiring": {
        "difficulty_text": "Hard",
        "avatar": "/storage/avatars/84ab22908d5dd51ab6bd64f99f81b7be.png",
        "os": "Windows",
        "name": "Acute",
        "id": 438
      }
    }
  ],
  "links": {
    "first": "https://www.hackthebox.com/api/v4/machine/unreleased?unreleased=1&page=1",
    "last": "https://www.hackthebox.com/api/v4/machine/unreleased?unreleased=1&page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 1,
    "links": [
      {
        "url": null,
        "label": "&laquo; Previous",
        "active": false
      },
      {
        "url": "https://www.hackthebox.com/api/v4/machine/unreleased?unreleased=1&page=1",
        "label": "1",
        "active": true
      },
      {
        "url": null,
        "label": "Next &raquo;",
        "active": false
      }
    ],
    "path": "https://www.hackthebox.com/api/v4/machine/unreleased",
    "per_page": 15,
    "to": 2,
    "total": 2
  }
}
```

### List ToDo machines
```bash
/home/athena $ curl --location --request GET https://www.hackthebox.com/api/v4/machine/todo -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
  "info": [
    {
      "id": 480,
      "name": "Faculty",
      "os": "Linux",
      "points": 30,
      "static_points": 30,
      "authUserInUserOwns": null,
      "authUserInRootOwns": null,
      "authUserFirstUserTime": null,
      "authUserFirstRootTime": null,
      "stars": "4.6",
      "difficulty": 49,
      "feedbackForChart": {
        "counterCake": 4,
        "counterVeryEasy": 4,
        "counterEasy": 13,
        "counterTooEasy": 28,
        "counterMedium": 76,
        "counterBitHard": 20,
        "counterHard": 18,
        "counterTooHard": 2,
        "counterExHard": 0,
        "counterBrainFuck": 1
      },
      "avatar": "/storage/avatars/8e579b42d4f769a16ca5dde6feae8df4.png",
      "difficultyText": "Medium",
      "playInfo": {
        "isSpawned": null,
        "isSpawning": null,
        "isActive": false,
        "active_player_count": null,
        "expires_at": null
      },
      "free": true,
      "release": "2022-07-02T16:00:00.000000Z",
      "user_owns_count": 431,
      "root_owns_count": 382,
      "recommended": 0
    }
  ]
}
```
or
```json
{
  "info": []
}
```

### Add or remove machine to/from the ToDo list
Note: the adding and the removing to/from ToDo list is the same request.
```bash
/home/athena $ curl --location --request POST https://www.hackthebox.com/api/v4/machine/todo/update/480 -H "Authorization: Bearer $appkey" | jq
```
Response adding a machine to the ToDo list:
```json
{
   "info":[
      {
         "id":480,
         "name":"Faculty",
         "os":"Linux",
         "points":30,
         "static_points":30,
         "authUserInUserOwns":null,
         "authUserInRootOwns":null,
         "authUserFirstUserTime":null,
         "authUserFirstRootTime":null,
         "stars":"4.6",
         "difficulty":49,
         "feedbackForChart":{
            "counterCake":4,
            "counterVeryEasy":4,
            "counterEasy":13,
            "counterTooEasy":28,
            "counterMedium":76,
            "counterBitHard":20,
            "counterHard":18,
            "counterTooHard":2,
            "counterExHard":0,
            "counterBrainFuck":1
         },
         "avatar":"\/storage\/avatars\/8e579b42d4f769a16ca5dde6feae8df4.png",
         "difficultyText":"Medium",
         "playInfo":{
            "isSpawned":null,
            "isSpawning":null,
            "isActive":false,
            "active_player_count":null,
            "expires_at":null
         },
         "free":true,
         "release":"2022-07-02T16:00:00.000000Z",
         "user_owns_count":431,
         "root_owns_count":383,
         "recommended":0
      }
   ]
}
```
Response removing a machine from the ToDo list:
```json
{
  "info": []
}
```

## Leaderboard
### Ranking by user (Hall of Fame Top 100)
```bash
curl --location --request GET https://www.hackthebox.com/api/v4/rankings/users -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
   "status":true,
   "data":[
      {
         "rank":1,
         "points":2926,
         "root_owns":221,
         "user_owns":221,
         "challenge_owns":221,
         "root_bloods":25,
         "user_bloods":14,
         "challenge_bloods":5,
         "fortress":40,
         "endgame":21,
         "id":13569,
         "name":"xct",
         "avatar_thumb":"\/storage\/avatars\/8c9faea377de064aa1c5333355e4ac14_thumb.png",
         "country":"DE",
         "level":"Omniscient",
         "ranks_diff":0
      },
      {
         "rank":2,
         "points":2925,
         "root_owns":163,
         "user_owns":163,
         "challenge_owns":298,
         "root_bloods":1,
         "user_bloods":1,
         "challenge_bloods":7,
         "fortress":40,
         "endgame":21,
         "id":275298,
         "name":"m4cz",
         "avatar_thumb":"\/storage\/avatars\/e02601e7f4cb3dce6f3744254dcc4f7d_thumb.png",
         "country":"DE",
         "level":"Omniscient",
         "ranks_diff":0
      },
      <SNIP>
```

### Ranking by country (Top 100)
```bash
curl --location --request GET https://www.hackthebox.com/api/v4/rankings/country/US/members -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
   "status":true,
   "data":{
      "country_name":"United States",
      "rankings":[
         {
            "id":32580,
            "points":2280,
            "root_owns":106,
            "user_owns":106,
            "challenge_owns":128,
            "root_bloods":0,
            "user_bloods":0,
            "challenge_bloods":0,
            "fortress":40,
            "endgame":21,
            "name":"batemanmd",
            "country":"US",
            "level":"Guru",
            "rank":1,
            "ranks_diff":1,
            "avatar_thumb":"\/storage\/avatars\/b5a1f343f331d0337143ec9e3b57322c_thumb.png"
         },
         {
            "id":288520,
            "points":1511,
            "root_owns":170,
            "user_owns":172,
            "challenge_owns":64,
            "root_bloods":0,
            "user_bloods":0,
            "challenge_bloods":0,
            "fortress":40,
            "endgame":21,
            "name":"j88001",
            "country":"US",
            "level":"Guru",
            "rank":2,
            "ranks_diff":1,
            "avatar_thumb":"\/storage\/avatars\/fd878cdaf97e82280ec6862c7d5f4c9f_thumb.png"
         }
      },
      <SNIP>
```

### Ranking by team (Top 100)
```bash
curl --location --request GET https://www.hackthebox.com/api/v4/rankings/teams -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
   "status":true,
   "data":[
      {
         "rank":1,
         "points":3177,
         "root_owns":289,
         "user_owns":289,
         "challenge_owns":276,
         "root_bloods":24,
         "user_bloods":29,
         "challenge_bloods":45,
         "fortress":40,
         "endgame":21,
         "id":1709,
         "name":"WinBARs",
         "avatar_thumb_url":"https:\/\/www.hackthebox.com\/storage\/teams\/52d080a3e172c33fd6886a37e7288491_thumb.jpg",
         "country":"US",
         "ranks_diff":0
      },
      {
         "rank":2,
         "points":3143,
         "root_owns":272,
         "user_owns":273,
         "challenge_owns":298,
         "root_bloods":84,
         "user_bloods":66,
         "challenge_bloods":26,
         "fortress":40,
         "endgame":21,
         "id":1750,
         "name":"TheATeam",
         "avatar_thumb_url":"https:\/\/www.hackthebox.com\/storage\/teams\/6a5dfac4be1502501489fc0f5a24b667_thumb.jpg",
         "country":"EU",
         "ranks_diff":0
      },
      <SNIP>
```

### Ranking by university (Top 100)
```bash
curl --location --request GET https://www.hackthebox.com/api/v4/rankings/universities -H "Authorization: Bearer $appkey" | jq
```
Response:
```json
{
   "status":true,
   "data":[
      {
         "rank":1,
         "students":48,
         "points":3050,
         "root_owns":228,
         "user_owns":231,
         "challenge_owns":221,
         "root_bloods":4,
         "user_bloods":2,
         "challenge_bloods":0,
         "fortress":50,
         "endgame":21,
         "id":24,
         "name":"WMG, University of Warwick",
         "country":"GB",
         "ranks_diff":0
      },
      {
         "rank":2,
         "students":9,
         "points":2161,
         "root_owns":171,
         "user_owns":181,
         "challenge_owns":145,
         "root_bloods":0,
         "user_bloods":0,
         "challenge_bloods":0,
         "fortress":50,
         "endgame":0,
         "id":299,
         "name":"Epitech Toulouse",
         "country":"FR",
         "ranks_diff":0
      },
      <SNIP>
```

## VPN Connection
### Switching to a specific VPN Server
**Starting Labs**
| VPN Server | ID |
| ---------- | -- |
| EU Starting Point 1 | 412 |
| US Starting Point 1 | 414 |

**Lab Machines**
| VPN Server | ID |
| ---------- | -- |
| EU Free 1 | 1 |
| EU Free 2 | 201 |
| EU Free 3 | 253 |
| US Free 1 | 113 |
| US Free 2 | 202 |
| US Free 3 | 254 |
| AU Free 1 | 177 |
| SG Free 1 | 251 |

Example: switch to the EU Free 1 server
```bash
curl --location --request POST "https://www.hackthebox.com/api/v4/connections/servers/switch/1" -H "Authorization: Bearer $appkey" | jq
```
Response:
```
{
  "status": true,
  "message": "VPN Server switched",
  "data": {
    "id": 1,
    "friendly_name": "EU Free 1",
    "current_clients": 35,
    "location": "EU"
  }
}
```
### Download the OVPN file of a specific VPN server

Example: get the OPVN file of US Free 1. Before getting it, you need to switch to US Free 1 VPN server
```bash
curl --location --request POST "https://www.hackthebox.com/api/v4/connections/servers/switch/113" -H "Authorization: Bearer $appkey" | jq
curl --location --request GET "https://www.hackthebox.com/api/v4/access/ovpnfile/113/0" -H "Authorization: Bearer $appkey" -o lab-vpn.ovpn
```

## Example Script
```bash
#!/bin/sh

echo "Type the machine name please:"
read machine-name

api=your-api-key>"

machine-id=$(curl --location --request GET "https://www.hackthebox.com/api/v4/machine/profile/$machine-name "Authorization: Bearer $api" | jq .info.id)

echo "Which action to take on machine?"
echo "1 - Start Machine"
echo "2 - Stop Machine"
echo "3 - Quit"
read action;
case $action in
  1) echo "Starting machine...";echo "$machineid";curl --location --request POST "https://www.hackthebox.com/api/v4/machine/play/$machine-id" -H "Authorization: Bearer $api";;
  2) echo "Stop machine...";curl --location --request POST "https://www.hackthebox.com/api/v4/machine/stop" -H "Authorization: Bearer $api";;
  3) echo "Quitting...";exit 0;;
  *) echo "This action is not available. Please choose a different one.";; 
esac
```
