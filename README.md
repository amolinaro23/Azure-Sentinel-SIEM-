# Azure-Sentinel-SIEM-
In this lab, I setup Azure Sentinel and connected it to a live virtual machine acting as a honey pot. This allowed me to observe live attacks, specifically RDP brute force attacks, from all around the world. I then used  PowerShell script (from Josh Madakor) to look up the attackers geolocation information and plotted it on the Azure Sentinel map. 


<h2>Step 1 - Step Up an Azure Sentinel Account (Free)</h2>

I had already created an account for a previous lab, so I do not have screenshots of this particular step... but, if you visit https://azure.microsoft.com/en-us/free/ you can sign up for a free account that grants you $200. After all that is done, you'll click on the Virtual Machines icon. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/e787e39d-2c06-4124-a934-73df2a211756)

<h2>Step 2 - Setting up the Virtual Machine</h2>

After you click on the VM icon, we will need to create our virtual machine. Next to **Resource group** click **Create new** and name the resource group whatever you'd like. I followed Josh Madakor's video initially, so I went with what we originally did, "Honeypot_lab". Next you'll name your virtual machine under **Instance details**. For Region, I just kept mine '(US) East US 2'. I kept **Availability Zone** on the default selection. Next, you will choose what OS the VM will run, I choose **Windows 10 Pro** for the image (vm OS). Lastly, create the admin sign in account with the details of your choosing. Leave the **Public inbound ports** and **Select inbound ports** on their default selections. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/2da3067e-59d3-49f0-9c2d-4ec7bfcc88d9)

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/998306cc-6fe5-41f9-9dfc-53812016f0f6)

Click **Next** at the bottom of the screen, **NOT** Review and create. Once you get to the **Networking** tab, we're going to select **Advanced** under the **NIC network security group**. Think of this as the firewall for the VM. Once you click **Advanced** then a dropdown menu will appear and you will click **Create** to make a new one. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/fb2dc23b-800d-4741-84a6-934afcb94016)

On the **Create network security group** page, we are going to get rid of the default rules for the NIC. Click the trashcan icon on the right.

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/98c0c333-d3b4-4171-99f6-d857e4d41c2f)

Next, we will create our own inbound rule. On the **Add inbound security rule** make the following selections: 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/b7ca1e20-03a4-457f-bf8b-f8649cc84b8e)

After you've completed this, go ahead and click **Review + create** button down at the bottom. Your VM should begin deploying. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/8543d67f-9a23-47bf-9f92-09a28f0cde01)

<h2>Step 3 - Create Log Analytics Workspace</h2>

As your VM is deploying, go to the search bar and search for **Log Analytics Workspaces**. Creating this will allow us to input logs from the VM. Go ahead a click the blue button **Create log analytics workspace**. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/ba0c6504-3fc7-479a-8684-3f5418fbc2f6)

When you get to the **Basics** of creating our workspace, the drop down next to **Resource Group** should have the same resource group you created earlier when configuring the VM. For **Instance details** you can put whatever name you like. I named mine, "LAW-Honeypot", for Log Analytics Workspace for simplicity. Then click the blue button at the bottom that says, **Create**. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/ac70ea5c-428f-4991-9e88-512a7223c33f)


<h2>Step 4 - Enabling Log Gathering in Microsoft Defender for Cloud</h2>

The next step will be to enable the ability to gather the logs. In the search bar, search for **Microsoft Defender for Cloud**. In the navigation pane, find **Management** and select **Environment Settings**. You should be able to see our workspace labeled with your honeypot name. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/68d928c7-24e2-4b45-9af2-6f60e1ebf04f)

At the **Settings** page, you will want to turn OFF the **SQL servers on machines** and turn ON **Servers**. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/9d6366a4-3411-4ef0-a6df-24a6fa28db92)

On the navigation page, click on **Data collection** and select **All Events**

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/8aaa42d5-d869-4bed-bee5-1890c1e004aa)

<h2>Step 5 - Connect Log Analytics to VM</h2>

Back in the search bar, go back to **Log Analytics Workspaces** and select your titled workspace. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/846573e2-439b-4c68-9916-daad6f13c702)

On the navigation pane to the left, scroll until you see **Virtual Machines (deprecated)** and select it. You should see our VM we created. Click the VM name and on the next window click **Connect**. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/04973ee4-f800-4287-b1ce-82f9a2821c01)

<h2>Step 6 - Set Up Sentinel</h2>

As the Log Analytics are loading, open up a new tab to **portal.azure.com**. Search for **Sentinel**. On the Microsoft Sentinel page, click the blue **Create Microsoft Sentinel** button. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/328d4c35-1f03-4b3c-9c6f-2c00b4922614)

Then, you should see our Honeypot workspace appear. Select your workspace and then click **Add** at the bottom of the screen. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/3b492262-7f1b-4459-8093-9e71022bdd10)

<h2>Step 7 - Log in to VM</h2>

In the search bar, search **Virtual Machines**. By this time, everything should be connected and ready to go and you will see your VM. Select the honeypot VM and copy your Public IP address for the VM. 

**Side note and troubleshooting** - If your VM doesn't have an IP address (mine didn't initially), check to see if you have other VMs on your account (if this isn't your first time creating one). I ran into an issue because I had done a previous lab with two other VMs in Azure and the free trial doesn't allow you to have that many public IP addresses... ANYWAY, click on the VM and in the navigation pane, select **Network Settings**. In the new window pane you should see next to **Public IP** there is a **(configure)** hyperlink, click it. Now select the **ipconfig1** at the bottom and check the box next to **Associate public IP address**. In the drop down menu, select your VM and click **Create a public IP address**. You should be able to just click **OK**, then **Save**, then **Create**. Azure should begin deploying the IPv4 configuration to the VM. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/77edd716-1cc8-4d77-b75f-cead0ec416c6)

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/74d5cdf8-349c-42cb-82ec-72dc2eb4b8a5)

BACK TO STEP 7 - In Windows (your PC), search for **Remote Desktop Connection**. Enter your VM's Public IP address and then at the bottom of the authentication select **More choices** and select **Use a different account**. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/81078109-311c-4378-8d67-88913dc1363d)

Accept the certificate warning. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/822f51b6-c0b6-4afc-9484-77f01c72e209)

As the VM loads, and the privacy settings page pops up. Select **NO** for all the options then hit select. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/99f5dcfb-7ece-44f5-905c-85048d60f383)

Go ahead and set up Microsoft Edge. Then go to start and search **Event Viewer**. 

<h2>Step 8 - Observe Event Viewer Logs in VM</h2>

In the VM, search for Event Viewer from the start menu. Then in the left navigation pane, expand the **Windows Logs** and then select **Security**. What you see are all the security events that have transpired so far. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/31a52718-41ac-470b-8a52-a6b496603a97)

Now, in Josh's example of this lab, he mistaken failed a login attempt and has a log for event # 4625. I did not fail a login attempt, therefore I did not have this event in my logs. Instead, for the sake of explaination, I used my successful login attempt instead. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/7ea9ed2a-6e62-4ec4-bdb5-1dc9612d4d96)

If you also did not fail to login to the VM initially, go to YOUR computer and back to Remote Desktop and attempt to log in to the VM again with the wrong credentials. Go back to the VM and refresh the log. You should now see the **Audit Failure** with the event ID of 4625. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/f6442591-4d60-4569-b5e3-2abc915efef4)

If you scroll you should see the **Source Network Address** copy it. 

<h2>Step 9 - Disable the VM Firewall</h2>

We are going to disable the firewall on our VM to allow hackers to discover the machine online. Go back to your computer and open a command line. Use the **ping** command and ping your VM. The request should time out. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/f8d3f9dd-e7c3-4d56-9de1-878442c36cd7)

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/a83c5242-570a-425c-b8aa-6f73460c3c66)

So, go back to the VM and go to **Start** and search **wf.msc**. This will open a new window displaying the firewall configuration. At the bottom of the top pane, click **Windows Defender Firewall Properties** and turn ALL (Domain, Private, Public) the firewall **off**. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/522c535a-fc78-4361-b185-f32171cd1e8d)

Back on your PC's command line, you should start seeing ping requests. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/09d64529-5944-4f6c-806f-63d196166097)

<h2>Step 10 - Download PowerShell Script</h2>

On the VM, go to Microsoft Edge and follow the link to download the PowerShell script from Github. 

https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1

You will click on the **Custom Security Log Export** file. Josh explained in his video you can either download the script or just copy it. I went ahead and just copied it. Next, open **PowerShell ISE** on the VM. Once it opens, we'll click **new** and paste the script into PowerShell ISE. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/296d377a-d277-49b3-adf0-432df2a6f3f9)

Save the PowerShell script to the Desktop on the VM. Now, we will need our own API number. Go the Microsoft Edge on the VM and go to ipgeolocation.io and signup. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/7974053b-4acc-4fcc-b028-4282534551a6)

After you sign up and sign back in, you'll see your API key. Paste that into the PowerShell script at the top. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/6fbf4c3f-b02c-4d47-84f4-34444436ce10)

After you've completed that step, you can run the script. The script is capturing all the data from the Event Log that we opened earlier. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/34786b7a-6525-4a0c-9e9f-c35b9d34b149)

<h2>Step 11 - Access Log Data from Script</h2>

The log data from this script is programmed to save to a hidden path within the C-Drive. We have to manually access this drive since it is hidden. Go to start and search **Run** then open the file by typing **C:\ProgramData\** 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/72e9bfee-b9d6-4d49-9abe-86a0278e7b98)

The folder will open and at the bottom  you'll see the newly created log files. Open the **failed_rdp** file. That contains all the failed login attempts using that Event#4625 we talked about earlier. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/ad4011ac-856d-45fe-a112-fd4df065d012)

<h2>Step 12 - Create Custom Log in Log Analytics Workspace</h2>

Minimize your VM and go back to Azure. There have been updates to Azure since Josh originally completed this lab, so here are the steps I found to get the rest of this to work. First, search **Log Analytics workspaces** in Azure and select your honeypot workspace. In the left navigation pane, click **Tables** then in the main window click **Create**. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/bd06d85c-9c5f-48fd-93c4-d1fdd1e24f86)

We will need to move the **failed_rdp** log file from our VM to our actual PC. Go to your VM and if you still have the file manager running, you can click on the **failed_rdp** file and select and copy the whole record. Open **notepad** on your pc and paste the log file. Make sure to save the log file somewhere easy (desktop). 

Now, back on your PC in Azure, when you clicked create, select **New custom log (MMA-based)**. Input your saved failed_rdp file into the dropdown in the next window. Then at the bottom click, **Next**. On the **Collections Path** tab, under **Type** select **Windows** and in the path type **C:\ProgramData\failed_rdp.log** as that is the path to the file in our VM. Click **Next** and then in the details tab name your log, I choose what Josh did, "Failed_RDP_With_Geo", then click **Next** and then **Create** on the next page. 

On your PC, if you go to the left Navigation pane and select **Logs**, in the New Query 1, type **SecurityEvent** and you will be able to see the Windows logs from the VM. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/c3a4e3a1-dbf5-4e7d-b6cb-bee12efa5cb2)

Back in logs, type **SecurityEvent | where EventIT == 4625** now you can see all the failed login attempts on our VM. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/44c8d06a-422e-4276-9dab-89fbe4e1f6ac)

Our custom log will take some time to update. Take a break and check back periodically by typing **Failed_RDP_With_Geo** or whatever you named your custom log. 

<h2>Step 13 - Extrat Raw Data from Logs </h2>

We are going to parse and organize the information for when we extract the data. To do this, copy the following script and run it. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/9ba3e184-5953-4c19-b3c3-6f3208b2a9d1) 

You should now see the information from the raw data has been parsed and organized by country, state, sourcehost, username, destination, longitude, and latitude. 

<h2>Step 13 - Setup Map in Sentinel with Latitude and Longitude</h2>

Leave your parsed log data open and open a new tab and go to **portal.azure.com** and then search for **Sentinel**. In Sentinel, select your Honeypot workspace. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/89643e2b-c1f8-415b-87c9-56bd3af4cdbe)

Now, I had to take care of my daughter when I originally started this lab... so this information from Sentinel is over the course of a couple days... but, you can see the specific **Failed_RDP_Honeypot** instances highlighted by Sentinel. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/4b5a8825-9e0b-4b47-a5ef-d42cd62178e7)

<h2>Step 14 - Setting Up Our Map</h2>

In Sentinel, in the left navigation pane, select **Workbooks**. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/64b15afa-4a30-482a-ab60-f98320ce57b6)

At the top of the Workbooks page, select **Add Workbook** 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/2953717f-374b-41b4-996a-bb4418b3af3f)

Then click, **Edit** 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/30117c91-0763-4258-b13d-c8f21ed8c1ed)

At the far right of the Workbook, you should see the word **Edit** with the three dots. Click the dots and select to **Remove** the two predisposed widgets. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/9464ec25-7a3f-45f5-93da-73e39db64f95)

Then, when the Owl and the **This Azure Sentinels Report has no content** appears, below the Owl click **Add** 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/aaecf54d-a9db-4dbe-a15a-8d7cfc5adf42)

Now, we will run our query with our parsed categories. In the script of your workbook, copy the same script we used to categorized our rawdata. Then, at the top of the query panel there is a dropdown box under **Visualiztion**, select **Map** from that dropdown. 

![image](https://github.com/amolinaro23/Azure-Sentinel-SIEM-/assets/164687651/9fecb9d9-35c1-42c3-b69f-85b5aadd7bb1)

Make sure to save your map! What you can do, is buy the subscription to the ipgeolocation and allow the attacks to continue on and watch your map grow as more attacks happen. I choose to stay with the free trail and therefore only had 1,000 attacks logged. 

I hope you enjoyed the lab! It was very interesting to see all this happen in real-time and gave me vaulable experience with Azure. 

