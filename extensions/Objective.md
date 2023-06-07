# Objective

### What is it?

Objective is a basic implementation of an Autonomous AI Agent for SillyTavern, which is a fancy way of saying an extension that provides an AI with a given objective, having it create a task list to achieve the objective, and then letting it run through each task executing and then marking it completed when done.

This differs from adding static direction through prompting in that it adds sequential and paced directives for the AI to follow without user intervention. It gives a more genuine autonomous purpose for the AI to follow.

### Common Use Cases

Your imagination is the limit, you can give the AI any objective you wish and it will plan out how to achieve it. Have it plan how to slay a demon, rob a temple, throw a lavish party, or emotionally heal all your psychological problems. Ok, maybe not that last one.

### Usage

The extension is found in the Extensions menu under Objective. For typical usage, you will enter in an objective into the top text box, then click on Auto-Generate Tasks. This sends a request to the AI model you have configured and tasks will be populated once the AI responds. That's it, you can start roleplaying with the AI from there.

Note: Clicking Auto-Generate Tasks will delete all existing tasks if any exists.

### Tasks

Tasks are typically managed automatically by the AI. Given an objective, the AI will create the tasks, roleplay through completing the current task, check the chat history periodically for completion, mark the task as complete, and move onto the next task.

Tasks can also be fully managed by the user as well, allowing for manual create, update, delete, and completion of tasks.

#### Current Task Selection

The current task will always be the highest task that is not completed. Any manual updates to tasks will trigger a check for what the current task should be. So if you add a task above a bunch of completed tasks, it will be set as the current task. Once it's completed, the next completed tasks will be skipped and the next incomplete task will be selected.

#### Manually Complete Tasks

You can manually toggle the completion status of a task by clicking the checkbox next to it. This will set the next incomplete task to be selected.

#### Manually Add Tasks

When no tasks are present, an Add Task button is visible, allowing you to manually create the first task. Once one or more tasks are present, to add a task, click the '+' button to the right of any task to insert a new task after it.

#### Delete Tasks

Click the red 'x' to delete an existing task. The next incomplete task will be selected as the current task automatically.

#### Hiding Tasks

Some people like a good mystery to make the roleplay more fun. Enable 'Hide Tasks' to shroud the AI's intentions in mystery. The option simply hides the tasks from the user, nothing else. 

### Configuration

#### Position in Chat

This sets the location of the current task in the prompt. A value of 0 will place the current task at the bottom of the prompt, causing the AI to really focus on the task. A higher value will allow the AI to focus more on the context of the conversation, since it is higher in the prompt, giving a looser focus on the Task. Set this too high and the AI may never get around to the task. 

#### Task Check Frequency

The AI will check the conversation periodically to see if the task was completed. The frequency of this check is configurable. Setting to 0 will disable automatic checks. Setting to 1 will check for completion every time a message is received. Setting to 5 will check every 5 messages received. 

WARNING: setting Task Check Frequency to 1 will double your API calls to the LLM model. If you're using a paid model, then this will cost more. 

Increasing the setting will slow down the pace for completing tasks, allowing more room for roleplaying the task. It may also make it harder for the AI to determine the task as completed if too high. 

### Manual Task Check

If you want to manually trigger the AI to check for task completion, click on the Extras Extension button (the magic wand next to where you type messages) and select Manual Task Check