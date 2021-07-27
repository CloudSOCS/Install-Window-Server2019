Windows Server 2019 Active Directory Domain Controller Install
---

In this tutorial, I will guide you through configuring a Microsoft Windows Server 2019 Active Directory Domain Controller.

We do this by first using the GUI method, then we repeat the same process using Powershell automation.

I will post a video at the end of the tutorial.

I have already done a base install of [windows server 2019](https://cloudsoc961429112.wordpress.com/windows-server/) using VMWare fusion in another tutorial.

So lets login to our server, The first thing I'm going to do is set up a static IP address.

Double click on IPv4 and select Use the following IP address and for the IP address type in 192.168.95.20 and the Subnet mask to 255.255.255.0 and the default gateway to 192.168.95.2 and for my perfered DNS server Im going to type in 127.0.0.1

![ip dns](/images/1.png)

Now lets close these windows by clicking on ok.

Now I'm going to open up server manager
First thing to do here is enable RDP

![rdp](/images/3.png)


 (allow remote connections) and click OK.

![allow](/images/2.png)

  Next I will disable the enhanced security configuration.
![disable](/images/4.png)

  I will turn them both off. OK.

  ![off](/images/6.png)

  Then set the server host name. Now click on the computer name,
![host](/images/7.png)

then on change, and for the name I'm going to keep it simple and call it DC1.
![name](/images/8.png)

now that we changed the host name we must reboot the server , but before I do that I will change my time zone.
![reboot](/images/9.png)
For me it will be US central time (US and Canada).
![time](/images/10.png)

Now I will restart the server.

Once it reboots I will sign in again and open server manager and click on Add roles and features.
![roles](/images/11.png)
You're presented with a welcome screen, click next,
![welcom](/images/12.png)
click next for role-based or feature-based installations,
![next](/images/13.png)
we have our server selected so click next,
![server](/images/14.png)
now here we are going to select Active Directory Domain Services
![active](/images/15.png)
and then next will click on add features,
![features](/images/16.png)
just make sure you have Include management tools selected as well
and then click on next, the features have already been added so click on next,
![alreadt](/images/17.png)
we are only going to be installing one active domain controller , however in production you would want at least 2 active domain controllers, click on next, and click install. The installation has completed successfully, now what we want to do is click  promote this server to a domain controller.
![promote](/images/18.png)

Now we want to select add a new forest. I'm going to give a domain name of cloudsoc.local and click on next,
![forrest](/images/19.png)
So now that this is the only active domain server I'm happy with the default settings here. However if you select the drop down menu you can see that we can also select backward compatibility. Next is to type in a password it doesn't have to be the same as the administrator password, but for this tutorial I will keep them the same,
![default](/images/20.png)
now click next, you will see a warning, we don't need to create a delegation, so click on next,
![delegation](/images/21.png)

for the NetBIOS this will be auto populated, we will leave that as default and click on next,
![auto](/images/22.png)

Next is the database , log files, and SYSVOL I'm going to leave this as the default , because I only set my server up with one drive, and click on next.
![sys](/images/23.png)

Next you will see a summary of all the settings we selected, click next,
![summary](/images/24.png)

Prerequisites have been run, and the first warning you see is a security setting , the next warning is we cannot find a  parent name for our domain name, and thats good because we only want it to exist internally , and you can see that all the Prerequisites Check Completed so now we can click on install
![install](/images/25.png)

Now the server is automatically going to be restarted.
![autostart](/images/26.png)

The server rebooted successfully so now I will log back in. Notice that we are logging into our active domain now.
![domain](/images/27.png)

I like to have DNS on my taskbar, so click on start and type DNS, right click and choose pin to taskbar.
![dns](/images/28.png)
I will do the same for active directory users and computers.
![actdir](/images/29.png)

Lets click on the DNS icon, I'll double click on my server,
![server](/images/30.png)
If I click on forward lookup zone you can see the zone cloudsoc.local now click on cloudsoc.local and you can see our host name records.
![hosts](/images/31.png)

What we need to do now is set up a reverse lookup, Im going to click on reverse lookup zone, Ill right click then select new zone,
![zone](/images/32.png)
next, we will make it a primary zone,
![prime](/images/33.png)

next, and select to all DNS servers running on domain controllers in the domain cloudsoc.local , next,
![toall](/images/34.png)
It will be an IPv4 lookup zone, next,
![ipv4](/images/35.png)
here Im goin to type 192.168.95  , next,
![revers](/images/36.png)

Allow only secure dynamic updates, next and finish.
![only](/images/37.png)

Now right click on DC1 and go to properties,
![prop](/images/38.png)

and select forwarders, now here we can select DNS forwarders , if you don't select the forwarder its going to use the root hint,
![forward](/images/39.png)

and you can see root hints by clicking on root hints,
![root](/images/40.png)

now Im going to select the advance tab and Im going to select enable automatic scavenging and leave it set to 7 days, and this going to remove all DNS records that don't need to be there anymore.
![adv](/images/41.png)

Now select the monitoring tab and we will do a simple DNS test, we will do a simple query and then a recursive query, test now, make sure they both pass. Click on apply and OK.
![monitor](/images/42.png)

Now right click on cloudsoc.local go to properties and click on aging and select scavenge resource records, and leave it set to 7 days, OK, and OK again. That completes the DNS setup so close the window.

![scav](/images/43.png)

Now lets go to Active directory Sites and Services, so click on start and type sites and open up active directory sites and services
![site](/images/44.png)

expand sites, select subnet folder, right click on subnet and select new subnet,
![sub](/images/45.png)

for the prefix Im going to type in 192.168.95.0/24 thats my local IP subnet  and then Im going to select default first site name, OK.
![ip](/images/46.png)

Now if I expand default first site name and then servers, you can see that my server DC1 is registered here
![expand](/images/47.png)

you can right click on default first name and change it to whatever name you want, Im going to leave the default name. What this means here if we are in a multi site environment with multiple subnets and multiple domain controllers it knows when a user signs in to a specific subnet it will use the the domain controller that subnet is tied to. So if Im coming from 192.168.95.0 it knows Im server DC1 . Now close the window.

Now open up active directory users and computers that you added top your taskbar
![AD](/images/48.png)

now I selected cloudsoc.local and you can see we have a few folders, if I select the computer folder this is for if I join a computer to this domain, in the next folder is the domain controller so you will see DC1 and the users folder and you will see the administrator user along with some groups,
![user](/images/49.png)

now you can right click on cloudsoc.local and select new organizational unit,
![org](/images/50.png)

I can type in Windows Admin, and click OK,
![admin](/images/51.png)
and from our new folder we can right click, select new,and select user,
![new](/images/52.png)

type in the first and last name, Windows Admin1, then the user login name, windowsadmin1, next,
![new](/images/53.png)

 give the user a password, this is an Admin account so Im going to deselect user must change password at root login, next. and finish.
![pass](/images/54.png)

So double click on the user account , we want to make it a an administrator, click member of tab,
![mem](/images/55.png)

click on add, type domain admin, check names, OK,
![add](/images/56.png)


select domain admin and set Primary group and then go to domain users and remove that group, select yes, and OK, now close active directory. So thats what it takes to set up active directory domain controller GUI method. Lots of steps involved. Now if you're setting multiple domain controller you want to automate this process. next I will show you how to automate this process.  


Video going over the installation of server 2019  

[![Windows Server 2019 Install ](https://res.cloudinary.com/marcomontalbano/image/upload/v1627411032/video_to_markdown/images/youtube--6uoj5dACSeU-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/6uoj5dACSeU "Windows Server 2019 Install ")   
