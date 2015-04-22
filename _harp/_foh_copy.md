# Field of Heroes

## Summary

The Field Of Heroes, a memorial in Central Ohio, is a field of almost 7000 crosses, each one dedicated to a soldier who fell during the engagments in the Middle East. We built an app that generated individual QR codes that could be placed on each cross, and when scanned, would provide more information about that soldier.

## Description

Try it out: QR code

During my time at Microsoft, I had the chance to work closely with two engineers who are now great friends of mine: Ryen Macababbad and Chadwick Hasbrook.

Ryen and Chad are former Army and Air Force (respectively) service members who have gone through the [Microsoft Software and Systems Academy](http://westillserve.com/mssa/), a program dedicated to preparing former US active duty service members for high-tech IT and software development jobs.

After her graduation, Ryen kept in touch with a mailing list for graduates of the program. She met Cassondra Slaughter, another Microsoft employee looking for volunteers to help with a memorial in her hometown in central Ohio.

The Field of Heroes is a memorial of almost 7000 crosses, each one dedicated to a service member who fell during the engagements in the Middle East. 

Heard of a project called the Field of Heroes, memorial in Cassondra's hometown in ohio, 7000 crosses dedicated to soldiers who fell during the engagements in the middle east

The project was looking for a web developer - Ryen put out some feelers, I volunteered my time
Aside: volunteering low-skilled labor vs high-skilled labor
I got involved, worked with Cassondra and Zane
began meeting every tuesday for a couple hours in the Garage, 

presents some interesting non-engineering problems; scanning the QR code and redirecting to a specific URL is easy. so that means you'll need a specific QR code for each soldier. how are the QR codes going to be attached to the crosses? is it just stickers that get placed on the crosses, or printed off, cut out and taped on there? how do the organizers (potentially nontechnical people) know which QR codes go on which crosses? if they're printing them on stickers, what format do they need them in? some of these are more logistical problems than anything else
service generated ~7000 individual QR codes, presented to them in a printable format

running on node, express, using jade as a template engine. Zane had already begun development on the database by the time we started, so we just rolled forward with a WCF API to his SQL database with the soldier data.

presented to MSSA group, served as some mentorship opportunities
donated lots of time to the org through //give, microsoft's employee time matching program

