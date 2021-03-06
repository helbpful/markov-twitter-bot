## markov-twitter-bot
[![Standard - JavaScript Style Guide](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com/)

Everything you need to create a twitter bot that generates tweets using a [Markov Chain](https://en.wikipedia.org/wiki/Markov_chain), the method used by most '\_ebooks' accounts. All you need to do is supply a twitter handle (without the @), and an object containing your bot account's API keys.

### Requirements
You will need an active Twitter account for your bot, and you will also need to create a Twitter application. If you are unsure of how to do that, follow steps 1 - 3 on [this page](http://www.pygaze.org/2016/03/how-to-code-twitter-bot/).

### Usage

```
const TwitterBot = require('markov-twitter-bot')

const options = {
  account: 'SomeTwitterHandle',
  twitter: {
    consumer_key: 'CONSUMERKEYFROMTWITTER',
    consumer_secret: 'CONSUMERSECRETFROMTWITTER',
    access_token_key: 'ACCESSKEYFROMTWITTER',
    access_token_secret: 'ACCESSSECRETFROMTWITTER'
  },
  replyTo: 'MyBotAccountName'
}
// You can leave the twitter object out if you have environment variables set. (see below)

const bot = new TwitterBot( options )

```
That's all you need to have a bot generate tweets from `SomeTwitterHandle` every 4 hours, while updating the available tweets every hour at 55 minutes past the hour.

### Options
A full options object with it's defaults is as follows:
```
{
  hour: 4,
  minute: 0,
  account: '',
  twitter: {
    consumer_key: process.env.TWITTER_CONSUMER_KEY,
    consumer_secret: process.env.TWITTER_CONSUMER_SECRET,
    access_token_key: process.env.TWITTER_ACCESS_TOKEN_KEY,
    access_token_secret: process.env.TWITTER_ACCESS_TOKEN_SECRET
  },
  bannedWords: [],
  includeAts: true,
  includeHashtags: true,
  replyTo: ''
}
```
#### options.hour
*Number -- optional*

This is the interval of hours that a new tweet will be generated and posted. Keep in mind that this will start at midnight local time. The example above will post a tweet every 4 hours, starting at midnight (continuing at 4AM, 8AM, 12PM...)

#### options.minute
*Number -- optional*

This is the interval of minutes to post a new tweet. Use this if you want to stagger the hourly posting of tweets so that it doesn't occur exactly on the hour. In the example above, this is set to 0, so a tweet is posted exactly every four hours. If this were set to 15, it would be posted every 4 hours on the fifteenth minute of the hour.

#### options.account
*String -- **required***

This is the account to pull tweets from. Make sure to not include the @ in the string.

#### options.twitter
*Object -- **required***

This is an object that contains four properties: `consumer_key`, `consumer_secret`, `access_token_key`, and `access_token_secret`. These properties default to environment variables, so you can have them automatically set that way, or you can explicitly pass them as strings. Here is the default object, so you can see the environment variable names:
```
{
  consumer_key: process.env.TWITTER_CONSUMER_KEY,
  consumer_secret: process.env.TWITTER_CONSUMER_SECRET,
  access_token_key: process.env.TWITTER_ACCESS_TOKEN_KEY,
  access_token_secret: process.env.TWITTER_ACCESS_TOKEN_SECRET
}
```

#### options.bannedWords
*Array -- optional*

An array of strings that you do not want to allow in the generated tweets. This package already uses [this-is-probably-ok-to-say](https://www.npmjs.com/package/this-is-probably-ok-to-say), but you can choose to filter out certain other words as well (i.e. a bot I created for a friend generated tweets containing the @ handle of their ex). Entered words will be case-insensitive, so `'TEST'` will match `'test'`, etc.

#### options.includeAts
*Boolean -- optional*

Set this to false to exclude any tweets containing `@username`s. Defaults to true.

#### options.includeHashtags
*Boolean -- optional*

Set this to false to exclude any tweets containing hashtags. Defaults to true.

#### options.replyTo
*String -- optional*

If you supply an account name to this value, the bot will reply to any tweets sent to that account. Enter your bot account's handle (**without the @**) to have it reply when it is mentioned. Defaults to `''`, disabling this functionality.
