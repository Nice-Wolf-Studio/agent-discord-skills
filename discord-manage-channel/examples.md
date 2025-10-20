# Discord Manage Channel - Examples

## Example 1: Rename Channel

**User Request:**
> Rename channel 123456789012345678 to "general-discussion"

**Skill Actions:**
1. Validate `DISCORD_BOT_TOKEN` is set
2. Validate new name follows Discord rules
3. Execute API request:

```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "general-discussion"
  }'
```

**Expected Response:**
```json
{
  "id": "123456789012345678",
  "type": 0,
  "name": "general-discussion",
  "topic": "General chat",
  "guild_id": "987654321098765432",
  "position": 1
}
```

**User Feedback:**
> Successfully renamed channel to "general-discussion".
> Channel ID: 123456789012345678

---

## Example 2: Update Channel Topic

**User Request:**
> Update the topic of channel 123456789012345678 to "Discuss anything related to our community"

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "topic": "Discuss anything related to our community"
  }'
```

**User Feedback:**
> Successfully updated channel topic.
> New topic: "Discuss anything related to our community"

---

## Example 3: Move Channel to Category

**User Request:**
> Move channel 123456789012345678 to category 111222333444555666

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "parent_id": "111222333444555666"
  }'
```

**User Feedback:**
> Successfully moved channel to category 111222333444555666.
> The channel will now appear under this category in the server.

---

## Example 4: Remove Channel from Category

**User Request:**
> Remove channel 123456789012345678 from its category

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "parent_id": null
  }'
```

**User Feedback:**
> Successfully removed channel from category.
> The channel is now at the top level of the server.

---

## Example 5: Toggle NSFW Status

**User Request:**
> Mark channel 123456789012345678 as NSFW

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "nsfw": true
  }'
```

**User Feedback:**
> Successfully marked channel as NSFW.
> Users will now see an age restriction warning before entering.

---

## Example 6: Update Voice Channel User Limit

**User Request:**
> Set the user limit to 5 for voice channel 123456789012345678

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "user_limit": 5
  }'
```

**User Feedback:**
> Successfully set user limit to 5 for voice channel.
> Maximum 5 users can join this voice channel at once.

---

## Example 7: Update Voice Channel Bitrate

**User Request:**
> Set the bitrate to 96kbps for voice channel 123456789012345678

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "bitrate": 96000
  }'
```

**User Feedback:**
> Successfully set bitrate to 96kbps.
> Voice quality has been increased (requires server boost level).

---

## Example 8: Update Multiple Properties at Once

**User Request:**
> Rename channel 123456789012345678 to "announcements" and update the topic to "Official server announcements"

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "announcements",
    "topic": "Official server announcements"
  }'
```

**User Feedback:**
> Successfully updated channel:
> - Name: announcements
> - Topic: Official server announcements

---

## Example 9: Update Channel Position

**User Request:**
> Move channel 123456789012345678 to position 0 (top of the list)

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "position": 0
  }'
```

**User Feedback:**
> Successfully moved channel to position 0.
> The channel now appears at the top of the channel list.

---

## Example 10: Update Channel Permissions - Make Private

**User Request:**
> Make channel 123456789012345678 private (only admins can see it). Admin role ID is 111222333444555666, server ID is 987654321098765432.

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "permission_overwrites": [
      {
        "id": "987654321098765432",
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

**Explanation:**
- First override: Deny @everyone (guild ID) VIEW_CHANNEL (1024)
- Second override: Allow admin role VIEW_CHANNEL (1024)

**User Feedback:**
> Successfully made channel private.
> Only users with the Admin role can now view this channel.

---

## Example 11: Update Channel Permissions - Read-Only

**User Request:**
> Make channel 123456789012345678 read-only for @everyone. Server ID is 987654321098765432.

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "permission_overwrites": [
      {
        "id": "987654321098765432",
        "type": 0,
        "allow": "1024",
        "deny": "2048"
      }
    ]
  }'
```

**Explanation:**
- Allow VIEW_CHANNEL (1024)
- Deny SEND_MESSAGES (2048)

**User Feedback:**
> Successfully made channel read-only.
> Users can view messages but cannot send new ones.

---

## Example 12: Clear All Permission Overwrites

**User Request:**
> Reset all custom permissions on channel 123456789012345678

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "permission_overwrites": []
  }'
```

**User Feedback:**
> Successfully cleared all permission overwrites.
> Channel now uses default server/category permissions.

---

## Example 13: Delete Channel (with Confirmation)

**User Request:**
> Delete channel 123456789012345678

**Skill Actions:**
1. First, ask for confirmation (using AskUserQuestion or similar)
2. If confirmed, execute delete:

```bash
curl -X DELETE "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}"
```

**User Feedback (Confirmation Request):**
> ⚠️ Warning: You are about to delete channel 123456789012345678.
>
> This action is PERMANENT and cannot be undone. All messages in this channel will be lost.
>
> Are you sure you want to delete this channel? (yes/no)

**User Feedback (After Confirmation):**
> Channel 123456789012345678 has been permanently deleted.
> All messages and data in that channel have been removed.

---

## Example 14: Error - Invalid Channel Name

**User Request:**
> Rename channel 123456789012345678 to "General Chat!"

**Skill Actions:**
1. Detect invalid characters (space and !)
2. Suggest corrected name

**User Feedback:**
> Error: Invalid channel name "General Chat!".
>
> Discord channel names must:
> - Be 2-100 characters long
> - Use lowercase letters, numbers, hyphens, and underscores only
> - Not contain spaces or special characters
>
> Suggested name: "general-chat"
>
> Would you like to use this name instead?

---

## Example 15: Error - Missing Permissions

**User Request:**
> Update channel 123456789012345678 topic

**API Response:**
```json
{
  "code": 50013,
  "message": "Missing Permissions"
}
```

**User Feedback:**
> Error: Unable to update channel. The bot is missing the "Manage Channels" permission.
>
> To fix this:
> 1. Go to Server Settings → Roles
> 2. Find the bot's role
> 3. Enable "Manage Channels" permission
> 4. Try again

---

## Example 16: Error - Channel Not Found

**User Request:**
> Update channel 999999999999999999

**API Response:**
```json
{
  "code": 10003,
  "message": "Unknown Channel"
}
```

**User Feedback:**
> Error: Channel not found (ID: 999999999999999999).
>
> Please verify:
> - The channel ID is correct
> - The channel hasn't been deleted
> - The bot has access to this channel

---

## Example 17: Error - Topic Too Long

**User Request:**
> Set channel topic to a 2000 character string

**API Response:**
```json
{
  "code": 50035,
  "errors": {
    "topic": {
      "_errors": [{
        "code": "BASE_TYPE_MAX_LENGTH",
        "message": "Must be 1024 or fewer in length."
      }]
    }
  }
}
```

**User Feedback:**
> Error: Channel topic is too long (2000 characters).
>
> Channel topics must be 1024 characters or fewer.
> Current length: 2000 characters
> Maximum length: 1024 characters
>
> Please provide a shorter topic.

---

## Example 18: Batch Update Multiple Channels

**User Request:**
> Move channels 123456789012345678, 123456789012345679, and 123456789012345680 to category 111222333444555666

**Skill Actions:**
```bash
# Update first channel
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"parent_id": "111222333444555666"}'

# Wait 1 second for rate limiting
sleep 1

# Update second channel
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345679" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"parent_id": "111222333444555666"}'

# Wait 1 second
sleep 1

# Update third channel
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345680" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"parent_id": "111222333444555666"}'
```

**User Feedback:**
> Successfully moved 3 channels to category 111222333444555666:
> - Channel 123456789012345678 ✓
> - Channel 123456789012345679 ✓
> - Channel 123456789012345680 ✓

---

## Example 19: Remove Voice Channel User Limit

**User Request:**
> Remove the user limit from voice channel 123456789012345678

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "user_limit": 0
  }'
```

**User Feedback:**
> Successfully removed user limit.
> The voice channel now allows unlimited users.

---

## Example 20: Comprehensive Channel Update

**User Request:**
> Update channel 123456789012345678: rename to "community-hub", set topic to "Central community discussion", move to category 111222333444555666, and make it non-NSFW

**Skill Actions:**
```bash
curl -X PATCH "https://discord.com/api/v10/channels/123456789012345678" \
  -H "Authorization: Bot ${DISCORD_BOT_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "community-hub",
    "topic": "Central community discussion",
    "parent_id": "111222333444555666",
    "nsfw": false
  }'
```

**User Feedback:**
> Successfully updated channel with the following changes:
> ✓ Name: community-hub
> ✓ Topic: Central community discussion
> ✓ Moved to category: 111222333444555666
> ✓ NSFW status: disabled
>
> Channel ID: 123456789012345678

---

## Best Practices from Examples

1. **Validate Input** - Check names, topics, and IDs before making requests
2. **Confirm Deletions** - Always ask for confirmation before deleting
3. **Batch Carefully** - Add delays between batch updates (1 second)
4. **Combine Updates** - Update multiple properties in one request when possible
5. **Provide Clear Feedback** - List all changes made
6. **Handle Errors Gracefully** - Provide actionable error messages
7. **Sanitize Names** - Convert to lowercase, replace invalid characters
8. **Document Permissions** - Explain permission changes clearly
9. **Warn About Limits** - Check bitrate and user limit constraints
10. **Preserve Data** - Warn about data loss before destructive actions