# ersl-bot

## Environments

The following table explains the branches for different environments of ersl-bot

| Branch Name | Purpose                                                                                                                                        |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| develop     | This is where features are approved and reviewed here before being selected to go into the release branch a few days before a regular release. |
| master      | Production environment.                                                                                                                        |

## Development

You must install [Node.js](https://nodejs.org/). The suggested method is with [nvm](https://github.com/creationix/nvm).
The `.nvmrc` file specifies the node version used by the console. Install it by running `nvm install`. Switch versions with `nvm use`.

After you have node, you must install the project dependencies. We use [Yarn](https://yarnpkg.org/) to manage node dependencies.
Do this by running `yarn install`. If you already had dependencies installed and it gives you any errors,
you may try deleting your `node_modules` folder and try running `yarn install` again.

### Setup Slack Bot
- Follow this tutorial for create new [Slack's Workspace and Slack Bot](https://dev.to/oghenebrume50/building-a-slack-bot-with-nodejs-3g40)
- Create `env file` from `.env.example` and add Slack Credentials from `Basic Information > App Credentials`.
- Make a temporary domain by [ngrok](https://www.npmjs.com/package/ngrok), look like `https://temporary.ngrok.io` and add to `SLACK_REDIRECT_URI` in env file
```
ngrok http 5000
```
- In slack api page, go to `Features > OAuth & Permissions`, add temporary domain (`https://temporary.ngrok.io`) into `Redirect URLs`,
![](https://i.gyazo.com/5d656165e57d5e90656610175098bfd8.png)
-  Then add list scopes `app_mentions:read`, `channels:history`, `channels:read`, `chat:write`, `groups:history`, `groups:read`, `im:history`, `im:read`, `im:write`, `users.profile:read`, `users:read`, `users:read.email`, `mpim:write`
- After that, generate Bot user token and add to `SLACK_BOT_TOKEN` in `env file`.
![](https://i.gyazo.com/cf4a2bf5b62df28a87c80191d7d6654f.png)
- Start app
- Go to `Event Subscriptions` in Slack API, enable and add `Request URL = https://temporary.ngrok.io/api/messages`,
![](https://i.gyazo.com/be85ca3ee9130986d5f25078fbefd1db.png)  
- Add subscribe events: `app_mention`, `message.channels`, `message.im`.                         
![](https://i.gyazo.com/d8f24b0def5f1f7e5620930b8bcbc14b.png)
- Go to `Interactivity & Shortcuts`, enable and `Request URL = https://temporary.ngrok.io/api/messages`
![](https://i.gyazo.com/b0407031cb56cb191528bf6f3d71e245.png)
- Done, go to slack chat for recheck.
![](https://i.gyazo.com/34bd2f148dde53657580946522d73d76.png)

### Setup Google API
- Create Google API Project: [Here](https://console.developers.google.com/apis/)
- Enable `Calendar SDK` & `Admin SDK`: `API & Services` > `Library` > Find and enable `Calendar SDK` , `Admin SDK` & `Google Driver API`
![](https://i.gyazo.com/dfca8e583d941151e1ca9e3a99d96450.png)
- Go to `Credentials` > `Create Credentials` > `OAuth CLient ID`             
![](https://i.gyazo.com/90a4d6cb25a0d5e50470d9cc424f9dd6.png)
- If is the first time you create a project with Google API, you need `Configure Consent Screen` first; complete all required fields then `Save and Countinue`. 
![](https://i.gyazo.com/60e4aa1f5a35652a30870165b1253e89.png)
![](https://gyazo.com/624a27535372b7cdef7afd663fcab46a.png)
- Create new OAuth Client ID                          
![](https://i.gyazo.com/fb30abd0b736e9d888f9a563b63312d4.png)
- Choose default OAuth 2.0 Client ID and copy `Client ID (GOOGLE_CLIENT_ID)`, `Client secret (GOOGLE_REDIRECT_URL)` to `.env` file.
![](https://i.gyazo.com/a851c9015b992992d0e3d01e32cf32d9.png)
- Last config is `GOOGLE_REDIRECT_URL`, just like `Redirect URLs` of Slack Bot, we add `https://temporary.ngrok.io/google/authorized` or `http://localhost:5000/google/authorized`` in Google config and `.env` file.                  
![](https://i.gyazo.com/664cc978b606641de75034d8f2b1403f.png)


### Setup Shortcut Actions.
- Go to `Slack API` of bot, enter `Interactivity & Shortcuts` add shortcuts look like this
- Action key: `translate_en`, `translate_vi`, `translate_ja`
![](https://i.gyazo.com/167dc706e2c542fb3571a6bd86190048.png)

### Running Migrations

We haven't inserted anything into the database. Now to actually create that table in database you need to run migration command.

```
NODE_ENV=development yarn db:migrate
```

### Running Seeds

To do that we need to run a simple command.

```
NODE_ENV=development yarn db:seed
NODE_ENV=development yarn redis:seed
```

This will execute that seed files under `seeders/*.js`. Reference [Sequelize#migrations](https://sequelize.org/v5/manual/migrations.html) to know details.

To start your application in the dev profile, simply run:

```
yarn start
```

## Testing

Those tests can be run using `yarn test`.
