# @outlet/sdk

Official TypeScript/JavaScript SDK for [Outlet.sh](https://outlet.sh) - The modern, self-hosted email platform.

## Installation

```bash
npm install @outlet/sdk
# or
pnpm add @outlet/sdk
# or
yarn add @outlet/sdk
```

## Quick Start

```typescript
import { Outlet } from '@outlet/sdk';

const outlet = new Outlet('your-api-key', 'https://mail.yourdomain.com');

// Send a transactional email
const result = await outlet.emails.sendEmail({
  to: 'user@example.com',
  subject: 'Welcome!',
  html_body: '<h1>Welcome to our platform</h1>',
});

console.log('Message ID:', result.message_id);
```

## Features

### Transactional Emails

```typescript
// Send with template
await outlet.emails.sendEmail({
  to: 'user@example.com',
  template_slug: 'welcome-email',
  variables: {
    name: 'John',
    activation_link: 'https://example.com/activate/123'
  }
});

// Check delivery status
const status = await outlet.emails.getEmailStatus(messageId);

// Get email events (opens, clicks, etc.)
const events = await outlet.emails.listEmailEvents(messageId);
```

### Contacts

```typescript
// Create a contact
const contact = await outlet.contacts.createContact({
  email: 'user@example.com',
  first_name: 'John',
  last_name: 'Doe',
  custom_fields: { company: 'Acme Inc' }
});

// Add tags
await outlet.contacts.addContactTags(contact.id, {
  tags: ['customer', 'premium']
});

// View activity
const activity = await outlet.contacts.listContactActivity(contact.id);
```

### Lists & Subscriptions

```typescript
// Subscribe to a list
await outlet.lists.subscribeToList('newsletter', {
  email: 'user@example.com'
});

// Unsubscribe
await outlet.lists.unsubscribeFromList('newsletter', {
  email: 'user@example.com'
});
```

### Sequences (Drip Campaigns)

```typescript
// Enroll in a sequence
await outlet.sequences.enrollInSequence({
  email: 'user@example.com',
  sequence_slug: 'welcome-series'
});

// Pause enrollment
await outlet.sequences.pauseSequenceEnrollment({
  email: 'user@example.com',
  sequence_slug: 'welcome-series'
});

// Resume
await outlet.sequences.resumeSequenceEnrollment({
  email: 'user@example.com',
  sequence_slug: 'welcome-series'
});
```

### Webhooks

```typescript
// Register a webhook
const webhook = await outlet.webhooks.registerWebhook({
  url: 'https://your-app.com/webhooks/outlet',
  events: ['email.delivered', 'email.opened', 'email.clicked'],
  secret: 'your-webhook-secret'
});

// List webhooks
const webhooks = await outlet.webhooks.listWebhooks();

// Test webhook
await outlet.webhooks.testWebhook(webhook.id);
```

### Stats & Analytics

```typescript
// Overview stats
const overview = await outlet.stats.getStatsOverview({
  start_date: '2024-01-01',
  end_date: '2024-01-31'
});

// Email stats
const emailStats = await outlet.stats.getEmailStats({
  start_date: '2024-01-01',
  end_date: '2024-01-31',
  group_by: 'day'
});

// Contact growth stats
const contactStats = await outlet.stats.getContactStats({
  start_date: '2024-01-01',
  end_date: '2024-01-31'
});
```

## Configuration

```typescript
const outlet = new Outlet(apiKey, baseUrl, {
  timeout: 30000, // Request timeout in milliseconds (default: 30000)
});
```

## Error Handling

```typescript
try {
  await outlet.emails.sendEmail({
    to: 'invalid',
    subject: 'Test',
    html_body: '<p>Test</p>'
  });
} catch (error) {
  console.error('Failed to send email:', error.message);
}
```

## TypeScript Support

This SDK is written in TypeScript and includes full type definitions. All request/response types are exported:

```typescript
import type {
  SendEmailRequest,
  SendEmailResponse,
  ContactRequest,
  SDKContactInfo
} from '@outlet/sdk';
```

## License

MIT
