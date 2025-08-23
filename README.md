# AWS RoboAdvisor

A serverless chatbot that recommends a retirement portfolio using Amazon Web Services (AWS).

The bot combines an Amazon Lex conversational interface with an AWS Lambda function that
validates user input and returns a suggested allocation of stocks and bonds.

## Features

- **Interactive dialogue** – gathers a user's name, age, investment amount, and risk tolerance.
- **Input validation** – ensures the user is between 1 and 64 years old and investing at least $5,000.
- **Automated recommendations** – maps the selected risk level to an appropriate bond/equity mix.

## Repository structure

```
.
├── README.md
├── RoboAdvisor/
│   ├── lambda_function.py            # Lambda code for validation and recommendation
│   ├── RecommendPortfolio_Export.json  # Lex intent configuration
│   ├── RoboAdvisor_Export-2.json       # Full Lex bot definition
│   └── RoboAdvisor Bot.gif            # Conversation example
└── Test Cases/
    ├── age_error.txt                 # Sample dialog: age outside permitted range
    ├── negative_age_error.txt        # Sample dialog: negative age supplied
    ├── incorrect_amount_error.txt    # Sample dialog: investment below $5,000
    └── correct_dialog.txt            # Sample dialog: successful conversation
```

## Getting started

1. **Deploy the Lex bot**
   - Import `RoboAdvisor_Export-2.json` into Amazon Lex.
   - Build the bot so the intent and slots become active.

2. **Create the Lambda function**
   - Upload `lambda_function.py` to AWS Lambda and include the `python-dateutil` dependency.
   - Connect the Lambda function to the Lex bot as its fulfillment method.

3. **Test the conversation**
   - In the Lex console, use the test window and try the scripts from the `Test Cases` directory.

## Local testing

There are no automated unit tests included, but you can invoke the Lambda function locally
by sending it a simulated event:

```bash
python - <<'PYTHON'
from RoboAdvisor.lambda_function import lambda_handler

sample_event = {
    "currentIntent": {
        "name": "RecommendPortfolio",
        "slots": {
            "firstName": "Jane",
            "age": "30",
            "investmentAmount": "10000",
            "riskLevel": "Low"
        }
    },
    "invocationSource": "FulfillmentCodeHook",
    "sessionAttributes": {}
}

print(lambda_handler(sample_event, None))
PYTHON
```

## Disclaimer

This project is for educational purposes only and does not constitute financial advice.
