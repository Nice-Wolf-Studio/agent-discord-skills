# Discord List Channels - Examples

## Example 1: List All Channels (Simple)

**User Request:**
> List all channels in Discord server 123456789012345678

**Skill Actions:**
1. Validate `DISCORD_BOT_TOKEN` is set
2. Execute API request:

```bash
curl -X GET "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}"
```

**Expected Response:**
```json
[
  {
    "id": "111222333444555666",
    "type": 0,
    "name": "general",
    "position": 0,
    "parent_id": null
  },
  {
    "id": "111222333444555667",
    "type": 2,
    "name": "voice-chat",
    "position": 1,
    "parent_id": null
  },
  {
    "id": "111222333444555668",
    "type": 4,
    "name": "Community",
    "position": 2,
    "parent_id": null
  }
]
```

**User Feedback:**
```
Found 3 channels in server 123456789012345678:

💬 general (Text) - ID: 111222333444555666
🔊 voice-chat (Voice) - ID: 111222333444555667
📁 Community (Category) - ID: 111222333444555668
```

---

## Example 2: List Channels with Details

**User Request:**
> Show me all channels in server 123456789012345678 with their topics and details

**Skill Actions:**
```bash
curl -s -X GET "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq -r '.[] | "Name: \(.name)\nType: \(.type)\nID: \(.id)\nTopic: \(.topic // "None")\n"'
```

**User Feedback:**
```
=== Channels in Server 123456789012345678 ===

💬 general (Text Channel)
ID: 111222333444555666
Topic: General discussion for all members
Position: 0

📢 announcements (Announcement Channel)
ID: 111222333444555667
Topic: Official server announcements
Position: 1

🔊 voice-lounge (Voice Channel)
ID: 111222333444555668
User Limit: 10
Bitrate: 64kbps
Position: 2

📁 Community (Category)
ID: 111222333444555669
Position: 3
```

---

## Example 3: Filter by Channel Type (Text Channels Only)

**User Request:**
> Show me only the text channels in server 123456789012345678

**Skill Actions:**
```bash
curl -s -X GET "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq '.[] | select(.type == 0)'
```

**User Feedback:**
```
Text channels in server 123456789012345678:

💬 general - ID: 111222333444555666
💬 random - ID: 111222333444555670
💬 help - ID: 111222333444555671
💬 bot-commands - ID: 111222333444555672

Total: 4 text channels
```

---

## Example 4: Filter by Channel Type (Voice Channels Only)

**User Request:**
> Show me all voice channels in server 123456789012345678

**Skill Actions:**
```bash
curl -s -X GET "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq '.[] | select(.type == 2 or .type == 13)'
```

**User Feedback:**
```
Voice channels in server 123456789012345678:

🔊 voice-lounge - ID: 111222333444555668 (Voice)
🔊 gaming - ID: 111222333444555673 (Voice)
🎙️ community-stage - ID: 111222333444555674 (Stage)

Total: 3 voice/stage channels
```

---

## Example 5: List Channels Grouped by Category

**User Request:**
> Show me all channels organized by category in server 123456789012345678

**Skill Actions:**
```bash
CHANNELS=$(curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}")

# Display organized by category
echo "$CHANNELS" | jq -r '
  group_by(.parent_id) |
  .[] |
  "Category: " + (.[0].parent_id // "No Category") + "\n" +
  (map("  - " + .name + " (" + (.type | tostring) + ")") | join("\n"))
'
```

**User Feedback:**
```
=== Channels in Server 123456789012345678 ===

📁 Community (Category - ID: 111222333444555669)
  💬 general-chat
  💬 introductions
  🔊 voice-room-1

📁 Gaming (Category - ID: 111222333444555675)
  💬 game-discussion
  💬 looking-for-group
  🔊 gaming-voice

Uncategorized Channels:
  💬 welcome
  📢 announcements
  🔊 afk-voice
```

---

## Example 6: Find Channel by Name

**User Request:**
> Find the channel ID for "general" in server 123456789012345678

**Skill Actions:**
```bash
curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq '.[] | select(.name == "general")'
```

**User Feedback:**
```
Found channel "general":
ID: 111222333444555666
Type: Text Channel
Topic: General discussion for all members
Position: 0
```

---

## Example 7: Search Channels by Partial Name

**User Request:**
> Find all channels with "game" in the name in server 123456789012345678

**Skill Actions:**
```bash
curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq '.[] | select(.name | contains("game"))'
```

**User Feedback:**
```
Found 3 channels matching "game":

💬 game-discussion - ID: 111222333444555676
🔊 gaming-voice - ID: 111222333444555677
📁 Gaming (Category) - ID: 111222333444555675
```

---

## Example 8: List Channels in Specific Category

**User Request:**
> Show me all channels in the "Community" category (ID: 111222333444555669) in server 123456789012345678

**Skill Actions:**
```bash
curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq '.[] | select(.parent_id == "111222333444555669")'
```

**User Feedback:**
```
Channels in "Community" category:

💬 general-chat - ID: 111222333444555678
💬 introductions - ID: 111222333444555679
💬 off-topic - ID: 111222333444555680
🔊 voice-room-1 - ID: 111222333444555681

Total: 4 channels in this category
```

---

## Example 9: Export Channel List as CSV

**User Request:**
> Export all channels from server 123456789012345678 as CSV format

**Skill Actions:**
```bash
echo "Channel ID,Name,Type,Category ID,Topic" > channels.csv

curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq -r '.[] | [.id, .name, .type, (.parent_id // ""), (.topic // "")] | @csv' >> channels.csv
```

**User Feedback:**
```
Exported channel list to channels.csv:

Channel ID,Name,Type,Category ID,Topic
111222333444555666,general,0,,General discussion
111222333444555667,announcements,5,,Server updates
111222333444555668,voice-lounge,2,111222333444555669,
111222333444555670,random,0,111222333444555669,Random chat

Total: 15 channels exported
```

---

## Example 10: Count Channels by Type

**User Request:**
> How many channels of each type are in server 123456789012345678?

**Skill Actions:**
```bash
curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq 'group_by(.type) | map({type: .[0].type, count: length})'
```

**User Feedback:**
```
Channel count by type in server 123456789012345678:

💬 Text Channels: 8
🔊 Voice Channels: 4
📁 Categories: 3
📢 Announcement Channels: 1
🎙️ Stage Channels: 1

Total: 17 channels
```

---

## Example 11: List NSFW Channels

**User Request:**
> Show me all NSFW channels in server 123456789012345678

**Skill Actions:**
```bash
curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq '.[] | select(.nsfw == true)'
```

**User Feedback:**
```
NSFW channels in server 123456789012345678:

🔞 nsfw-content - ID: 111222333444555682 (Text)
🔞 nsfw-media - ID: 111222333444555683 (Text)

Total: 2 NSFW channels
```

---

## Example 12: Sort Channels by Position

**User Request:**
> List all channels in server 123456789012345678 sorted by their position

**Skill Actions:**
```bash
curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq 'sort_by(.position)'
```

**User Feedback:**
```
Channels sorted by position:

Position 0: 💬 welcome
Position 1: 💬 rules
Position 2: 📢 announcements
Position 3: 📁 Community
Position 4: 💬 general-chat
Position 5: 💬 help
...
```

---

## Example 13: Error - Bot Not in Server

**User Request:**
> List channels in server 999999999999999999

**API Response:**
```json
{
  "code": 10004,
  "message": "Unknown Guild"
}
```

**User Feedback:**
> Error: Server not found (ID: 999999999999999999).
>
> Please verify:
> - The server ID is correct
> - The bot is a member of this server
> - The server hasn't been deleted

---

## Example 14: Error - Missing Permissions

**User Request:**
> List channels in server 123456789012345678

**API Response:**
```json
{
  "code": 50013,
  "message": "Missing Permissions"
}
```

**User Feedback:**
> Error: Unable to list channels. The bot is missing permissions to view channels in this server.
>
> This usually means:
> - The bot needs the "View Channels" permission
> - The bot may be restricted by role permissions
> - Check the bot's permissions in Server Settings

---

## Example 15: List Announcement Channels for Following

**User Request:**
> Show me all announcement channels I can follow from server 123456789012345678

**Skill Actions:**
```bash
curl -s "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" | \
  jq '.[] | select(.type == 5)'
```

**User Feedback:**
```
Announcement channels in server 123456789012345678:

📢 server-updates - ID: 111222333444555684
  Topic: Official server announcements and updates
  Can be followed from other servers

📢 patch-notes - ID: 111222333444555685
  Topic: Game patch notes and changes
  Can be followed from other servers

Total: 2 announcement channels available for following
```

---

## Best Practices from Examples

1. **Show Channel Icons** - Use emojis to visually distinguish channel types
2. **Include IDs** - Always show channel IDs for reference in other operations
3. **Group Logically** - Organize by categories when displaying
4. **Filter Efficiently** - Use jq for client-side filtering
5. **Handle Empty Results** - Check for servers with no channels
6. **Sort Appropriately** - Use position field to match Discord's ordering
7. **Export Options** - Provide CSV/JSON export for documentation
8. **Count Summary** - Show totals when listing many channels
9. **Respect Hierarchy** - Display parent-child relationships for categories
10. **Show Metadata** - Include topics, NSFW status, and other relevant details
