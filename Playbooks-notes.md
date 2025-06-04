**Playbooks:**

While LLMs are powerful, they are general-purpose. To make a generative agent perform specific tasks reliably and consistently within a business context, the LLM needs structured guidance and operational instructions. This is precisely where the playbook comes in.

           Think of a playbook as a structured instruction manual or a blueprint that guides the LLM on how to achieve a particular goal or handle a specific conversational scenario. It provides the necessary context, constraints, and operational details that transform a general-purpose LLM into a specialized, task-oriented conversational agent.

            In essence, while the LLM provides the brain (the ability to understand and generate), the playbook provides the **operational intelligence, structure, and specific knowledge** needed for that brain to function effectively and reliably as a dedicated conversational agent in Dialogflow CX. It bridges the gap between the LLM's broad capabilities and the precise requirements of a conversational application. Without playbooks, a generative agent would lack the necessary guidance to consistently perform specific tasks, adhere to business logic, or interact effectively with external systems.

## Playbook data

A playbook is composed of the following data:

* **Playbook name**: a concise name in natural language that helps developers and the LLM to understand what tasks the playbook handles  
* [Goals](https://cloud.google.com/dialogflow/cx/docs/concept/playbook/goal): high level description of what the playbook should accomplish  
* [Instructions](https://cloud.google.com/dialogflow/cx/docs/concept/playbook/instruction): defines the process steps that should be taken to accomplish the goal  
* [Examples](https://cloud.google.com/dialogflow/cx/docs/concept/playbook/example): sample conversations that are effectively few-shot prompt examples for the LLM  
* [Parameters](https://cloud.google.com/dialogflow/cx/docs/concept/playbook/parameter): are used to store information about a conversation like user input, user system information, results of actions, and so on.

Playbook Types:  
Two types \- Task playbook and Routine playbook

### Task playbooks

     *Task playbooks* are the original type of playbook. They are used to break down complex tasks into smaller, reusable sub-tasks. They are used for modeling *compositional* conversation stages, where each stage communicates through input and output parameters.  
   
    Any routine or task playbook can call another task playbook, but a task playbook cannot call another routine playbook.

### Routine playbooks

*Routine playbooks* are a new type of playbook. They are used for modeling *sequential* conversation stages, where each stage is complete and independent. They can call task playbooks to decompose larger tasks into smaller sub-tasks, and they can transition to other routine playbooks or flows.

Import and export playbooks

Once you have created playbooks using the Conversational Agents console, you can export these playbooks for use in another agent. To export a playbook:

1. Navigate to the playbooks list.  
2. Click the export button for the playbook you want to export.  
3. Select exporting options.  
4. Click **Export**.

To import a playbook you have previously exported:

1. Navigate to the playbooks list.  
2. Click **Import**.  
3. Select import options.  
4. Click **Import**.

Playbook tools

Using tools, you can connect playbooks to external systems. These systems can augment the knowledge of playbooks and empower them to execute complex tasks efficiently.  
OpenAPI tools

An agent can connect to an external API using an OpenAPI tool by providing the [OpenAPI](https://www.openapis.org/) schema. By default, the agent will call the API on your behalf.

We need a webhook for accessing the data.

Example:

In the school have library we need to access AI book

**User:** "Can you help me find a book on the ethics of artificial intelligence?"

**LibAssist Pro (The Playbook in action):**

* *(Internally: The `Resource_Discovery` Playbook is **triggered by the user's intent**. It identifies `$search_query` as "ethics of artificial intelligence" and `$resource_type` as "book".)*  
* *(Internally: The Playbook executes Step 2, calling the `Catalog_Search_Tool`.)*  
* *(Output from `Catalog_Search_Tool` might be: `{"title": "AI Ethics: A Global Perspective", "author": "Dr. Alistair Finch", "callNumber": "Q335 .F56 2023", "status": "Available"}`)*  
* "Searching for books on 'ethics of artificial intelligence'. I found **'AI Ethics: A Global Perspective' by Dr. Alistair Finch**. It's currently available. Would you like the call number or a link to the e-book if available?"

### 

### OpenAPI tool limitations

The following limitations apply:

* Supported parameter types are `path`, `query`, `header`. The `cookie` parameter type is not supported yet.  
* Parameters defined by OpenAPI schema support the following data types: `string`, `number`, `integer`, `boolean`, `array`. The `object` type is not supported yet.  
* You currently can't specify query parameters in the console example editor.  
* The request and response body must be empty or JSON.

# Data store tools 

Data store tools can provide AI-generated agent responses based on website content and uploaded data. Your agent can find answers to end-users' questions from your [data stores](https://cloud.google.com/dialogflow/vertex/docs/concept/data-store) during [fulfillment](https://cloud.google.com/dialogflow/cx/docs/concept/fulfillment).

## Data store tools: API

You can set up one data store of each type per tool, and the tool will query each of these data stores for answers. By default, the agent will call the [`dataStoreTool`](https://cloud.google.com/dialogflow/cx/docs/reference/rpc/google.cloud.dialogflow.cx.v3beta1#datastoretool) on your behalf.

There are three data store types:

* `PUBLIC_WEB`: A data store that contains public web content.  
* `UNSTRUCTURED`: A data store that contains unstructured private data.  
* `STRUCTURED`: A data store that contains structured data (for example FAQ).

**Example Interaction:**

* **User:** "I lost my library card and need a new one. How do I get it replaced?"

* **LibAssist Pro (The Playbook in action):**

  * *(Internally: The `Librarian_Support` Playbook is triggered. It asks for the user's name, student ID, and preferred contact method.)*  
  * "Okay, I can help you with that. Can you please confirm your full name and student ID number so I can create a replacement request for you?"  
  * **User:** "My name is Alex Smith, ID is 1234567\. Please email me when it's ready."  
  * *(Internally: The Playbook extracts details. It then executes Step 2, calling the `Librarian_Support_Jira` tool with `projectKey: "LIBHD"`, `summary: "Library Card Replacement"`, `description: "User Alex Smith (ID 1234567) needs card replacement. Contact via email."`)*  
  * "Thank you, Alex. I've successfully created a support ticket for your library card replacement. Your ticket ID is **LIBHD-789**. A librarian will follow up with you via email regarding the pickup details

**Connectors**

Directly connect to the data source

**The connector tool supports the following [Connector types]:**
BigQuery  - serverless data warehouse service by Google Cloud
CloudSQL - SQL Server - relational database service by Google Cloud
Jira Service Management- IT Service Management (ITSM) and customer service management (CSM) platform by Atlassian
Oracle DB- relational database management system (RDBMS) from Oracle Corporation
PayPal- online payment processing platform and digital wallet service.
Salesforce- cloud-based Customer Relationship Management (CRM) platform.

**Functions:**   for real-time sensor streams

If you have functionality accessible by your client code, but not accessible by OpenAPI tools, you can use function tools. Function tools are always executed on the client side, not by the agent.

**Advanced Capabilities (Tools & Playbooks): Real-time Study Room Availability**

"For real-time study room availability based on sensor data in the library's app, I would primarily use a **Function Tool** within a Playbook.

**Reasoning:**

* A **Function Tool** is designed for scenarios where the chatbot needs to trigger custom code directly within the client application (like a mobile app or web interface). Real-time sensor data is often directly accessible by the client app or via a local network connection that the client app can reach more easily than a cloud-based webhook. This avoids exposing internal sensor APIs externally.  
* An **OpenAPI Tool** would be suitable if the sensor data was exposed via a standard REST API that the Dialogflow CX cloud service could directly call.  
* A **Data Store Tool** is for querying large, often pre-indexed or semi-static datasets, not typically for real-time sensor streams.

**How it works with the Playbook:**

1. The `Facilities_Inquiry` Playbook (or a similar one) would be triggered when the user asks about study room availability.  
2. Within the Playbook's instructions, it would identify the floor or zone the user is interested in (e.g., `4th floor`, `quiet zone`) as parameters.  
3. The Playbook would then call the **Function Tool** (e.g., `Get_Study_Room_Status`), passing these parameters.  
4. The client application (e.g., the LibAssist Pro mobile app) would receive this Function Tool call, execute its own code to access the local sensor data, and retrieve the real-time availability.  
5. The client application would then send the result (e.g., '5 available seats in the quiet zone') back to Dialogflow CX.  
6. The Playbook would receive this data and use its instructions to formulate a natural language response to the user."

**Intent:** understanding the user goal

An **Intent** is the chatbot's understanding of the user's ultimate goal or purpose behind their words.  
An **Intent** is what the chatbot figures out you want to achieve.

**Example:** If you tell LibAssist Pro: "I want to check my book return date."

* The **Intent** is `Check_Book_Status`, because that's your goal.

**Entity**: specific information

An **Entity** is a specific piece of information the chatbot extracts from what you say.

**Example:** If you tell LibAssist Pro: "Find me a book by **Stephen King**."

* "**Stephen King**" is the **Entity** (specifically, an "author" entity).

Entity Types:

* [Map entity]
* [List entity]
* [Composite entity] (a special kind of list entity)  
* [Regexp entity]

**Flows**: it is nothing but focused on a specific path.

A **Flow** is like a special pathway or a "mini-game" inside the chatbot. It helps the chatbot stay focused on one main topic at a time.

**Example:**

Imagine your LibAssist Pro chatbot is like a big library.

* **Flow:** `My_Library_Account` (instead of `Account_Management_Flow`)  
* **What it does:** This "game" is *only* about helping you with your library account stuff – like checking if your books are due, or making sure you can renew them. While you're in this "game," the chatbot won't suddenly talk about how to find a book or library hours. It stays focused\!  
* **How it works:** When you tell the chatbot something like "What books do I have checked out?", the chatbot knows you want to play the `My_Library_Account`. It takes you onto that special pathway and helps you find out about your books until you're all finished with your account questions.

**Webhook:** used to fetch the data

The webhook acts as a bridge, allowing your intelligent LibAssist Pro chatbot to fetch real-time, personalized data from your private library systems, making it much more powerful and useful\!

**Example for LibAssist Pro:**

Imagine your LibAssist Pro chatbot needs to tell a user their exact library fines. This information is stored securely in your library's internal financial system, which Dialogflow CX doesn't have direct access to.

1. **User asks LibAssist Pro:** "What are my current library fines?"

   * *(Intent: `Check_Library_Fines` is detected.)*  
2. **Dialogflow CX Calls a Webhook (The "Heads-Up"):**

   * Instead of replying directly, Dialogflow CX triggers a **Webhook**. It sends an automated message (an HTTP POST request) to a specific web address that you've set up. This address points to a small piece of custom code you control (often called a "webhook endpoint" or "fulfillment service," like a Google Cloud Function).  
   * This message includes information like the user's ID (if authenticated) and what they asked for.  
3. **Your Custom Code Does the Work:**

   * Your custom code receives this webhook message.  
   * It takes the user's ID.  
   * It then securely queries your library's internal financial system for that user's fines.  
   * Once it gets the fine amount, it formats a response.  
4. **Your Code Sends a Response Back to Dialogflow CX:**

   * Your custom code sends an HTTP response back to Dialogflow CX, containing the answer (e.g., "Your current fines are $5.25 for an overdue book.").  
5. **Dialogflow CX Replies to the User:**

   * LibAssist Pro then takes that response from your custom code and tells the user: "Your current library fines total $5.25 for an overdue book."

**Page:** one specific step or page

A **Page** is like one specific step or a single "screen" in the chatbot's conversation game, where it asks you something or tells you something important.

**Example:**

Remember our `My_Library_Account` (that's the **Flow**)?

* **Page:** `Check_My_Due_Dates_Page`  
* **What it does:** When you enter the `My_Library_Account` because you want to know about your books, the chatbot might take you to the `Check_My_Due_Dates_Page`. On this page, the chatbot's job is *only* to ask for which book you want to check, or your student ID, so it can tell you the due dates. It's one focused step in the bigger "game"\!

**Parameters:**

A **Parameter** is a specific piece of information the chatbot pulls out of what you say, like "**pizza**" when you say, "I want to order a **pizza**."

**Generators**:

A **Generator** lets your chatbot use Google's smartest AI (Large Language Models) directly inside Dialogflow CX to create responses or perform clever tasks. It's super useful because it can instantly summarize text, answer questions, or even change data without needing extra programming or a webhook.

**Example:**

Imagine LibAssist Pro gets a very long email about new library rules. You can use a Generator to summarize it.

* You'd set up a Generator with a prompt like: "Please summarize this text in two sentences: $text".  
* Then, you'd feed the long email into the `$text` part, and the Generator would spit out a short, easy-to-read summary for the user\!

### **What is Fallback in Dialogflow CX?**

**Fallback** is a safety net.  
 When the chatbot doesn't understand the user's input (i.e., no intent matches), it **triggers a fallback route**.

LLM might try to provide a more creative or apologetic response.

It helps:

* Avoid bot silence or errors.

* Ask the user to rephrase.

* Route to a live agent or Playbook.

### **Where is Fallback Configured?**

Fallbacks can be added in 3 places:

1. **Route group fallback** (for intents that are not matched)

2. **Page-level fallback** (when no conditions are met on a page)

3. **Flow-level fallback** (used if no page fallback is available)

### 

### **Simple Real-World Example:**

####    **Use Case:**

You have a **"Course Help Bot"** for a university. User types:  
 **"I want pizza"**

But your bot only understands:

* "What is my course progress?"

* "List available courses"

* "Quiz score for Python"

Since "I want pizza" doesn’t match anything...

####    **Fallback Route is triggered:**

* The bot responds:  
      “I’m not sure how to help with that. You can ask about your course progress or quiz scores.”

**Playbook fallback mechanism:**

        In Dialogflow CX, Playbooks are designed to handle complex, multi-turn goals using a defined set of instructions and tools. When a Playbook is involved, fallbacks typically occur in two main phases: before the Playbook even starts, or during its execution if something goes wrong with its instructions or tools.

         "Playbooks build upon Dialogflow CX's core fallback mechanisms. While the initial understanding (like `no-match`) is handled by standard Event Handlers, a well-designed Playbook also incorporates internal fallback logic within its instructions to handle specific scenarios like 'no results from a tool' or 'tool errors'. This ensures that even within complex multi-turn processes, the chatbot remains resilient and provides a helpful, guided experience to the user."

Difference between intent and entity

**Example for LibAssist Pro:**

* If a user says, 'Find me a book by **Stephen King**,'  
  * The **Intent** is `Find_Book` (the user's goal).  
  * '**Stephen King**' is an **Entity** (specifically, an `@sys.person` or custom 'author' entity type), as it's the specific data needed for the search."

Difference between flow and page

**Example for LibAssist Pro:**

* Imagine a user wants to manage their library account. They'd enter the `Account_Management_Flow`.  
* Within this Flow, if they ask to check due dates, they might be directed to the `Check_Due_Dates_Page`. On this Page, the chatbot's focus is to ask for their library ID and then provide the due dates. If they then say, 'Can I renew this book?', they might transition to the `Renew_Book_Page` within the same `Account_Management_Flow`, which would guide them through the renewal process."

**Webhooks vs. Generators:**

 Instead of calling *your* custom code like a webhook, a Generator allows the chatbot to send a 'prompt' directly to an LLM to generate text, summarize information, extract data, or even format content.

**The key difference for tasks like summarization:**

* With a **Webhook**, you would write custom code that receives text, then perhaps **calls a *different* external AI service** (or your own code) to perform the summarization, and then sends the summary back to Dialogflow. This requires more development overhead for the external service.  
* With a **Generator**, you **simply define a prompt** (e.g., 'Summarize this text: $text') directly within Dialogflow CX. The LLM performs the summarization natively, making it a much simpler and faster way to integrate generative AI capabilities without needing to build and manage external fulfillment code for that specific task."

**Fallback Mechanism**

"A **fallback mechanism** is absolutely crucial in LibAssist Pro to ensure a smooth user experience, even when the chatbot can't fulfill a request perfectly. It prevents users from hitting a 'dead end' or becoming frustrated.

**Scenario Example:** Imagine a user asks: 'Who won the last World Series?'

**Implementation of Fallback:**

1. **Initial No-Match Fallback (Most Common):**

   * **Problem:** This query is entirely outside LibAssist Pro's intended domain (library services). No specific Intent like `Check_Sports_Scores` is defined or would be relevant.  
   * **Solution:** The system's **`sys.no-match-default` Event Handler** (often set at the Flow level) would be triggered.  
   * **LibAssist Pro's Response:** 'I apologize, I'm a library assistant and can only help with library-related questions about books, services, or account information. What can I assist you with today?'  
   * **(Advanced using Generative Fallback):** If enabled, the system might try to provide a more natural, yet still redirecting response, like: 'That's an interesting question\! While I can't provide sports scores, I can certainly help you find books on sports history in our collection.'  
2. **Fallback within a Playbook (e.g., Tool Failure/No Results):**

   * **Scenario:** A user asks: 'Find me the book with the ISBN 12345.' LibAssist Pro correctly triggers the `Resource_Discovery` Playbook to use the `Catalog_Search_Tool`.  
   * **Problem:** The `Catalog_Search_Tool` tries to search the library's catalog, but either the ISBN is invalid, or the catalog API is temporarily down, or it simply finds no book with that ISBN.  
   * **Solution:**  
     * **A. Playbook Instructions:** The Playbook's instructions would have explicit logic: 'If **${TOOL: Catalog\_Search\_Tool}** returns no results, inform the user: 'I couldn't find a book with that ISBN in our catalog. Please double-check the number or try searching by title/author.''  
     * **B. Webhook Error Handler (for system-level issues):** If the `Catalog_Search_Tool` relies on a webhook and that webhook *fails* due to a network error or the catalog server being completely unresponsive, a **`webhook.error` Event Handler** (configured at the Page or Flow level) would catch this. LibAssist Pro would then deliver a more generic error message: 'I apologize, I'm experiencing technical difficulties connecting to our catalog right now. Please try again in a few minutes.'

