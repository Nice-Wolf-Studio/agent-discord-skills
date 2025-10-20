# Discord Get Messages - Examples

## Example 1: Get Recent Messages (Default)

**User Request:**
> Get the last 10 messages from Discord channel 123456789012345678

**Skill Actions:**
1. Validate `DISCORD_BOT_TOKEN` is set
2. Validate channel ID format
3. Execute API request:

```bash
curl -X GET "https://discord.com/api/v10/channels/123456789012345678/messages?limit=10" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}"
```

**Expected Response:**
```json
[
  {
    "id": "999888777666555444",
    "content": "Most recent message",
    "author": {
      "id": "111222333444555666",
      "username": "User1",
      "discriminator": "0001"
    },
    "timestamp": "2025-10-20T12:05:00.000000+00:00"
  },
  {
    "id": "999888777666555443",
    "content": "Second most recent",
    "author": {
      "id": "111222333444555667",
      "username": "User2",
      "discriminator": "0002"
    },
    "timestamp": "2025-10-20T12:04:00.000000+00:00"
  }
  // ... 8 more messages
]
```

**User Feedback:**
```
Retrieved 10 messages from channel 123456789012345678:

[2025-10-20 12:05] User1: Most recent message
[2025-10-20 12:04] User2: Second most recent
[2025-10-20 12:03] User1: Another message
[2025-10-20 12:02] User3: Hello everyone
...
```

---

## Example 2: Get Specific Number of Messages

**User Request:**
> Get the last 50 messages from channel 123456789012345678

**Skill Actions:**
```bash
curl -X GET "https://discord.com/api/v10/channels/123456789012345678/messages?limit=50" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}"
```

**User Feedback:**
> Retrieved 50 messages from channel 123456789012345678.
> Messages span from 2025-10-20 08:00 to 2025-10-20 12:05.

---

## Example 3: Paginate Messages (Get Older Messages)

**User Request:**
> Get messages before message ID 999888777666555444 from channel 123456789012345678

**Skill Actions:**
```bash
curl -X GET "https://discord.com/api/v10/channels/123456789012345678/messages?before=999888777666555444&limit=50" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}"
```

**User Feedback:**
> Retrieved 50 messages from before message ID 999888777666555444.
> These are older messages in the channel history.

---

## Example 4: Get Messages After Specific Point

**User Request:**
> Get all new messages after message ID 999888777666555400 from channel 123456789012345678

**Skill Actions:**
```bash
curl -X GET "https://discord.com/api/v10/channels/123456789012345678/messages?after=999888777666555400&limit=100" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}"
```

**User Feedback:**
> Retrieved 23 new messages after message ID 999888777666555400.

---

## Example 5: Get Messages Around a Specific Message

**User Request:**
> Get messages around message ID 999888777666555420 from channel 123456789012345678

**Skill Actions:**
```bash
curl -X GET "https://discord.com/api/v10/channels/123456789012345678/messages?around=999888777666555420&limit=50" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}"
```

**User Feedback:**
> Retrieved 50 messages centered around message ID 999888777666555420.
> This includes ~25 messages before and ~25 messages after the target message.

---

## Example 6: Search Messages from Specific User

**User Request:**
> Get messages from user "JohnDoe" in channel 123456789012345678

**Skill Actions:**
1. Retrieve messages with limit
2. Filter by username

```bash
# Get messages
MESSAGES=$(curl -s -X GET "https://discord.com/api/v10/channels/123456789012345678/messages?limit=100" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}")

# Filter by username using jq
echo "$MESSAGES" | jq '.[] | select(.author.username == "JohnDoe")'
```

**User Feedback:**
```
Found 12 messages from user JohnDoe:

[2025-10-20 12:00] JohnDoe: First message
[2025-10-20 11:30] JohnDoe: Second message
[2025-10-20 11:00] JohnDoe: Third message
...
```

---

## Example 7: Search for Keywords

**User Request:**
> Find messages containing "meeting" in channel 123456789012345678

**Skill Actions:**
```bash
# Get messages
MESSAGES=$(curl -s -X GET "https://discord.com/api/v10/channels/123456789012345678/messages?limit=100" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}")

# Filter by content keyword
echo "$MESSAGES" | jq '.[] | select(.content | contains("meeting"))'
```

**User Feedback:**
```
Found 5 messages containing "meeting":

[2025-10-20 10:30] User1: Don't forget the meeting at 2pm
[2025-10-20 09:15] User2: Meeting agenda is ready
[2025-10-19 16:00] User3: Thanks for the great meeting today
...
```

---

## Example 8: Get Message Details Including Attachments

**User Request:**
> Get the last 20 messages including any attachments from channel 123456789012345678

**Skill Actions:**
```bash
curl -X GET "https://discord.com/api/v10/channels/123456789012345678/messages?limit=20" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}"
```

**Processing:**
Check each message for `attachments` array

**User Feedback:**
```
Retrieved 20 messages:

[2025-10-20 12:05] User1: Check out this screenshot
  📎 Attachment: screenshot.png (https://cdn.discordapp.com/attachments/...)

[2025-10-20 12:00] User2: Here's the document
  📎 Attachment: document.pdf (https://cdn.discordapp.com/attachments/...)

[2025-10-20 11:55] User3: Regular text message

...
```

---

## Example 9: Retrieve Message History (Pagination)

**User Request:**
> Get the last 250 messages from channel 123456789012345678

**Skill Actions:**
Since limit is max 100, use pagination:

```bash
# First batch (most recent 100)
BATCH1=$(curl -s "https://discord.com/api/v10/channels/123456789012345678/messages?limit=100" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}")

# Get oldest message ID from batch 1
OLDEST_ID=$(echo "$BATCH1" | jq -r '.[].id' | tail -1)

# Wait 1 second for rate limiting
sleep 1

# Second batch (next 100)
BATCH2=$(curl -s "https://discord.com/api/v10/channels/123456789012345678/messages?before=$OLDEST_ID&limit=100" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}")

# Get oldest message ID from batch 2
OLDEST_ID=$(echo "$BATCH2" | jq -r '.[].id' | tail -1)

# Wait 1 second
sleep 1

# Third batch (next 50)
BATCH3=$(curl -s "https://discord.com/api/v10/channels/123456789012345678/messages?before=$OLDEST_ID&limit=50" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}")
```

**User Feedback:**
> Retrieved 250 messages from channel 123456789012345678.
> Messages span from 2025-10-18 to 2025-10-20.
> Used 3 paginated requests to retrieve all messages.

---

## Example 10: Error Handling - Missing Permissions

**User Request:**
> Get messages from channel 123456789012345678

**API Response:**
```json
{
  "code": 50013,
  "message": "Missing Permissions"
}
```

**User Feedback:**
> Error: Unable to retrieve messages. The bot is missing the "Read Message History" permission in this channel. Please check the bot's permissions in your Discord server.

---

## Example 11: Export Channel History as Text

**User Request:**
> Export the last 100 messages from channel 123456789012345678 as a formatted text file

**Skill Actions:**
```bash
# Get messages
curl -s "https://discord.com/api/v10/channels/123456789012345678/messages?limit=100" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
jq -r '.[] | "[" + .timestamp + "] " + .author.username + ": " + .content' | \
tac > channel_history.txt
```

**User Feedback:**
> Exported 100 messages to `channel_history.txt`:
>
> ```
> [2025-10-20T12:05:00.000000+00:00] User1: Most recent message
> [2025-10-20T12:04:00.000000+00:00] User2: Second message
> [2025-10-20T12:03:00.000000+00:00] User1: Third message
> ...
> ```

---

## Example 12: Get Embeds from Messages

**User Request:**
> Get messages with embeds from channel 123456789012345678

**Skill Actions:**
```bash
# Get messages and filter for those with embeds
curl -s "https://discord.com/api/v10/channels/123456789012345678/messages?limit=50" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
jq '.[] | select(.embeds | length > 0)'
```

**User Feedback:**
```
Found 8 messages with embeds:

[2025-10-20 12:00] BotUser: [Embed: "Server Status"]
[2025-10-20 11:30] BotUser: [Embed: "Daily Report"]
...
```

---

## Best Practices from Examples

1. **Always specify a limit** - Default is 50, max is 100
2. **Use pagination for large datasets** - Break into batches of 100
3. **Respect rate limits** - Wait 1 second between paginated requests
4. **Filter after retrieval** - Use jq or similar tools for complex filtering
5. **Handle empty responses** - Check if array is empty before processing
6. **Format timestamps** - Convert ISO 8601 timestamps to readable format
7. **Check for attachments** - Always check the attachments array
8. **Preserve message order** - Use `tac` command to reverse chronological order for export
