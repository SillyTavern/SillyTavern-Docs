---
order: -40
icon: chevron-right
route: /usage/chatting/swipes/
---
# Swipes

**Swipes** allow you to regenerate messages while preserving the old versions.

### Basic Usage
* Swiping the last message enabled by default. To `Show Swipes for all Messages` you must enable it in `User Settings > Chat/Message Handling`.
* Clicking <i class="fa-solid fa-chevron-right"></i> on the last swipe will generate a new response and hide the old one to the "left".
* Clicking <i class="fa-solid fa-chevron-left"></i> or <i class="fa-solid fa-chevron-right"></i> will allow you to navigate between swipes.

#### Show Swipes for all Messages
!!!warning
Swiping on all messages is an experimental feature. You can disable it at any time.
!!!
[!embed height="500" width="500" ](/static/swipes/swipes-2025-09-29.mp4)
!!!info
When `Show Swipes for all Messages` is enabled, all messages can be swiped.
!!!

* Swiping any message will hide all messages after it.
* Swiping <i class="fa-solid fa-chevron-right"></i> on a user message will allow you to edit it before a generation is started.
* Alternate swipes are saved in first line of your chat file.
* Chat Tree exports must be created manually.

###### Swiping will switch all following messages, and restore the branch to the state you last left it in.
=== [Here's](https://github.com/SillyTavern/SillyTavern/issues/1731#issuecomment-1937843859) an explanation created by [aikitoria](https://github.com/aikitoria)
-![1. Current state|300](/static/swipes/1.png)
-![2. Swipe the first AI message backwards|300](/static/swipes/2.png)
-![3. Swipe the first AI message forwards|300](/static/swipes/3.png)
-![4. Swipe the first AI message forwards again|300](/static/swipes/4.png)
-![5. Swipe the first AI message backwards|300](/static/swipes/5.png)
===

## Developers
* Hidden swipes or "branches" are stored in the `chatTree`, which you can access with `getContext()`
```javascript
chatTree = {
  branch_id: 1,
  branch: [
    { mes: "Hi" },
    { mes: "Hello", branch_id: 0, branch: [...] },
  ]
}
```
* When a message is swiped, `chatTree` is updated via `saveChatToTree()`, then the next "branch" is loaded using `getChatFromTree()`.
* Examples of navigating the tree are in `public/scripts/chat-tree.js`.
* See [PR #4573](https://github.com/SillyTavern/SillyTavern/pull/4573) for more information.
