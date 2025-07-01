# Pushcut Notify Action

A GitHub Action to send push notifications via the [Pushcut](https://www.pushcut.io) API.

## Features

- Send customizable push notifications to your iOS devices
- Support for notification actions and default actions
- Time-sensitive notifications
- Delayed notifications
- Input validation with helpful error messages

## Usage

### Basic Example

```yaml
- uses: lzilioli/pushcut-notify-action@v1
  with:
    api-key: ${{ secrets.PUSHCUT_API_KEY }}
    notification-name: 'Deployment'
    title: 'Deployment Complete'
    text: 'Your application has been successfully deployed!'
```

### Advanced Example with Actions

```yaml
- uses: lzilioli/pushcut-notify-action@v1
  with:
    api-key: ${{ secrets.PUSHCUT_API_KEY }}
    notification-name: 'JSONBasedConfig'
    title: '🚀 Deployment to Production'
    text: 'Your site has been deployed successfully!'
    time-sensitive: 'true'
    actions: |
      [
        {"name": "View Site", "url": "https://example.com", "keepNotification": true},
        {"name": "View Logs", "url": "https://github.com/owner/repo/actions", "keepNotification": true},
        {"name": "Apple Site (with drafts)", "url": "https://apple.lukezilioli.com/?showDrafts=true", "keepNotification": true}
      ]
    default-action: |
      {"url": "https://example.com"}
```

### Delayed Notification Example

```yaml
- uses: lzilioli/pushcut-notify-action@v1
  with:
    api-key: ${{ secrets.PUSHCUT_API_KEY }}
    notification-name: 'Reminder'
    title: 'Follow-up Required'
    text: 'Remember to check the deployment status'
    delay: '30m'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api-key` | Your Pushcut API key | **Yes** | - |
| `notification-name` | The name of the notification as configured in your Pushcut app | **Yes** | - |
| `title` | The notification title | **Yes** | - |
| `text` | The notification body text | **Yes** | - |
| `actions` | JSON array of action objects | No | `[]` |
| `default-action` | JSON object for the default action | No | `{}` |
| `time-sensitive` | Mark notification as time-sensitive (`true`/`false`) | No | `false` |
| `delay` | Delay before sending (e.g., `1h`, `30m`, `1h 5m 10s`) | No | - |

## Setup

1. Install the [Pushcut app](https://apps.apple.com/app/pushcut/id1450936447) on your iOS device
2. Create a notification configuration in the app
3. Generate an API key in Pushcut settings
4. Add your API key as a GitHub secret (e.g., `PUSHCUT_API_KEY`)
5. Use the action in your workflows!

## Action Objects

Action objects support the following properties:

```json
{
  "name": "Action Name",
  "url": "https://example.com",
  "keepNotification": true,
  "shortcut": "Shortcut Name"
}
```

## Default Action Object

The default action is triggered when the user taps the notification:

```json
{
  "url": "https://example.com",
  "shortcut": "Shortcut Name"
}
```

## Error Handling

The action will fail with a non-zero exit code if:
- The Pushcut API returns an error
- Invalid JSON is provided for actions or default-action
- Required inputs are missing

## License

MIT