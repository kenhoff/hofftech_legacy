# Field of Heroes

## Summary

The Field Of Heroes, a memorial in Central Ohio, is a field of almost 7000 crosses, each one dedicated to a soldier who fell during the engagments in the Middle East. We built an app that generated individual QR codes that could be placed on each cross, and when scanned, would provide more information about that soldier.

## Description

Try it out: QR code

During my time at Microsoft, I had the chance to work closely with two engineers who are now great friends of mine: Ryen Macababbad and Chadwick Hasbrook.

Ryen and Chad are former Army and Air Force (respectively) service members who have gone through the [Microsoft Software and Systems Academy](http://westillserve.com/mssa/) (MSSA), a program dedicated to preparing former US active duty service members for high-tech IT and software development jobs.

After her graduation, Ryen kept in touch with a mailing list for graduates of the program. She met Cassondra Slaughter, another Microsoft employee who was looking for volunteers to help with a memorial in her hometown in central Ohio.

The Field of Heroes is a memorial of almost 7000 crosses, each one dedicated to a service member who fell during the engagements in the Middle East. Cassondra offered to help the organizers of the memorial using Microsoft's talent and resources. They decided that a way that Microsoft could help would be to put together an app that would scan codes that would be placed on the crosses, and display additional information about the soldiers.

Cassondra was looking for a web developer; she already had another graduate of the MSSA program, Zane Coppedge, who had been working on the project so far. They had a list of soldiers, and Zane had been populating a SQL database with all the information. Ryen put out some feelers on the Azure Active Directory team, to see if anyone knew any web developers who would be interested in donating some time on the project.

I have some feelings on the nature of volunteer work. Anyone can donate time; I can volunteer at the food bank, or spend a day at a park doing some trail work. However, I also have a very specialized set of skills, and if I donate my technical time and expertise in the right way, it can be orders of magnitude more valuable to the organization than providing unskilled labor. 

(reword that) ^

Ryen introduced me to Cassondra and Zane, and we set up a 3-hour hack session every two weeks in the Garage at Microsoft. Cassondra was managing the project and communicating with the organizers, and Zane was working on getting the data into a database and available for usage. I'd be responsible for generating QR codes and the web server, and making sure that when a QR code got scanned, the right soldier's information would come up.

Our architecture ran entirely on Microsoft Azure, with a single webserver to display the pages and generate QR codes, and a SQL database paired with a WCF API. The webserver was running node.js, using the Express framework, and using Jade templates to compile and serve static pages for each soldier; no dynamic pages here!

Generating the QR code and having the webserver display the correct information wasn't hard, but this project presented some interesting nontechnical challenges. How do you get ~7000 QR codes, each individually keyed to a specific cross, handed off to some nontechnical event organizers, who will need to print stickers for each one and place them on the correct crosses?


presented to MSSA group, served as some mentorship opportunities
donated lots of time to the org through //give, microsoft's employee time matching program

