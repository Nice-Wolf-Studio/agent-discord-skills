# Discord Create Channel - Examples

## Example 1: Create Basic Text Channel

**User Request:**
> Create a text channel called "general-discussion" in Discord server 123456789012345678

**Skill Actions:**
1. Validate `DISCORD_BOT_TOKEN` is set
2. Validate guild ID format
3. Sanitize channel name (convert to lowercase, replace spaces with hyphens)
4. Execute API request:

```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "general-discussion",
    "type": 0
  }'
```

**Expected Response:**
```json
{
  "id": "987654321098765432",
  "type": 0,
  "guild_id": "123456789012345678",
  "position": 5,
  "permission_overwrites": [],
  "name": "general-discussion",
  "topic": null,
  "nsfw": false,
  "last_message_id": null,
  "parent_id": null
}
```

**User Feedback:**
> Successfully created text channel "general-discussion" in server 123456789012345678.
> Channel ID: 987654321098765432
> Channel URL: https://discord.com/channels/123456789012345678/987654321098765432

---

## Example 2: Create Text Channel with Topic

**User Request:**
> Create a channel called "announcements" with topic "Official server announcements and updates"

**Skill Actions:**
```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "announcements",
    "type": 0,
    "topic": "Official server announcements and updates"
  }'
```

**User Feedback:**
> Successfully created text channel "announcements" with topic.
> Channel ID: 987654321098765433

---

## Example 3: Create Voice Channel

**User Request:**
> Create a voice channel called "Voice Lounge" in server 123456789012345678

**Skill Actions:**
1. Sanitize name: "Voice Lounge" → "voice-lounge"
2. Set type to 2 (voice channel)

```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "voice-lounge",
    "type": 2
  }'
```

**User Feedback:**
> Successfully created voice channel "voice-lounge".
> Channel ID: 987654321098765434
> Members can now join this voice channel in the server.

---

## Example 4: Create Voice Channel with User Limit

**User Request:**
> Create a voice channel called "meeting-room" with a limit of 5 users in server 123456789012345678

**Skill Actions:**
```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "meeting-room",
    "type": 2,
    "user_limit": 5
  }'
```

**User Feedback:**
> Successfully created voice channel "meeting-room" with user limit of 5.
> Channel ID: 987654321098765435

---

## Example 5: Create Category

**User Request:**
> Create a category called "Community Channels" in server 123456789012345678

**Skill Actions:**
```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Community Channels",
    "type": 4
  }'
```

**Expected Response:**
```json
{
  "id": "987654321098765436",
  "type": 4,
  "guild_id": "123456789012345678",
  "position": 0,
  "permission_overwrites": [],
  "name": "Community Channels",
  "nsfw": false
}
```

**User Feedback:**
> Successfully created category "Community Channels".
> Category ID: 987654321098765436
> You can now add channels to this category.

---

## Example 6: Create Channel Inside Category

**User Request:**
> Create a text channel called "chat" inside category 987654321098765436 in server 123456789012345678

**Skill Actions:**
```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "chat",
    "type": 0,
    "parent_id": "987654321098765436"
  }'
```

**User Feedback:**
> Successfully created text channel "chat" inside the "Community Channels" category.
> Channel ID: 987654321098765437

---

## Example 7: Create Announcement Channel

**User Request:**
> Create an announcement channel called "server-updates" in server 123456789012345678

**Skill Actions:**
```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "server-updates",
    "type": 5,
    "topic": "Follow this channel to get updates in other servers"
  }'
```

**User Feedback:**
> Successfully created announcement channel "server-updates".
> Channel ID: 987654321098765438
> Note: Users can follow this channel from other servers to receive updates.

---

## Example 8: Create NSFW Channel

**User Request:**
> Create an NSFW text channel called "nsfw-content" in server 123456789012345678

**Skill Actions:**
```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "nsfw-content",
    "type": 0,
    "nsfw": true
  }'
```

**User Feedback:**
> Successfully created NSFW text channel "nsfw-content".
> Channel ID: 987654321098765439
> This channel is marked as age-restricted.

---

## Example 9: Create Stage Channel

**User Request:**
> Create a stage channel called "community-events" in server 123456789012345678

**Skill Actions:**
```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "community-events",
    "type": 13
  }'
```

**User Feedback:**
> Successfully created stage channel "community-events".
> Channel ID: 987654321098765440
> This channel can be used for large audio events with speakers and audience.

---

## Example 10: Create Private Channel with Permissions

**User Request:**
> Create a private channel called "moderators-only" that only moderators can see in server 123456789012345678. The moderator role ID is 111222333444555666.

**Skill Actions:**
```bash
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "moderators-only",
    "type": 0,
    "permission_overwrites": [
      {
        "id": "123456789012345678",
        "type": 0,
        "deny": "1024"
      },
      {
        "id": "111222333444555666",
        "type": 0,
        "allow": "1024"
      }
    ]
  }'
```

**Explanation of permissions:**
- First override: Deny @everyone role (guild_id) from viewing channel
- Second override: Allow moderator role to view channel

**User Feedback:**
> Successfully created private text channel "moderators-only".
> Channel ID: 987654321098765441
> Only users with the Moderator role can see and access this channel.

---

## Example 11: Error - Invalid Channel Name

**User Request:**
> Create a channel called "General Chat!" in server 123456789012345678

**Skill Actions:**
1. Detect invalid characters (space and exclamation mark)
2. Suggest sanitized name: "general-chat"
3. Ask user for confirmation

**User Feedback:**
> The channel name "General Chat!" contains invalid characters. Discord channel names must be lowercase letters, numbers, hyphens, and underscores only.
>
> Suggested name: "general-chat"
>
> Would you like me to create the channel with this name instead?

---

## Example 12: Error - Missing Permissions

**User Request:**
> Create a channel called "new-channel" in server 123456789012345678

**API Response:**
```json
{
  "code": 50013,
  "message": "Missing Permissions"
}
```

**User Feedback:**
> Error: Unable to create channel. The bot is missing the "Manage Channels" permission in this server.
>
> To fix this:
> 1. Go to Server Settings → Roles
> 2. Find the bot's role
> 3. Enable "Manage Channels" permission
> 4. Try again

---

## Example 13: Error - Channel Name Too Short

**User Request:**
> Create a channel called "a" in server 123456789012345678

**User Feedback:**
> Error: Channel name "a" is too short. Discord requires channel names to be between 2 and 100 characters.
>
> Please provide a longer name (at least 2 characters).

---

## Example 14: Error - Invalid Guild ID

**User Request:**
> Create a channel called "test" in server 999999999999999999

**API Response:**
```json
{
  "code": 10004,
  "message": "Unknown Guild"
}
```

**User Feedback:**
> Error: Guild/server not found (ID: 999999999999999999).
>
> Please verify:
> - The guild ID is correct
> - The bot is a member of this server
> - The server hasn't been deleted

---

## Example 15: Create Multiple Channels in Category

**User Request:**
> Create a "Gaming" category with three text channels: "general-gaming", "game-discussion", and "looking-for-group" in server 123456789012345678

**Skill Actions:**
1. First create the category
2. Then create each channel with parent_id set to category

```bash
# Step 1: Create category
CATEGORY_RESPONSE=$(curl -s -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Gaming",
    "type": 4
  }')

CATEGORY_ID=$(echo "$CATEGORY_RESPONSE" | jq -r '.id')

# Step 2: Create first channel
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d "{
    \"name\": \"general-gaming\",
    \"type\": 0,
    \"parent_id\": \"$CATEGORY_ID\"
  }"

# Step 3: Create second channel
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d "{
    \"name\": \"game-discussion\",
    \"type\": 0,
    \"parent_id\": \"$CATEGORY_ID\"
  }"

# Step 4: Create third channel
curl -X POST "https://discord.com/api/v10/guilds/123456789012345678/channels" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d "{
    \"name\": \"looking-for-group\",
    \"type\": 0,
    \"parent_id\": \"$CATEGORY_ID\"
  }"
```

**User Feedback:**
> Successfully created "Gaming" category with 3 channels:
> - general-gaming (ID: 987654321098765442)
> - game-discussion (ID: 987654321098765443)
> - looking-for-group (ID: 987654321098765444)
>
> All channels are organized under the Gaming category.

---

## Best Practices from Examples

1. **Sanitize Names** - Always convert to lowercase and replace invalid characters
2. **Validate Before Creating** - Check name length and character validity
3. **Use Categories** - Organize related channels in categories
4. **Set Permissions Early** - Configure permissions during creation when possible
5. **Provide Channel URLs** - Include Discord URLs for easy access
6. **Handle Errors Gracefully** - Provide clear, actionable error messages
7. **Confirm Destructive Actions** - Ask before creating many channels at once
8. **Document Channel Purpose** - Use the topic field to explain channel usage
