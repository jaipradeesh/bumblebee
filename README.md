## Bumblebee

Bumblebee is `slackRTM` powered slack bot written in `python` which helps you to communicate with your [VWO](https://vwo.com/) account over slack.

## Index

- [Usage](#usage)
- [Installation](#installation)
    - [Run the bot](#run-the-bot)
- [Dockerfile](#dockerfile)
- [Miscelleneous](#miscelleneous)
- [Tests](#tests-wip)
- [Links](#links)
- [License](#license)

## Usage

Example command for help

`@starterbot: help`

Replace `starterbot` by the name of the bot

`@starterbot: help`
> shows the list of commands which can be given to the bot

`@starterbot: ​all campaigns`
> shows the last 15 campaign data

`@starterbot: campaign details <your_campaign_id>`
> shows all data for a specific campaign, campaign_id would be an integer value

`@starterbot: update campaign <your_campaign_id> status to <new_campaign_status>`
> updates a campaign status to user specified ['run', 'stop', 'pause']

`@starterbot: share campaign <your_campaign_id>`
> Provides a link of the campaign to be shared

`@starterbot: tracking code`
> The tracking code to be installed in your websites

## Installation

- Assuming you have admin privileges in your slack team, go to the [Bot Users page](https://api.slack.com/bot-users).

- Create your new bot and give it a name.

- After you have added the integration, you will get an API key. That will be your `SLACK_BOT_TOKEN` token.

- Get your `BOT_ID` by running the script below, making the appropriate changes

```sh
from slackclient import SlackClient

BOT_NAME = 'starterbot'  # replace 'starterbot' with the name of your bot name

slack_client = SlackClient('token')  # replace 'token' with your slack bot token


if __name__ == "__main__":
    api_call = slack_client.api_call("users.list")
    if api_call.get('ok'):
        # retrieve all users so we can find our bot
        users = api_call.get('members')
        for user in users:
            if 'name' in user and user.get('name') == BOT_NAME:
                print("Bot ID for '" + user['name'] + "' is " + user.get('id'))
    else:
        print("could not find bot user with the name " + BOT_NAME)
```

- Generate [vwo tokens](https://app.vwo.com/#/developers/tokens/)

- Install the dependencies

```
$ git clone https://github.com/wingify/bumblebee.git
$ cd bumblebee
$ virtualenv env              # Create virtual environment
$ source env/bin/activate     # Change default python to virtual one
(env)$ make deps
```

Before running the bot, you have to configure it for your VWO API token and slack API tokens. Assuming that you are in the directory, `bumblebee`

```sh
$ cd bumblebee
$ cp config.ini.example config.ini
$ cat config.ini
[slack]
SLACK_BOT_TOKEN=
BOT_ID=
BOT_NAME=

[vwo]
VWO_API_TOKEN=
ACCOUNT_ID=
```

They are left blank, set to them up to dance with `bumblebee`


## Run the bot

```sh
(env)$ make run
```

**NOTE**:

Check whether the bot can connect to the `slack RTM API` by running

```sh
$ cd bumblebee
$ python -m unittest tests.test_slack_api_auth
```

## Dockerfile

A quicker way to get up and running would be to build the `Dockerfile` here.

```sh
$ git clone https://github.com/wingify/bumblebee.git
$ cd bumblebee
```

Next step would be to get your file `config.ini`. And place the appropriate tokens.
Get `BOT_ID` by running the script above.

Build the image using the `dockerfile`

```sh
$ docker build --no-cache=true -t bumblebee .
```

Run your bot bot


```sh
$ docker run -it \
-e SLACK_BOT_TOKEN="YOUR_SLACK_BOT_TOKEN" \
-e BOT_ID="YOUR_BOT_ID" \
-e VWO_API_TOKEN="YOUR_VWO_API_TOKEN" \
-e ACCOUNT_ID="YOUR_ACCOUNT_ID" \
-e BOT_NAME="YOUR_BOT_NAME" \
bumblebee make run
```

Where you should replace the `token` values with yours

## Miscelleneous

`flake8` needs to be installed manually for `pep8` conformancy.

```sh
$ pip install flake8
$ make flake8
```

**NOTE**: For getting your `BOT_ID`, run. This is assuming that you have
already specified the required tokens in the `bumblebee/config.ini` file


```sh
$ make bot_id
```

## Tests [WIP]

To run the current test suite

```sh
$ cd bumblebee_bot
$ make tests
```

## Links

- [VWO Docs](http://developers.vwo.com/docs/introduction/)
- [Token Generation](https://app.vwo.com/#/developers/tokens/)

## Docs

You can build the docs using sphinx.

```sh
$ make docs
```

## LICENSE

MIT Licensed. You can find a copy of the license file in the root directory

Copyright (c) 2016 Wingify Software Pvt. Ltd.

Author: [Tasdik Rahman](http://tasdikrahman.me/) [(@tasdikrahman)](https://twitter.com/tasdikrahman/)

[Wingify Software LTD](http://wingify.com/)