# Objective

### What is it?

The Objective extension lets the user specify an Objective for the AI to strive towards during the chat. This objective is broken down into step-by-step tasks. The completion status of each task in the list will be checked at certain intervals.

This differs from adding static direction through prompting in that it adds sequential and paced directives for the AI to follow without user intervention. It gives a more genuine experience of the AI autonomously striving to reach a goal.

### Common Use Cases

Your imagination is the limit, you can give the AI any objective you wish and it will plan out how to achieve it. You can ask it to plan how to slay a demon, rob a temple, throw a lavish party, or even take over the world.

![Objective Settings Panel](https://files.catbox.moe/kg9xfa.png)

### Configuration

- The extension is found in the Extensions menu under Objective.

- Type an objective into the top text box, then click on `Auto-Generate Tasks`. This sends a request to the connected API and asks it to provide  a list of tasks which match the objective you have typed in.

*Note: Clicking Auto-Generate Tasks will delete all existing tasks before adding new ones.*

- Upon receiving the response from the AI, a list of tasks will be created automatically in the space below the Objective input box.

- At the bottom of the panel are two boxes: `Position in Chat` and `Task Check Frequency`
  - `Position in Chat` - This is how 'deep' in the chat section of the prompt you want the current task to be inserted. The lower the number, the more attention the AI will give to the task. Setting to 0 will make the task the primary thing in the AI's mind. Setting at high values will put the task in the background and allow the AI to focus on the conversation at hand, but setting it too high may cause the AI to never 'get around' to the task at all.
  - `Task Check Frequency` - This is how often you want the AI to check if the task has been completed. If it is set to `3`, the AI will be asked if the current task has been completed every 3rd message.

**WARNING: Task Checking happens in a separate API request. Setting Task Check Frequency to 1 will double your API calls to the LLM service. Be careful with this if you are using a paid service.**

### Usage

By default the Objective extension will keep track of all tasks and their respective completion status automatically.

The User can also manually create, update, delete, and complete tasks at any time.

#### Current Task Selection

The current task will always be the first listed incomplete task. Any manual updates to tasks will trigger a check for what the current task should be. So if you add a task above a bunch of completed tasks, it will be set as the current task. Once it's completed, previously completed tasks will be skipped and the next incomplete task will be selected as 'Current'.

#### Manually Complete Tasks

You can manually toggle the completion status of a task by clicking the checkbox next to it. This will set the next incomplete task to be selected.

![Manual Task Check](https://files.catbox.moe/h5xpnj.png)

#### Manually Add Tasks

When no tasks are present, an `Add Task` button is visible, allowing you to manually create the first task.

If other tasks are already present, click the `+` button to the right of any task to insert a new task after it.

#### Delete Tasks

Click the red `x` to delete an existing task. The next incomplete task will be selected as the current task automatically.

#### Hiding Tasks

If you want to remain unaware of what tasks the AI is attempting to complete, check the `Hide Tasks` box to hide the task list and make the AI's intentions a mystery. For 100% mysteriousness, do this before clicking `Auto-Generate Tasks`!

#### Manual Task Check

If you want to manually trigger the AI to check for task completion, click on the Extras Extension button (the `magic wand` on the right side of the chat input bar) and select `Manual Task Check`.
