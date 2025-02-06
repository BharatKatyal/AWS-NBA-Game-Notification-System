# NBA Game Day Notifications

## Project Overview
Serverless AWS application that sends NBA game notifications via email using Lambda, EventBridge, and SNS. The application fetches game data from the NBA API and delivers real-time updates through email notifications.

![alt text](repo_assets/Architecture_Diagram.png)


## Architecture
- **Lambda Function**: Processes NBA game data (runs every 2 hours 9AM-2AM CT)
- **EventBridge**: Schedules function execution
- **SNS Topic**: Delivers email notifications
- **Secrets Manager**: Stores NBA API key securely
- **CloudWatch Logs**: Monitors Lambda execution and storing logs


## Environment Variables
- `SNS_TOPIC_ARN`: SNS topic for notifications
- `NBA_API_SECRET_ARN`: Secret name in AWS Secrets Manager

## Security Features
- API key stored in Secrets Manager
- IAM roles with least privilege
- Limited Lambda permissions
- NoEcho for sensitive parameters

## AWS Resources Created
- Lambda Function & IAM Role
- SNS Topic & Subscription
- Secrets Manager Secret
- EventBridge Schedule
- Required IAM Policies



## Project Structure
```
├── game_day_notification/          # Lambda function directory
│   ├── app.py                     # Main Lambda handler
│   └── requirements.txt           # Python dependencies
├── events/                        # Sample events
│   └── event.json                # Test event payload
├── template.yaml                  # SAM template
├── env.json                    # Environment variables to invoke lambda locally
└── README.md                      # This file
```

## Prerequisites
- AWS CLI configured with appropriate permissions
- AWS SAM CLI installed
- Python 3.13
- NBA API key from sportsdata.io
- Email address for notifications

## Deployment Steps

1. Build the application:
```bash
sam build
```

2. Deploy for first time (with guided prompts):
```bash
sam deploy --guided --capabilities CAPABILITY_NAMED_IAM
```

You'll need to provide:
- Stack name
- AWS Region
- NBA API Key (will be stored securely)
- Email address for notifications

3. For subsequent deployments:
```bash
sam deploy
```

4. Confirm Verification email from AWS SES
- After deployment, AWS SNS will send a subscription confirmation email. 
- Click the "Confirm subscription" link in the email
- Email notifications will begin after confirmation
## Testing

### Adding Enviroment Variables locally 
Edit the env.json with the appropriate arns for the SNS_TOPIC_ARN and NBA_API_SECRET_ARN

example
```
{
    "GameDayNotificationFunction": {
      "SNS_TOPIC_ARN": "arn:aws:sns:us-east-1:123456789:mynbanotification-GameDayTopic-eFZz7wWIxWmo",
      "NBA_API_SECRET_ARN": "arn:aws:secretsmanager:us-east-1:123456789:secret:/mynbanotification/nba-api-key-Js8PeI"
    }
  }
```
### Example Email Notification
Below is a sample of the email notification format:

![alt text](repo_assets/SNS_Email_NBA.png)

```bash
sam local invoke -e events/event.json --env-vars env.json 
```

### Troubleshooting
Ensure AWS credentials are configured
Verify SNS Topic ARN format
Check Secret name matches Secrets Manager
Confirm local environment variables match deployed values


