---
order: 100
icon: person-fill
---

# Characters

Characters are the AI identities that you can create and manage to shape the AI's role in the conversation. Each
character has a name, personality, and conversation history. You can create as many characters as you like, and
switch between them at any time.

Characters can be used in solo chats, or add multiple characters to a group chat to
let them interact with each other.

## Character Management Panel

Open the <i class="fa-solid fa-address-card"></i> **Characters** panel from the navbar to access the character
list. Click on a character or group to chat with them or edit them, or
choose <i class="fa-solid fa-user-plus"></i> **Create New Character** to add a new character.

### Panel Controls

* <i class="fa-solid fa-lock"></i> **Pin Panel**: Keep panel open while interacting
* <i class="fa-solid fa-list-ul"></i> **Character List**: Return to character list view
* **HotSwap Bar**: Quick access to favorite characters

### Character List

* <i class="fa-solid fa-user-plus"></i> **Create New Character**: Add a new character
* <i class="fa-solid fa-file-import"></i> **Import Character**: Load character from file
* <i class="fa-solid fa-cloud-arrow-down"></i> **External Import**: Import from URL
* <i class="fa-solid fa-users-gear"></i> **Create Group**: Start a new group chat

#### Search and sort

* **Search Bar**: Filter characters by name or attributes
* **Sort Dropdown**: Multiple sorting options:
    - Alphabetical (A-Z, Z-A)
    - Chronological (Newest, Oldest)
    - Usage-based (Recent, Most/Least chats)
    - Size-based (Most/Least tokens)
    - Special (Favorites, Random)

#### Filter characters by type or tag

* <i class="fa-solid fa-star"></i> **Favorites Filter**: Show favorite characters
* <i class="fa-solid fa-users"></i> **Groups Filter**: Show only group chats
* <i class="fa-solid fa-folder-plus"></i> **Tags as Folders**: Organize by tag hierarchy
* <i class="fa-solid fa-gear"></i> **Manage Tags**: [Tag configuration](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **Tag List**: View all available tags
* <i class="fa-solid fa-filter-circle-xmark"></i> **Clear Filters**: Reset all filters

### Character Creation/Edit Panel

* **Avatar Image**: Upload and preview character profile picture
* **Token Count**: [Token usage](characterdesign.md#character-tokens) for the character
* <i class="fa-solid fa-ranking-star"></i> **Stats**: Chat history and usage statistics
* [Tag management](/Usage/Characters/Tags.md)

#### Quick Actions

- <i class="fa-solid fa-star"></i> Favorite toggle
- <i class="fa-solid fa-book"></i> Advanced definitions
- <i class="fa-solid fa-globe"></i> Character lore
- <i class="fa-solid fa-passport"></i> Chat lore: link the chat to a [World Info](/Usage/worldinfo.md)
- <i class="fa-solid fa-file-export"></i> Export character
- <i class="fa-solid fa-clone"></i> Duplicate
- <i class="fa-solid fa-skull"></i> Delete

#### Extended Options

* World Info linking
* Card lore import
* Scenario override
* Persona conversion
* Character rename
* Source linking
* Replace/Update
* Tag import
* Gallery view

#### Content Fields

* **[Character Description](characterdesign.md#character-description)**: Brief character summary
* **[First Message](characterdesign.md#first-message)**: Initial greeting or prompt when starting a new chat
* **Alternative greetings**: Define multiple first messages that you can swipe between when starting a chat

### Advanced Definitions Panel

Click on the <i class="fa-solid fa-book"></i> **Advanced Definitions** button to access the extended character settings.

#### Prompt Overrides (Chat Completion/Instruct Mode)

* **Main Prompt**: Replaces default [main/system prompt](/Usage/Prompts/prompts.md#main-prompt-system-prompt), can use
  \{\{original\}\} placeholder to include the original prompt
* **Post-History Instructions**: Overrides
  default [post-history instructions](/Usage/Prompts/prompts.md#post-history-instructions)

#### Creator's Metadata

Non-prompt information about the character:

- Creator name/contact
- Character version
- Creator's notes
- Embedded tags list

#### Character Personality

* **[Personality Summary](characterdesign.md#personality-summary)**: Brief overview of character's traits
* **[Scenario](characterdesign.md#scenario)**: Context and circumstances of the dialog
* **Character's Note**: Custom message with selectable depth and message role (also
  see [Author's Note](/Usage/Characters/Author's-Note.md))
* **Talkativeness** (Group Chats): Slider for Shy → Normal → Chatty
* **Example Messages**: Examples of character's writing style

### Group Chat Management

If this is a group chat, you can manage the group members and settings from this panel.

See [Group Chats](/Usage/Characters/groupchats.md) for more details.
