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

<h2>Step 9 - </h2>
