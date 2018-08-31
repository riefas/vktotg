# vk to telegram forwarder
Powered by [Telegraf](https://github.com/telegraf/telegraf)  
[![NPM Version](https://img.shields.io/npm/v/vk-to-telegram.svg?style=flat-square)](https://www.npmjs.com/package/vk-to-telegram)
[![node](https://img.shields.io/node/v/vk-to-telegram.svg?style=flat-square)](https://www.npmjs.com/package/vk-to-telegram)
[![npm downloads](https://img.shields.io/npm/dm/vk-to-telegram.svg?style=flat-square)](http://npm-stat.com/charts.html?package=vk-to-telegram)
[![telegram test channel](https://img.shields.io/badge/telegram-test%20channel-blue.svg)](https://t.me/vktotgforwarderchannel)
[![telegram chat](https://img.shields.io/badge/telegram-chat-blue.svg)](https://t.me/vktotgforwarder)
## Installation
    npm install vk-to-telegram --save
### Example
```js
const app = require('express')()
const bodyParser = require('body-parser')
const vkToTelegram = require('vk-to-telegram')
const vkToTg = new vkToTelegram({
        botToken: 'your bot token',
        chatName: 'telegram chat/channel name', // 
        ownerId: 'your telegram id', // number
        vkToken: 'your very long token from vk api',
        vkConfirmation: 'group confirmation'
    })
app.use(bodyParser.json()) // Needs to parse request body
app.post('/', (req, res) => {
    vkToTg.send(req, res)
        .then(() => console.log('Done!'))
        .catch((err) => {
            console.log('Something went wrong')
            console.log(err)
        })
}) 

app.listen(80,()=>{
  console.log('listening on port 80')
})  
```
## What is this?  
It is a tool for express which using [VK callback api](https://vk.com/dev/callback_api) forwards posts from group in channel or chat in Telegram!  

## They use vk-to-telegram in production
|[<img src="https://i.imgur.com/pra7Wez.jpg" width="120">](https://vk.com/tavernofoverwatch)|[<img src="https://i.imgur.com/2RR0fXh.png" width="120">](https://vk.com/panzer_sofa) | [<img src="https://i.imgur.com/51DrStx.jpg" width="120">](https://vk.com/oleglivanovgaming) |
|-|-|-|

## What content does it forward?

| Content type | Works fully? |  
| - | - |  
| Photo(s) | `Yes` |
| Video(s) | `Yes`, as links. |  
| Audio(s) | **NO.** Why? [Read here](https://vk.com/dev/audio). |
| Document(s) | `Yes` |
| Link | `Yes`, but VK do [terrible things ...](https://i.imgur.com/vb0LDP4.png) with links titles written by cyrillic. |  
| Application Content | `Yes` |
| Poll | **Not** yet, but forwarder will send link to poll. |
| Album(s) | `Yes`, as photos in the caption have links to the album(s). |
| Graffiti | `Not tested.` |
| Wiki Page | `Not tested.` |
| Market item | `Not tested.` |
| Sticker | `Not tested.` |


## Free usage

If you want to test this code, or to use on a regular basis(beta, works via heroku), please [contact](#contact) me for details.

## Variables
|Variable|Type|Required|Description|
|-|-|-|-|
| `token`|`String`|**Yes**|Bot token from [Botfather](https://t.me/botfather)|
| `chatName`|`String` | **Yes**  | Telegram channel or group link, like '[@tavernofheroes](https://t.me/tavernofoverwatchnews)'|
|`chatId`|`Number`|`Optional`|If you know your chat/channel id, put it here, it will replace chatName parameter|
| `ownerId`|`Number`|**Yes** | Your telegram id for sending error if they are. U can get know it from [@getidsbot](https://t.me/getidsbot)|
| `vkConfirmation`|`String`|**Yes**|Confirmation string from ur group callback api server: <img src="https://i.imgur.com/Gq1bly4.png" width="600">|
|`fromId` |`Number`| Optional | VK group id with '-'in start or nothing, if you don't need check. |
|`customVkButton`|`String`|`Optional`|Title for button which will be added to each post to open it in VK|
|`customPollTitle`|`String`|`Optional`|Custom template string in the title of button with URL to poll("Open poll" -> "Open poll - ${poll.question}")|
|`customLongPostText`|`String`|`Optional`|Custom template string that replace full post text, because it's too long for Telegram(max 4096 characters) ("Too long post... [Read full]" -> "Too long post... \<a href="https://vk.com/poll${poll.owner_id}_${poll.id}">Read full</a>" and parse as HTML)|
|`signed`|`String`|`Optional`|Custom template string that add post signer in the end of Telegram message ("Post By" -> "\n\nPost by \<a href="https://vk.com/id${post.signer_id}">${signer.first_name} ${signer.last_name}</a>" and parse as HTML) |
|`heroku`|`Boolean`|`Optional`|Add filter that stops forwarder if detect that post repeats(Because of app [sleeping](https://devcenter.heroku.com/articles/free-dyno-hours))|
| `vkToken` |`String`| **Yes** | Follow the instructions below:|
||||1. Create Standalone application here: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage) |
||||2. Open settings in created application and copy application id |
||||3. Open this link with replace your application id: |
||||https://oauth.vk.com/authorize?client_id=YOUR APPLICATION ID&display=page&redirect_uri=http://vk.com/&scope=offline,video,docs&response_type=token&v=5.73|
||||4. Click allow all that need's and it's all! Your token is in query url, do not copy all link, only token without other params.  |

* DON'T forget to pick in your vk group api dashboard event type 'WALL POST - NEW'.
* Recommend to use vk api v5.71

## Contact
Here's a telegram [group](https://t.me/vktotgforwarder) ¯\\_(ツ)_/¯   
Also u can write to me directly in [Telegram](https://t.me/ejnshtein),
[VK](https://vk.com/lbmmbr001) or by [mail](mailto:ejnshtein@dsgstng.com)  