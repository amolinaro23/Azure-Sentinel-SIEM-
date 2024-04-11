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


<h2>Step 4 - </h2>
