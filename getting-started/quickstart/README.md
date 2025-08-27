---
icon: bullseye-arrow
---

# Proposal

### PROBLEM

**Context and problem.**

* Currently, Cardano only has one mobile wallet app, Yoroi, which consistently faces issues with blockchain data synchronization.
* The habits and level of understanding of most users involve checking their assets through mobile wallets. When the Yoroi wallet encounters synchronization problems, users are unable to check their assets.
* Checking and monitoring assets through explorers is limited by the necessity to use a PC or laptop.
* There are already some apps or bots that help track wallet assets; however, to some extent, users who value their privacy may choose not to use them.
* For example, when users use a Telegram bot to query wallet asset information, the bot owner can know your Telegram ID, which addresses you hold assets in, how much assets you have, and even the IP address from which you are connecting to the internet.
* Many regular users want to create their own tracking tools to monitor their assets daily and as they wish without needing a PC or using a third-party app. However, creating, programming, or deploying a bot requires time for research and may involve costs for renting an online server…
* Cardano has many API sources that provide free blockchain data. Most of these projects creating these APIs are funded by the Catalyst fund. However, their popularity is not yet high.

### **SOLUTION**

Starting from real needs, we will create documentation to guide users in creating their own Tracker to monitor asset fluctuations in their wallets. The documentation includes:

* **Documentation and Videos:** Guides on how to create and deploy a Guides on how to create and deploy an Asset tracker (Telegram bot, google script, simple database).t.
* **Using Free APIs:** Instructions on how to utilize free API sources.
* **Bot Source Code Documentation:** Detailed documentation of the bot's source code.

**Bot Functions:**

* **Track ADA Balance Fluctuations:** Monitor ADA balances by stake address and payment address.
* **Track Cardano Token Balance Fluctuations:** Monitor Cardano token balances by stake address and payment address.
* **Manual Tracking Mode:** Check at any time.
* **Auto Tracking Mode:** Every minute.
* **Daily and Weekly Modes:** Set tracking intervals.
* **Notifications:** Include transaction ID (tx id) and on-chain messages accompanying the transaction.
* **Notification Delivery:** Sent via Telegram or email.
* **Periodic Asset Reports:** Track and send reports about wallet assets to users on a daily, weekly, and monthly basis.

_Note: Feel free to use my bot (Vietnamese version)_

**Advantages of the Solution:**

* **Convenience for Busy or Mobile Users:** Ideal for those who are busy, frequently on the move, do not regularly use a computer, or do not want to frequently install the Yoroi wallet app on their mobile devices.
* **Zero Cost:**
* **Blockchain Data Queries:** Free (using public APIs like Koios, Blockfrost, etc.).
* **Bot Deployment Server Fees:** Free (using Google’s free cloud server).
* **Database:** Free and easy to use with Google Sheets.
* **Bot Source Code:** Simple, utilizing Google AppScript code.
* **Google Account:** Free.
* **Telegram Bot:** Always free.
* **Faster Result Retrieval:** Provides quicker access to results compared to using explorer websites or wallet apps.

### IMPACT

**Impact and benefit**

* **ADA holders** now have an additional convenient solution to track assets in their personal wallets, aside from using wallet apps, blockchain explorers, or third-party software.
* **Cost-saving solution:** No deployment costs… all resources used are free and familiar to millions of people worldwide (Google).
* **Promotes and enhances awareness and usability** of the available public APIs on Cardano. The expected sources are Blockfrost and Koios.
* **Non-developer holders** who master the bot will also gain additional knowledge about the Cardano blockchain.

**Key metrics to measure the success of the project**

* Number of bot functions.
* Number of bot users.
* Number of public API sources used.

**Main goal (6 months)**

* **Goal 1: Number of bot functions. This goal is defined by the following parameter:**
* Number of bot functions: **5**
* **Goal 2: Number of interactions with the product. This goal is determined by the number of times the code is forked from GitHub.**
* Forked times: **100**
* **Goal 3: Number of direct support cases.**
* Support number: **20 users**
* **Goal 4: Number of public API sources used.**
* Number of API sources: **2** (Blockfrost + Koios)

### CAPABILITY & FEASIBILITY

**Jimmy Lee**

* Leader of Viet Cardano Community (VCC) pool
* Creator of VCC bot https://t.me/vietpool\_bot
* Alternate delegate of Cardano Constituation Convention 2024
* Admin of largest Vietnamese Cardano facebook Community (helping over 47k followers) [https://www.facebook.com/groups/cardanoviet](https://www.facebook.com/groups/cardanoviet)
* Instructor of 2 “how to evaluate a crypto’s project" courses.
* An Entrepreneur with 3 year experience in the blockchain field.
* Has a deep knowledge in blockchain technology and blockchain projects.
* Telegram: [https://t.me/Jimmy\_Lee01](https://t.me/Jimmy_Lee01)
* **Linkedin :** [linkedin.com/in/le-linh-813125117](https://www.linkedin.com/in/le-linh-813125117)

I have experience managing and completing 7 proposals since Fund7. I have also successfully created a bot with similar functions to serve my Telegram community.

![](https://cardano.ideascale.com/a/community-id/163/attachments/embedded-files/embedded-idea-custom-field-image-e231bf/png)

### PROJECT MILESTONES

#### I. **Research and Planning**

**Tasks:**

* Research product structure.
* Collect user needs for bot usage.
* Define the bot's functions.

**Outputs:**

* A document detailing the survey of user needs within the community.
* A detailed document describing the bot creation process, bot functions, and deployment resources.
* A detailed document describing the process of creating a Telegram bot.

**Acceptance Criteria:**

* One document describing the article structure, the structure of one clip, the list of articles, and the plan for writing articles, making clips, and live streaming details by week.
* Two basic introductory articles.

Tracking/Audit sources: Google shared documents, Github (https://github.com/JimmyLee16/Cardanodocs)

**Timeline: 1st month**

**Budget Est: 6600 ADA**

#### **II. Develop and Deploy the First 2 bot Functions**

**Task:**

* Develop and deploy the function to query ADA balance via payment address in manual mode.
* Develop and deploy the function to query ADA + token balance via payment address in manual mode.
* Develop and deploy the function to query ADA balance via stake address in manual mode.
* Develop and deploy the function to query ADA + token balance via stake address in manual mode.
* Implement user engagement and provide direct support.

**Outputs:**

* A document describing the bot implementation process (Google Doc + GitHub).
* Bot source code (GitHub).
* User engagement report.

**Acceptance Criteria:**

* Source code repositories: 4 repositories.
* User engagement report (Google shared document).

**Timeline**: month 2nd

**Budget Est**: **6385 ADA**

#### **III. Develop and Deploy the Remaining 2 Bot Functions**

**Task:**

* Develop and deploy the function to track ADA balance fluctuations in real-time.
* Develop and deploy the function to query token balance in real-time.
* Implement user engagement and provide direct support.

**Outputs:**

* A document describing the bot implementation process (Google Doc + GitHub).
* Bot source code (GitHub).
* User engagement report.

**Acceptance Criteria:**

* Source code repositories: 2 GitHub repositories.
* Summary report: 1 document.

**Timeline**: month 3th

**Budget Est**: **6385 ADA**

#### **IV. Close out**

**Task**

* Creating instructions video
* Summarize and Create a Final Project Report

**Output**

* Instructional video clips (YouTube)
* Final documents

**Acceptance criteria**

* Number of clips: 04 clips

**Timeline**: month 4th

**Budget Est**: **2850 ADA**

#### Project team and roles

**1.Jimmy Lee – Project leader**

* Leader of Viet Cardano Community (VCC) pool
* Alternate delegate of Cardano Constituation Convention 2024
* Admin of 2 Vietnamese Cardano facebook Community (helping over 40k followers) [https://www.facebook.com/groups/cardanoviet](https://www.facebook.com/groups/cardanoviet) and https://www.facebook.com/groups/ada.holder
* Instructor of 2 “how to evaluate a crypto’s project" courses.
* An Entrepreneur with 3 year experience in the blockchain field.
* Has a deep knowledge in blockchain technology and blockchain projects.
* Telegram: [https://t.me/Jimmy\_Lee01](https://t.me/Jimmy_Lee01)
* **Linkedin :** [linkedin.com/in/le-linh-813125117](https://www.linkedin.com/in/le-linh-813125117)

### TOTAL BUDGET: 22,366 ADA

#### Budget Breakdown

**Milestone 1: 6,600 ADA**

* Research product structure: **4,416 ADA** (80 hrs × 55.2 ADA/hr)
* Documentation writing (bot functionality, technical, deployment): **1,536 ADA** (40 hrs × 38.4 ADA/hr)
* Project management & planning: **648 ADA** (30 hrs × 21.6 ADA/hr)

***

**Milestone 2: 6,385 ADA**

* Develop & deploy bot source code (2 features): **5,413 ADA** (\~80 hrs × 68 ADA/hr)
* Project management, user support, report: **972 ADA** (45 hrs × 21.6 ADA/hr)

***

**Milestone 3: 6,385 ADA**

* Develop & deploy bot source code (2 features): **5,413 ADA** (\~80 hrs × 68 ADA/hr)
* Project management, user support, report: **972 ADA** (45 hrs × 21.6 ADA/hr)

***

**Final Milestone: 2,850 ADA**

* Instructional videos (scripting, recording, editing): **1,232 ADA** (\~308 ADA/clip)
* Building technical docs: **970 ADA** ( 20hrs x 48.5 ADA/hr)
* Project management, support, final report: **648 ADA** (30 hrs × 21.6 ADA/hr)

### VALUE FOR MONEY

* I and my team has experience in implementing and completing 6 proposals.
* We already have a community with tens of thousands of members, a website with 500 accounts, so the marketing costs are reduced to a minimum.
* With ZERO cost to build bot as free cloud sever, free bot telegram, free database (google sheet) and Public API as Blockfrost, Koios. This Project will bring huge value of money for Cardano's ecosytem
* Every labour cost are listed and calculated in budget breakdown section.
