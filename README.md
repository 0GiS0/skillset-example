# Test Data Bot

> [!NOTE]
> To use Copilot Extensions, you must be enrolled in the limited public beta.
> 
> All enrolled users with a GitHub Copilot Individual subscription can use Copilot Extensions.
> 
> For enrolled organizations or enterprises with a Copilot Business or Copilot Enterprise subscription, organization owners and enterprise administrators can grant access to Copilot Extensions.

## Description

This is an example skills copilot extension, designed to create random data for a number of development purposes.  The purpose of this to showcase how an extension can be created using skills

## Installation:
1. Clone the repository: 

```
git clone git@github.com:copilot-extensions/testdatabot.git
cd testdatabot
```

2. Install dependencies:

```
go mod tidy
```

## Usage

1. Start up ngrok with the port provided:

```
ngrok http http://localhost:8080
```

2. Set the environment variables (use the ngrok generated url for the `FDQN`)
3. Run the application:

```
go run .
```

## Accessing the Agent in Chat:

1. In the `Copilot` tab of your Application settings (`https://github.com/settings/apps/<app_name>/agent`)
- Set the app type to "Skill"
- Add the following skills
```
Name: random_commit_message
Inference description: Generates a random commit message
URL: https://<your ngrok domain>/random-commit-message
Parameters: { "type": "object" }
Return type: String
---
Name: random_lorem_ipsum 
Inference description: Generates a random Lorem Ipsum text.  Responses should have html tags present.
URL: https://<your ngrok domain>/random-lorem-ipsum
Parameters: 
{
   "type": "object",
   "properties": {
      "number_of_paragraphs": {
         "type": "number",
         "description": "The number of paragraphs to be generated.  Must be between 1 and 10 inclusive"
      },
      "paragraph_length": {
         "type": "string",
         "description": "The length of each paragraph.  Must be one of \"short\", \"medium\", \"long\", or \"verylong\""
      }
   }
}
Return type: String
---
Name: random_user
Inference description: Generates data for a random user
URL: https://<your ngrok domain>/random-user
Parameters: { "type": "object" }
Return type: String
```

2. In the `General` tab of your application settings (`https://github.com/settings/apps/<app_name>`)
- Set the `Callback URL` to anything (`https://github.com` works well for testing, in a real environment, this would be a URL you control)
- Set the `Homepage URL` to anything as above
3. Ensure your permissions are enabled in `Permissions & events` > 
- `Account Permissions` > `Copilot Chat` > `Access: Read Only`
4. Ensure you install your application at (`https://github.com/apps/<app_name>`)
5. Now if you go to `https://github.com/copilot` you can `@` your agent using the name of your application.

## What can the bot do?

Here's some example things:

* `@testdatabot please create a random commit message`
* `@testdatabot generate a lorem ipsum`
* `@testdatabot generate a short lorem ipsum with 3 paragraphs`
* `@testdatabot generate random user data`

## Implementation

This bot provides a passthrough to a couple of other APIs:

* For commit messages, https://whatthecommit.com/
* For Lorem Ipsum, https://loripsum.net/
* For user data, https://randomuser.me/