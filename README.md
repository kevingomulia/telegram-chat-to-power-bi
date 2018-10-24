### Telegram Chat To Power BI

On 27th August, Telegram has released the chat Export feature.
Today, we will see what we can make out of the data.

##### You'll need these installed:

• Telegram Desktop  
• Power BI Desktop  
• Text editor (I recommend Notepad++)

First and foremost, let's export our chat history.  
Open Telegram Desktop and head over to **Settings** > **Advanced**.

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/1.png)

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/2.png)

Click **Advanced** > **Export Telegram** data and a pop-up will appear.  
The pop-up will prompt you on what data you want Telegram to export for you. There are a lot of details here including account information, contacts list, etc.

Since I am only interested in the chat, I unticked **Account Information** and **Contacts list**.  
Under chat export, you can select which chats you are more interested in.  
I am going with private groups, and since I want to see how many messages everyone has sent over time, I unticked **Only my messages** option.

Scrolling down further, you can choose whether you want to include media such as **Photos, Stickers, GIFs**, etc.  
Since my group chat has over 7000 photos, I decided to only export the text part.  
Note that the **Size** limit shown there is referring to the size of the **Files to include**, should you choose to include Files. _It is not the size of the chat log output_.

Finally, and most importantly, change the download format to **Machine-readable JSON**.  
(By default it is **Human-readable HTML**, but since we are going to feed it to a machine, the latter is easier to work with.)

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/3.png)

Finally, click **Export**. Telegram will churn out a JSON file for you in the background.

##### IMPORTANT:
This method will attempt to export every single private chat/group chat details into the JSON. If you want to only churn out one chat's details, do the following:
1. Bump up your chat. Telegram will churn out the details of your last active chat/group chat first.
2. After you click Export, there will be a progress bar saying how many of your messages have been processed. After it has processed all the messages in the chat you are concerned with. Simply press stop.
3. Because it is stopped halfway, the JSON is not formatted properly. By right, every open curly brace in a JSON file has to be accompanied by a closing curly brace, just like a set of parentheses.  
**To fix this**: Open the JSON file (I highly suggest using Notepad++), and add these 5 parentheses in order: **] } ] } }**, the indentation does not matter.  
(Closing square bracket, closing curly brace, closing square bracket, and two closing curly braces)

Now the JSON properly formatted.

Next, open up **Power BI Desktop**.  
On the ribbon on top, click Get **Data > More...**  
Select **JSON** and click **Connect**.

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/4.png)

Navigate through the file explorer and find your JSON file.  
I will choose my group chat JSON file.

Power BI will then open a new window called **Power Query Editor**.  
It looks something like this:

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/5.png)

Click on the highlighted **Record**. And then click on the highlighted **List**.
You will then likely see a List containing 1 (or more) **Records**. This is the number of group chats that has been processed.  
In my screenshot there are two **Records** because I clicked stop export (on Telegram) too late and a bit of the messages from the other group chat got processed, but it is not a problem for us.

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/6.png)

Click on one of the **Records**, and you will see the group name, the group type, and the list of messages inside that group.

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/7.png)

Click on **List**, and you will see a bunch of **Records**. Each of these records is a message at the most bottom level. If your group chat has 100 messages, then you will have 100 Records.
Now, on the ribbon, click **To Table**. A pop-up will appear. Leave everything as default and click OK.

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/8.png)

You will now see the messages sorted out as rows in a table, but this does not really mean much, does it?  
Click on the small button on the top right of the columns to expand the **Record**.
Since I am only interested in someone's texting activity in the group, I will only select **type, date, from, and text**.  
Also, you can untick the Use original column name as prefix. It is my personal preference.

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/9.png)

Click OK, and the table will be expanded.

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/10.png)

Now, we do some processing. Right click on the **Date** column header and **Change Type** to **Date**

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/11.png)

After changing the type to **Date**, you will notice that the format will change slightly.
Do the same thing for the other 3 columns: **type, text, and from** and set their **Type** to **Text**.

Afterwards, click **Close & Apply** on the top left hand corner.
We are then back to the main page of Power BI. On the right most corner, you will see the newly created table from the Power Query.
Expand the table and select the fields date and from.

The **from** will initially be added to the **Legend** tab. Move it to the **Value** tab to see the data.

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/12.png)

Now we are seeing something!  
To fine-grain our data even more, for example, to see the number of senders by **Month**,
navigate to the **Axis** part and click X on everything besides the **Month**.

We will then get the total number of messages being sent across, separated by month.

In order to separate the number of messages sent by each individual,
drag another **from** field from the **group** table and put it under the **Legend** part.

This is the result:

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/13.png)

The names have been censored for privacy issues, but you get the gist of it.

But these are grouped by Month, which means that the month of May consists of data from **May 2017 and May 2018**.
In order to show the data sequentially, we have to trace back our steps a bit.

Click on **Show All Levels** as shown below: (Please ignore the colour change, I am playing with the colour theme)

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/14.png)

You will then see the initial two blocks, separated by year:
Note the small **drill down** button highlighted. If you hover over it, it will show 'Expand all down one level in the hierarchy'.
Clicking on that will do exactly what it says: expanding down one level.
The level goes like: **Year > Quarter > Month > Day**

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/15.png)

So, by clicking it twice, we will then get our monthly sequential graph:

![TODO-Insert-Image](https://raw.githubusercontent.com/kevingomulia/telegram-chat-to-power-bi/master/img/16.png)

Voila! We're done! If you want another format of the chart,
simply choose any one from the **Visualizations** tab on the right hand side of the window.
