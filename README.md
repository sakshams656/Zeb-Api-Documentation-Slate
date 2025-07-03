# ZebPay API Documentation

## Introduction
This repository contains the API documentation for ZebPay. Follow the steps below to generate API keys, access documentation, set up a local development environment, and deploy updates.

## API Access
- **Generate API Keys**: Visit [https://zebpay.com/app/api-trading](https://zebpay.com/app/api-trading) to create your API keys.
- **Read Documentation**: Explore the full API documentation at __________.

## Development/Local Setup
To set up the documentation locally, follow these steps:
1. Clone the repository:
git clone https://github.com/coindcx-official/rest-api.git

text

Collapse

Wrap

Copy
2. Install dependencies:
bundle install

text

Collapse

Wrap

Copy
3. Start the local server:
bundle exec middleman server

text

Collapse

Wrap

Copy
4. Visit [http://127.0.0.1:4567](http://127.0.0.1:4567) to view the documentation.

## Writing Documentation
- Update the `index.html.md` file to add or modify documentation content.

## Deployment
To deploy changes:
1. Commit and push your changes to the repository.
2. Run the deployment script:
./deploy.sh
