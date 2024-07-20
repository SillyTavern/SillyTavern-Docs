---
order: -99
---

# (OBSOLETE) Smart Context

**THIS EXTENSION IS NO LONGER MAINTAINED AND NOT RECOMMENDED TO USE. PLEASE SEEK ALTERNATIVES**

!!!warning
THE USE OF THIS PLUGIN DOESN'T GUARANTEE A BETTER CHATTING EXPERIENCE OR IMPROVED MEMORY OF ANY SORT. ONLY USE IF YOU UNDERSTAND ALL THE IMPLICATIONS OF VECTOR DATABASE UTILIZATION.
!!!

### What is it?

Smart Context is a SillyTavern extension that uses the [ChromaDB library](https://www.trychroma.com) to give your AI characters access to information that exists outside the normal chat history context limit.

### How is that useful?

If you have a very long chat, the majority of the contents are outside the usual context window and thus unavailable to the AI when it comes to writing a response.

Smart Context automatically takes the entire history of the chat file and puts it into a vector database. This database is then searched each time you input something new into the chat, and if messages with matching keywords are found, those chat messages are placed into the context so the AI can see them when writing its next reply.

***

### Setup Instructions

1. Update SillyTavern to at least version 1.10.6.
2. Install the "Smart Context" extension from the "Download Extensions & Assets" menu in the Extensions panel (stacked blocks icon).
3. Install or Update [Extras](https://github.com/SillyTavern/SillyTavern-extras) to the latest version. Alternatively, use the [Colab notebook](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb).
4. *Local installs only:* Install requirements-complete.txt for Extras (even if you did it once before in a prior install).
5. Run Extras with the chromadb module enabled: `python server.py --enable-modules=chromadb`

#### Getting an error when installing ChromaDB?

```
ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects
```

Installing chromadb package requires one of the following:

- Have Visual C++ build tools installed: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
- Installing hnswlib from conda: `conda install -c conda-forge hnswlib`

***

### Configuration

Once Smart Context is enabled, you should configure it in the SillyTavern UI.
Smart Context configuration can be done from within the Extensions menu ![STExtensionMenuIcon](/static/extensions/menu-icon.png)

![Smart Context Config Panel](/static/extensions/smart-context.png)

There are 4 main concepts to be aware of:

- Chat History Preservation
- Memory Injection Amount
- Individual Memory Length
- Injection Strategy

***

#### SmartContext only starts after 10 mesages are in the chat history

- At the start of a new chat, ChromaDB is inactive.
- Once the chat has accumulated 10 messages, it will begin recording all messages into the database, and recalling messages as needed.

#### Chat History Preservation ('kept mesages')

By default, ChromaDB will keep as many recent natural chat history messages as specified in the slider.
Any messages beyond this amount will be removed from your sent prompt, and if 'memories' exist in the database they will be added in place of the older chat history messages (see Strategy below).

***

#### Memory Injection Amount

The maximum number of 'memories' Smart Context will insert into the context.
Not every injection attempt will get this full amount.
If you send an input related to 'dogs' and only one other message in the DB is related to dogs, then only 1 item will be inserted.

***

#### Individual Memory Length

This is the maximum length allowed for each injected 'memory'.
This is in **CHARACTERS** (not tokens).
If set too small, the memory could be cut off midway.

Example:

`Ross: I like dogs with long fur and fluffy tails. I dislike dogs with short fur and short tails.`

This database 'memory' is 103 characters long, so you would need to set the slider to at least `103` in order to pull it entirely into the context.

If the slider is less than 103, the message would be cut off and injected like that.

***

### Injection Strategy

#### Replace oldest history

This strategy keeps X recent messages, removes all message before that, and replaces them with 'memories'.

Advantage

- less likely to overflow your context limit
- memories existing near the top of the context will have less immediate impact on the response while still providing 'background information'.

Disadvantage

- old messages are inserted directly into the chat history with no special demarcation, and usually have no immediate natural relevance to the preserved natural chat history messages. This can confuse less intelligent AI models.

#### Add to Bottom

This strategy leaves the chat history in its natural state and adds 'memories' **after** it inside a formatted [bracket header].
This means the 'kept messages' sliders is effectively disabled.

Advantage

- does not shorten or alter the current natural chat history
- 'memories' exist after chat and have a stronger impact on the next AI response

Disadvantage

- because no chat items are being removed/replaced, there is a higher chance you will overflow your context limit.
- because the memories exist very close to the end of the prompt they can have TOO MUCH effect on the AI's response.

#### Custom Depth

This strategy leaves the chat history in its natural state and adds 'memories' at the depth you determine within the template you specify.
This means the 'kept messages' slider is effectively disabled.
The custom injection message should include the `{{memories}}` template word which is where all queried memories will be placed.

Advantage

- flexibility to experiment with memory placement 
- customizable introductions to memory within context

Disadvantage

- because no chat items are being removed/replaced, there is a higher chance you will overflow your context limit.


#### Use % Strategy

Note: This is not compatible with the 'Add to Bottom' strategy, which does not remove any messages at all.

While using the 'Replace Oldest History' strategy, checking this box will enable the slider for selecting a percentage of the in-context chat history to replace with SmartContext memories. It will also disable the two sliders for manually selecting the number of messages.

This strategy automatically calculates a percentage of the chat history to be replaced with SmartContext memories, instead of a fixed number of messages.

Advantage

- easier than manually calculating the number of messages yourself
- adjusts with the available context size, applying the same percentage to small and large prompt spaces

Disadvantage

- calculations for how much history to remove can be slightly innacurate as they are based on estimated tokens per message
- it rounds the number of messages to remove to the nearest number divisible by 5 (0, 5, 10, 15, 20, etc), so it is not as fine grained as manual numeric selection.

***

### Memory Recall Strategy

#### Recall only from this chat

This is the default behavior of smart-context and pulls 'memories' only from the ChromaDB collection for this specific chat.

#### Recall from all character chats

This is an experimental behavior of smart-context which pulls 'memories' from all ChromaDB collections for the selected character.
Hypothetically this should allow for the development of a more robust memory set spanning many interactions. 
Reccomended that this be used with 'Add to Bottom' or 'Custom Depth' strategies and 'kept messages' set to a low number so that ChromaDB will pull from memory sooner.

### Using Smart Context

Once it is enabled and configured, Smart Context happens automatically.

ChromaDB makes a new database for each chat that is opened inside SillyTavern.
This database is automatically filled with the entire chat history.

You can also manually insert text files into the database.

These text files do not have to be chats. They can be anything (wikipedia entries, fanfic, etc).

#### Purging the Database

You can use the 'Purge DB' button to clear the database for the current chat.

This can be helpful if you find inaccurate memories have been stored (such as chat message you have since deleted or edited).

***

### FAQ

#### What happens to the databases when I'm done chatting? Can I save them?

For locally installed Extras servers, Smart Context saves the databases. There is no need to save them manually in usual use cases.

For colab users, the databases are wiped when the extras server shuts down. Use the export button to save the database as a JSON file, and import it next time you want to use it.

**Usually there is no need to save Smart Context databases.**

Currently we have an Import/Export feature, which allows you to save the chat's DB and use it again at a later date.

#### Can I make one big database for all of my chats to reference?

This would not be a good use of Smart Context's capabilities.
We recommend using World Info for this purpose.
