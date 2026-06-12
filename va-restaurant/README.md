# va-restaurant

A voice agent that accepts To-Go orders via phone call for a restaurant business.

## Overview

This project builds a phone-based voice agent that allows customers to place to-go orders by calling the restaurant. The agent handles the full ordering flow — greeting, menu navigation, item selection, customizations, order confirmation, and estimated pickup time.

## Tech Stack (Planned)

- **Voice Platform**: Twilio (inbound call handling, TTS/STT)
- **AI/NLP**: Claude API (conversation management, intent recognition)
- **Backend**: Node.js / Python (order processing, menu management)
- **Database**: PostgreSQL (orders, menu items)

## Project Structure

```
va-restaurant/
├── agent/          # Voice agent logic and conversation flows
├── api/            # Backend REST API
├── config/         # Configuration files (menu, prompts, etc.)
├── docs/           # Documentation and design notes
└── tests/          # Test suites
```

## Getting Started

> Setup instructions will be added as the project develops.

## Features (Planned)

- [ ] Inbound call handling via Twilio
- [ ] Natural language order taking
- [ ] Dynamic menu reading
- [ ] Item customization handling (size, toppings, substitutions)
- [ ] Order confirmation and summary
- [ ] Estimated pickup time notification
- [ ] Order handoff to kitchen/POS system
