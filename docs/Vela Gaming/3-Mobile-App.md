---
tags: [Vela Gaming, Mobile App]
---

![Vela Gaming](../../assets/images/vela_gaming/logo.jpg)

# Mobile App

## Overview

This document describes the service integration between **Gaming (provider)** and an **operator**. The following chapters describe the general concept of integration as well as descriptions and examples of the API methods used for the service integration.

### General Notes

- Data Format: JSON (JavaScript Object Notation)
- Wallet Option: Separate Wallet
- **Operator** - Referred as the operator
- **Provider** - Referred as the game provider
- **Host ID** - This Host ID is an unique token generated from game provider for each game operator
- **Points** - Referred as the game points; not player's actual wallet balance.
- The operator manages the user account database (personal information, balance, wallet operations, etc.)
- The Provider only manages the player data necessary to perform game operation.
- All methods support uses **HTTPS GET** verb in the system.
- Players perform all betting and gaming by using VG Game Server's wallet system. Therefore, **operator** need to integrate VG API.
- Funds are deposited and witdrawn to the game provider with API calls.

### Getting started

To be able to connect to our game server, **operator** needs to provide the following API for us to communicate between game server and operator site. Players perform all betting and gaming by using VG Game Server's wallet system.

- [Create Player](#create-player)
- [Change Password](#change-password)
- [Get Balance](#get-balance)
- [Deposit](#deposit)
- [Withdraw](#withdraw)
- [Suspend](#suspend)
- [Unsuspend](#unsuspend)
- [Game List](#game-list)
- [Game Launch](#game-launch)
- [User Report](#user-report)
- [Generate Access Token]($access_token)

### Game Launch Process

- When a player launches the game, it will call the **Operator's Authenticate** API.
- Operator needs to provide us the **Authenticate** API.
- The **Authenticate** API is used by VG Game Server to retrieve players' information from the operator.
- Authenticate API is called upon launching the game for the logged-in player.

![Diagram](../../assets/images/vela_gaming/game_launch_diagram.jpg)

* * *

## API 
### Create Player

To create a new player account, the create player API is called.

<!--
type: tab
title: Docs
-->

### Request

| Name      | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| host_id   | string | Unique ID of Operator System (provided by game provider) |
| member_id | string | Unique ID / Username of the player                   |
| currency  | string | Currency code is referred to ISO 4217                              |
| password  | string | Player login password                                |

> ##### Example
>
> [https://{PROVIDER_API_ENDPOINT}/api/user/create?host_id={host_id}&member_id={member_id}\&currency=MYR&password={password}](https://{PROVIDER_API_ENDPOINT}/api/user/create?host_id={host_id}&member_id={member_id}&currency=MYR&password={password})

### Response

| Name        | Type   | Description                                 |
| ----------- | ------ | ------------------------------------------- |
| status_code | int    | Response status code                        |
| balance     | uint64 | Current **POINTS** of the player (in cents) |

#### Status Code

| Code | Description       |
| ---- | ----------------- |
| 0    | Success           |
| 1    | Invalid Member ID |
| 2    | Invalid Host ID   |
| 3    | Invalid Currency  |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "balance": 100000
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID"
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/create",
  "query": {
    "host_id": "{host_id}",
    "member_id": "{member_id}",
    "currency": "MYR",
    "password": "{password}"
  }
}
```

<!-- type: tab-end -->

### Change Password

To change the password for player account, the change password API is called.

<!--
type: tab
title: Docs
-->

### Request

| Name      | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| host_id   | string | Unique ID of Operator System (provided by game provider) |
| member_id | string | Unique ID / Username of the player                   |
| password  | string | Player login password                                |

> ##### Example
>
> <https://{PROVIDER_API_ENDPOINT}/api/user/change-password?host_id={host_id}&member_id={member_id}&password={password}>

### Response

| Name        | Type   | Description          |
| ----------- | ------ | -------------------- |
| status_code | int    | Response status code |
| message     | string | Response message     |

#### Status Code

| Code | Description       |
| ---- | ----------------- |
| 0    | Success           |
| 1    | Invalid Member ID |
| 2    | Invalid Host ID   |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "message": "Password change successfully"
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID"
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/change-password",
  "query": {
    "host_id": "{host_id}",
    "member_id": "{member_id}",
    "password": "{password}"
  }
}
```

<!-- type: tab-end -->

### Get Balance

To query the balance of a particular player account, the get balance API is called.

<!--
type: tab
title: Docs
-->

### Request

| Name      | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| host_id   | string | Unique ID of Operator System (provided by game provider) |
| member_id | string | Unique ID / Username of the player                   |

> ##### Example
>
> <https://{PROVIDER_API_ENDPOINT}/api/user/balance?host_id={host_id}&member_id={member_id}>

### Response

| Name        | Type   | Description                                 |
| ----------- | ------ | ------------------------------------------- |
| status_code | int    | Response status code                        |
| balance     | uint64 | Current **POINTS** of the player (in cents) |

#### Status Code

| Code | Description       |
| ---- | ----------------- |
| 0    | Success           |
| 1    | Invalid Member ID |
| 2    | Invalid Host ID   |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "balance": 100000
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID"
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/balance",
  "query": {
    "host_id": "{host_id}",
    "member_id": "{member_id}",
  }
}
```

<!-- type: tab-end -->

### Deposit

To transfer funds into the player account, the deposit API is called.

<!--
type: tab
title: Docs
-->

### Request

| Name      | Type       | Description                                          |
| --------- | ---------- | ---------------------------------------------------- |
| host_id   | string     | Unique ID of Operator System (provided by game provider) |
| member_id | string     | Unique ID / Username of the player                   |
| amount    | uint64     | Total **POINTS** to be transferred (in Cents)        |
| transid   | string(52) | Unique transaction ID from operator                  |

> If player's transfer amount is 1000 points, amount is 100000 (in Cents)
>
> ##### Example
>
> <https://{PROVIDER_API_ENDPOINT}/api/user/deposit-v2?host_id={host_id}&member_id={member_id}&amount=10000>

### Response

| Name        | Type   | Description                                 |
| ----------- | ------ | ------------------------------------------- |
| status_code | int    | Response status code                        |
| balance     | uint64 | Current **POINTS** of the player (in cents) |

#### Status Code

| Code | Description                |
| ---- | -------------------------- |
| 0    | Success                    |
| 1    | Invalid Member ID          |
| 2    | Invalid Host ID            |
| 5    | Invalid transid            |
| 6    | This transid has been used |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "balance": 100000
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID"
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/deposit-v2",
  "query": {
    "host_id": "{host_id}",
    "member_id": "{member_id}",
    "amount": "10000",
    "transid": "{transid}"
  }
}
```

<!-- type: tab-end -->

### Withdraw

To transferring funds out of the player account, the withdraw API is called.

<!--
type: tab
title: Docs
-->

### Request

| Name      | Type       | Description                                          |
| --------- | ---------- | ---------------------------------------------------- |
| host_id   | string     | Unique ID of Operator System (provided by game provider) |
| member_id | string     | Unique ID / Username of the player                   |
| amount    | uint64     | Total **POINTS** to be transferred (in Cents)        |
| transid   | string(52) | Unique transaction ID from operator                  |

> If player's transfer amount is 1000 points, amount is 100000 (in Cents)
>
> ##### Example
>
> <https://{PROVIDER_API_ENDPOINT}/api/user/withdraw-v2?host_id={host_id}&member_id={member_id}&amount=10000&transid={transid}>

### Response

| Name        | Type   | Description                                 |
| ----------- | ------ | ------------------------------------------- |
| status_code | int    | Response status code                        |
| balance     | uint64 | Current **POINTS** of the player (in cents) |

#### Status Code

| Code | Description                |
| ---- | -------------------------- |
| 0    | Success                    |
| 1    | Invalid Member ID          |
| 2    | Invalid Host ID            |
| 5    | Invalid transid            |
| 6    | This transid has been used |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "balance": 100000
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID"
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/withdraw-v2",
  "query": {
    "host_id": "{host_id}",
    "member_id": "{member_id}",
    "amount": "10000",
    "transid": "{transid}"
  }
}
```

<!-- type: tab-end -->

### Suspend

To suspend an active player account, the suspend API is called.

<!--
type: tab
title: Docs
-->

### Request

| Name      | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| host_id   | string | Unique ID of Operator System (provided by game provider) |
| member_id | string | Unique ID / Username of the player                   |

> ##### Example
>
> <https://{PROVIDER_API_ENDPOINT}/api/user/suspend?host_id={host_id}&member_id={member_id}>

### Response

| Name        | Type   | Description                        |
| ----------- | ------ | ---------------------------------- |
| status_code | int    | Response status code               |
| member_id   | string | Unique ID / Username of the player |
| message     | string | Response message                   |

#### Status Code

| Code | Description       |
| ---- | ----------------- |
| 0    | Success           |
| 1    | Invalid Member ID |
| 2    | Invalid Host ID   |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "member_id": "demo01",
    "message": "User has been suspended"
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID"
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/suspend",
  "query": {
    "host_id": "{host_id}",
    "member_id": "{member_id}"
  }
}
```

<!-- type: tab-end -->

### Unsuspend

To activate a suspended player account, the unsuspend API is called.

<!--
type: tab
title: Docs
-->

### Request

| Name      | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| host_id   | string | Unique ID of Operator System (provided by game provider) |
| member_id | string | Unique ID / Username of the player                   |

> ##### Example
>
> <https://{PROVIDER_API_ENDPOINT}/api/user/unsuspend?host_id={host_id}&member_id={member_id}>

### Response

| Name        | Type   | Description                        |
| ----------- | ------ | ---------------------------------- |
| status_code | int    | Response status code               |
| member_id   | string | Unique ID / Username of the player |
| message     | string | Response message                   |

#### Status Code

| Code | Description       |
| ---- | ----------------- |
| 0    | Success           |
| 1    | Invalid Member ID |
| 2    | Invalid Host ID   |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "member_id": "demo01",
    "message": "User has been activated"
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID"
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/unsuspend",
  "query": {
    "host_id": "{host_id}",
    "member_id": "{member_id}"
  }
}
```

<!-- type: tab-end -->

## Game List

This api will return game list to operator.

<!--
type: tab
title: Docs
-->

### Request

| Name    | Type   | Description                                          |
| ------- | ------ | ---------------------------------------------------- |
| host_id | string | Unique ID of Operator System (provided by game provider) |

> ##### Example
>
> <https://{PROVIDER_API_ENDPOINT}/api/user/gamelist?host_id={host_id}>

### Response

| Name        | Type  | Description          |
| ----------- | ----- | -------------------- |
| status_code | int   | Response status code |
| list        | array | A list of game list  |

#### Status Code

| Code | Description                    |
| ---- | ------------------------------ |
| 0    | Success                        |
| 2    | Invalid Host ID                |
| 2001 | Required field cannot be empty |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "list": [
      {
        "title": {
          "en": "Blue Ocean",
          "cn": "蓝海龙王"
        },
        "game_id": "shoot-01",
        "game_code": "fish",
        "lobby_group": "shooting",
        "url": "https://shoot-01.velachip.com"
      }
    ]
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID",
    "retry": null
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/gamelist",
  "query": {
    "host_id": "{host_id}"
  }
}
```

<!-- type: tab-end -->

## Game Launch

For launching games, please refer to the URL parameters as below. The game URL is referred to [Game List](#game-list) reponse.

<!--
type: tab
title: Docs
-->

### Request

| Name           | Type   | Description                                                                         |
| -------------- | ------ | ----------------------------------------------------------------------------------- |
| host_id        | string | Unique ID of Operator System (provided by game provider)                                |
| access_token   | string | The access token is generated by operator system for the player's session.      |
| mode           | string | Game mode <br> (singleplayer, multiplayer) <br> \*\*only required for action game   |
| lang           | string | Language of the game (ch, en) <br> Default language is en                           |
| allow_vertical | int    | Default is 1 <br> 0 - landscape mode <br> 1 - allow vertical mode and landscap mode |

> If host_id and access_token is not provided, it is treated as **Guest** mode.
>
> ##### Example
>
> <https://{GAME_URL}?host_id={host_id}&access_token={access_token}&mode=singleplayer&lang=ch&allow_vertical=1>

<!-- type: tab-end -->

## User Report

This API is to get the games report from Game Server. **Operator** must maintain the key to keep track log request. For the first time to request log, key = 0. The response return the key value to **operator** for the next log request.

<!--
type: tab
title: Docs
-->

### Request

| Name    | Type   | Description                                          |
| ------- | ------ | ---------------------------------------------------- |
| host_id | string | Unique ID of Operator System (provided by game provider) |
| key     | string | Unique database index number                         |
| page_size | int(optional) | Number of records. Maximum 500 |

> If key is empty, it will return data from index zero (0).<b>
> If page_size is empty, default return 10 records.<b>
>
> ##### Example
>
> <https://{PROVIDER_API_ENDPOINT}/api/report?host_id={host_id}&key={key}&page_size={page_size}>

### Response

| Name        | Type  | Description          |
| ----------- | ----- | -------------------- |
| status_code | int   | Response status code |
| report      | array | A list report        |

#### Status Code

| Code | Description     |
| ---- | --------------- |
| 0    | Success         |
| 2    | Invalid Host ID |

#### Report

| Name          | Type   | Description     |
| ------------- | ------ | --------------- |
| id            | int    | Id              |
| ticket_id     | int    | Ticket Id       |
| game_code     | string | Game code is referred to [Game List](#game-list) API       |
| game_group    | string | Game group      |
| username      | string | Player username |
| bet_stake     | double | Bet stake       |
| commission    | double | Commission      |
| bet_info      | array  | Bet info        |
| result_info   | array  | Result info     |
| payout_amount | double | Payout amount   |
| gain_amount   | double | Gain amount     |
| loss_amount   | double | Loss amount     |
| draw_amount   | double | Draw amount     |
| cancel_amount | double | Cancel amount   |
| reject_amount | double | Reject amount   |
| status        | string | Status          |
| report_date   | date   | Report date     |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
    "data": {
        "status_code": 0,
        "report": [
            {
                "id": "1552012",
                "ticket_id": "1493",
                "game_code": "fish",
                "code": "fish",
                "game_group": "shooting",
                "username": "demo1",
                "bet_stake": 2.7,
                "commission": 0,
                "bet_info": [
                    {
                        "payout": [
                            {
                                "type": 0,
                                "multiplier": 2,
                                "bullet_type": 0,
                                "bullet_cost": 0.1,
                                "total_gain": 0.2
                            },
                            {
                                "type": 3,
                                "multiplier": 4,
                                "bullet_type": 0,
                                "bullet_cost": 0.1,
                                "total_gain": 0.4
                            },
                            {
                                "type": 3,
                                "multiplier": 4,
                                "bullet_type": 0,
                                "bullet_cost": 0.1,
                                "total_gain": 0.4
                            },
                            {
                                "type": 0,
                                "multiplier": 2,
                                "bullet_type": 0,
                                "bullet_cost": 0.1,
                                "total_gain": 0.2
                            }
                        ],
                        "bullet": "[27,0,0,0,0]",
                        "total_bullet": 27
                    },
                    {
                        "payout": [],
                        "bullet": "[0,0,0,0,0]",
                        "total_bullet": 0
                    },
                    {
                        "payout": [],
                        "bullet": "[0,0,0,0,0]",
                        "total_bullet": 0
                    }
                ],
                "result_info": null,
                "payout_amount": 1.2,
                "gain_amount": 0,
                "loss_amount": 1.5,
                "draw_amount": 0,
                "cancel_amount": 0,
                "reject_amount": 0,
                "before_balance": 2070796.59,
                "after_balance": 2070795.09,
                "data": null,
                "jackpot": null,
                "status": "done",
                "remark": null,
                "report_date": "2022-03-28 15:58:27",
                "report_link": "https://**{PROVIDER_API_ENDPOINT}**/admin/detail-report?game=fish&id=1493&isiframe=true"
            }
        ],
        "key": "1552012",
        "version_key": "1552012"
    }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 2,
    "message": "Invalid Host ID"
  }
}
```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/report",
  "query": {
    "host_id": "{host_id}",
    "key": "{key}"
  }
}
```

<!-- type: tab-end -->

### Generating Access Token

This api will return a generated access token for the player to login.
<!--
type: tab
title: Docs
-->

### Request

| Name      | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| host_id   | string | Unique ID of Operator System (provided by game provider) |
| member_id | string | Unique ID / Username of the player                   |


> ##### Example
>
> [https://{PROVIDER_API_ENDPOINT}/api/user/generate-access-token?host_id={host_id}&member_id={member_id}](https://{PROVIDER_API_ENDPOINT}/api/user/generate-access-token?host_id={host_id}&member_id={member_id})

### Response

| Name        | Type   | Description                                 |
| ----------- | ------ | ------------------------------------------- |
| status_code | int    | Response status code                        |
| access_token     | string | The access token is generated by the game provider.
 |

### Status Code

| Code | Description         |
| ---- | -----------------   |
| 0    | Success             |
| 1001 | Invalid Host ID     |
| 1002 | Invalid Member ID   |
| 1003 | User does not exist |

<!--
type: tab
title: Examples
-->

### Sample Success Response

```json
{
  "data": {
    "status_code": 0,
    "access_token": "1tel8bz9ocbl6l8ze6gh3z88v9epfwzb5y58zmg5"
  }
}
```

### Sample Error Response

```json
{
  "error": {
    "status_code": 1006,
    "message": "User does not exist",
    "retry": null
  }
}

```

<!--
type: tab
title: Try It
-->

```json http
{
  "method": "get",
  "url": "https://{PROVIDER_API_ENDPOINT}/api/user/generate-access-token",
  "query": {
    "host_id": "{host_id}",
    "member_id": "{member_id}",
  }
}
```

<!-- type: tab-end -->
