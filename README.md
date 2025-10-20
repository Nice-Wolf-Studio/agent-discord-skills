# Discord API Skills for Claude Code

A comprehensive collection of Claude Code skills for interacting with the Discord API. These skills enable Claude to manage Discord servers, channels, and messages through natural language interactions.

## Features

- **Message Management**: Send and retrieve messages from Discord channels
- **Channel Operations**: Create, list, and manage Discord channels
- **Bot Token Authentication**: Secure authentication using environment variables
- **Discord API v10**: Built on the latest Discord API version

## Available Skills

### 1. discord-send-message
Send messages to Discord channels with support for text content, embeds, and formatting.

**Use cases:**
- Send notifications to Discord channels
- Post automated updates
- Send formatted messages with embeds

### 2. discord-get-messages
Retrieve messages from Discord channels with pagination and filtering options.

**Use cases:**
- Read channel history
- Search for specific messages
- Monitor channel activity

### 3. discord-create-channel
Create new channels in a Discord guild/server.

**Use cases:**
- Set up new discussion channels
- Create voice channels for meetings
- Organize server structure

### 4. discord-list-channels
List all channels in a Discord guild with filtering by type.

**Use cases:**
- Audit server channel structure
- Find specific channels
- Export channel lists

### 5. discord-manage-channel
Update channel properties, manage permissions, and delete channels.

**Use cases:**
- Update channel names and topics
- Modify channel permissions
- Archive or delete channels

## Installation

1. **Clone this repository** to your Claude Code skills directory:

```bash
# Default Claude Code skills directory
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/Nice-Wolf-Studio/agent-discord-skills.git
cd agent-discord-skills
```

2. **Set up your Discord Bot Token**:

```bash
export DISCORD_BOT_TOKEN="your-bot-token-here"
```

For persistent configuration, add this to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
echo 'export DISCORD_BOT_TOKEN="your-bot-token-here"' >> ~/.zshrc
source ~/.zshrc
```

## Getting a Discord Bot Token

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications)
2. Click "New Application" and give it a name
3. Navigate to the "Bot" section in the left sidebar
4. Click "Add Bot"
5. Under the "TOKEN" section, click "Reset Token" and copy it
6. Save this token securely - you'll need it for the `DISCORD_BOT_TOKEN` environment variable

### Bot Permissions

When adding your bot to a server, it needs the following permissions:
- `Send Messages` (2048)
- `Read Message History` (65536)
- `Manage Channels` (16)
- `View Channels` (1024)

**OAuth2 URL Generator:**
Use the Discord Developer Portal OAuth2 URL Generator with these scopes:
- `bot`
- `applications.commands` (optional, for future slash command support)

## Usage

Once installed, Claude Code will automatically detect when to use these skills based on your requests.

**Example interactions:**

```
You: Send a message to Discord channel 123456789 saying "Hello from Claude!"

You: Get the last 10 messages from channel 987654321

You: Create a new text channel called "general-chat" in guild 111222333

You: List all channels in my Discord server 111222333

You: Update the channel name of 123456789 to "announcements"
```

## Requirements

- Claude Code (latest version)
- Discord Bot Token with appropriate permissions
- Access to a Discord server where the bot is installed

## Discord API Version

These skills use Discord API v10 (`https://discord.com/api/v10`).

## Security Notes

- **Never commit your bot token** to version control
- Store the token in environment variables only
- Use bot accounts, not user accounts
- Follow Discord's [Terms of Service](https://discord.com/terms) and [Developer Terms](https://discord.com/developers/docs/policies-and-agreements/developer-terms-of-service)

## Development

To contribute or modify these skills:

```bash
git clone https://github.com/Nice-Wolf-Studio/agent-discord-skills.git
cd agent-discord-skills
npm install  # Install dev dependencies
```

## Skill Structure

Each skill follows the Claude Code skill format:

```
skill-name/
├── SKILL.md       # Main skill instructions with YAML frontmatter
└── examples.md    # Usage examples and scenarios
```

## Testing

Before using these skills in production:

1. Create a test Discord server
2. Add your bot to the test server
3. Test each skill with the test server's IDs
4. Verify error handling with invalid inputs

## Troubleshooting

### "Authentication Failed" Error
- Verify `DISCORD_BOT_TOKEN` is set correctly
- Check that your bot token hasn't expired
- Ensure the bot is added to the target server

### "Missing Permissions" Error
- Verify bot has required permissions in the Discord server
- Check channel-specific permission overrides
- Ensure bot role is positioned correctly in role hierarchy

### "Unknown Channel" Error
- Verify the channel ID is correct
- Ensure bot has access to view the channel
- Check that the channel exists in the specified guild

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Submit a pull request

## License

MIT License - See [LICENSE](LICENSE) file for details

## Resources

- [Discord Developer Portal](https://discord.com/developers/docs/intro)
- [Discord API Documentation](https://discord.com/developers/docs/reference)
- [Claude Code Skills Documentation](https://docs.claude.com/en/docs/claude-code/skills)
- [Anthropic Skills Repository](https://github.com/anthropics/skills)

## Support

For issues and questions:
- GitHub Issues: [agent-discord-skills/issues](https://github.com/Nice-Wolf-Studio/agent-discord-skills/issues)
- Discord API Support: [Discord Developers Server](https://discord.gg/discord-developers)

---

Built with ❤️ by [Nice Wolf Studio](https://github.com/Nice-Wolf-Studio)
