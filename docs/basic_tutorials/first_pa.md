# Your first personal assistant

In this step-by-step tutorial, you will create your first personal AI assistant with Latenode. By the end of the tutorial, you will have a simple yet complete AI assistant that can get natural-language tasks from the Telegram chat and helps you to handle your e-mails, tasks and meetings. Look at the final schema of the assistant at the image below.

![assistant schema](../assets/final-schema.png){ align=right }


The tutorial assumes that you already have an account and know some Latenode basics. If not, you should go through [Getting started](./getting_started.md) first.

It also assumes that you are using an online-version of Latenode. If you are using a standalone version, some interfaces, links and details may vary.

The major steps are:

1. [Add necessary nodes][add-nodes]
1. [Configure the tools][configure-tools]
1. [Setup the environment][environment]
1. [Fine-tune the agent][finetune]

[](){ #add-nodes }
## Add necessary nodes

In this short first part, we'll just fill a new scenario with necessary nodes.

Let's create a new scenario and add the first node:

1. Log in to Latenode and go to [scenarios page](https://app.latenode.com/scenarios).
1. Press **Create a new scenario** and optionally set the scenario name at the left corner of the page. For example, let it be `My First AI Assistant`.
1. Press **Add a Node to Begin** and choose **AI Agent...** from the list of available nodes. Then choose common **AI Agent** node.

    ![AI Agent node](../assets/add-node.png){ align=right }

    The node appears on the board and the node settings window pops up. Press the node name `UNTITLED` and rename to `Stanley`. For now leave other settings by default and close the window, we will back here later.

This AI Agent node is the brain of our assistant, who will manage the other nodes. We want our assistant to be able to read mails and create mail drafts, check and create events in Google Calendar, list and create tasks in the task tracker.

Now let's add tool nodes to manage:

1. Press **Add node** in the bottom menu. Find and click **Gmail...** app. Use the search field to find it easy.
1. Choose the **Create Draft** node. 
1. Repeat the process for the **Find Email** node.
1. Add the following nodes in a similar way:

    * **List Events** and **Create Event** nodes from **Google Calendar...** app.
    * **List uncompleted tasks** and **Create Task** nodes from **Todoist...** app.
    * **Create Chat Completion** from **AI: Perplexity...** app. We will explain this point later.

1. Connect the **AI Agent** node with every tool node. You should get a layout like in the image below.

![First Layout](../assets/first-layout.png){ loading=lazy }

You can see that our tool nodes are marked with yellow triangles. It means that we should configure this nodes properly.

[](){ #configure-tools }
## Configure the tools

Let's give OAuth tokens to our nodes to connect them with real systems — Todoist, Gmail and Calendar:

1. Click a Todoist node and press **Sign in**. If you are not authorized in Todoist yet, system prompts you to sign in.
1. Sing in if needed. You'll see that the **Connection** field contains an auth token.
1. Press **Save** and close the window.
1. Click another Todoist node and make sure that it has the same auth token appointed. You can manage your authorizations and rename them in **Authorizations** section in left menu.
1. Repeat above steps for Gmail and Calendar nodes. It may require giving necessary permissions for Latenode. It is safe.

Now we need to give our nodes descriptive names, clear descriptions and specify `fromAIAgent` operator for some fields. It provides the AI Agent with reliable information about these nodes.

1. Click **Perplexity** node and change its name on the top from **Untitled** to **web-search**. This is not a trivial step, as the AI agent uses the node name to understand its purpose.
1. Tweak tool description like this: `Perplexity is an AI-powered search engine delivering accurate, concise answers and context`.
1. Click the **User prompt** field, delete prefilled text, and choose the `fromAIAgent` operator in popup menu. Tweak the prompt like this: `Your new question prompt`.
1. Press **Save**.

![Config Perplexity](../assets/config-perplexity.png){ loading=lazy }

Now let's set up our Google calendar nodes:

1. Click **List events** node and rename it as **list_calendar_events**.
1. In the **Calendar ID** field choose your Google calendar.
1. In the **Tool description** field insert the following text: `Retrieves events from Google Calendar within the specified window`.
1. Choose your time zone in the **Time Zone** field.
1. Toggle **Show advanced settings** and find **Time Max** and **Time Min** fields. This parameters define the time window we mentioned in tool description. Click **Time Max** and choose the `fromAIAgent` operator in popup menu. The operator is filled in automatically, but let's tweak the prompt like this: `Must be an RFC3339 timestamp with mandatory time zone offset, for example: 2011-06-03T10:00:00+03:00`. Hereinafter `+03:00` is your UTC time zone, replace it accordingly. Repeat this for the **Time Min** field.
1. Press **Save**.

![Config Calendar](../assets/config-calendar1.png){ loading=lazy }

Configure the rest of nodes in a similar way. Corresponding fields are listed below.

* **Create Event**:

    * **Name** — `create_calendar_event`
    * **Calendar ID** — your Google calendar
    * **Tool description** — `Creates a calendar event`
    * **Start Date** — `Format according to RFC3339: yyyy-mm-dd-Thh:mm:ss+03:00`
    * **End Date** — `Format according to RFC3339: yyyy-mm-dd-Thh:mm:ss+03:00`
    * **Summary** — `Title of event` (by default).Replace default `Summary` with `Title`
    * **Description** — `Description`

* **List Uncompleted Tasks**:

    * **Name** — `list_tasks`
    * **Tool description** — `Fetches all uncompleted tasks from Todoist`
    * **Project ID** — a project ID from the dropdown list

* **Create Task**:

    * **Name** — `create_task`
    * **Tool description** — `Creates task in task tracker`
    * **Content** — default `fromAIAgent` prompt
    * **Priority** — `fromAIAgent` operator with prompt `Task priority from 1 (normal) to 4 (urgent)`
    * **Due Datetime** — `fromAIAgent` operator with prompt `Specific date and time in RFC3339 format in UTC, for example: "2024-09-26T11:30:00+03:00". Leave blank if no date is required.`

* **Create Draft**:

    * **Name** — `create_email_draft`
    * **Tool description** — `Creates draft (creates an email but not send)` (default)
    * **Subject** — default `fromAIAgent` prompt
    * **Email Body** — tweak the default prompt like this: `Include an email body in plain text`
    * **To** — default `fromAIAgent`
    * **From name** — your name

* **Find Email**:

    * **Name** — `list_email_unreads`
    * **Tool description** — `Finds an email message`
    * **Label ID** — `UNREAD`

The tools are configured and now we should setup a communication environment to speak with our AI Agent.

[](){ #environment }
## Setup the environment



[](){ #finetune }
## Fine-tune the agent

