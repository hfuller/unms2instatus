# unms2instatus

This little script converts the emails UNMS sends to InStatus dashboard updates. This results in an InStatus dashboard that automatically reflects the status of certain devices in UNMS.

## Prerequisites and caveats

 - UNMS has to be able to deliver mail locally on your system. (Only exim has been tested.)
 - Your SMTP listener has to be listening on an IP other than 127.0.0.1 or ::1. (UNMS can't connect to those because it runs in Docker.)
 - Your devices in UNMS can't have spaces in their hostnames. (Sorry.)

## Setup

 1. Clone this repo somewhere cool like /opt/unms2instatus.
 2. Set the email forward for a user on your system to point to the following: `|/opt/unms2instatus/ingest` (including the pipe). I use the user 'alerts' in this example. You can set it in /etc/aliases or in ~alerts/.forward
 3. Configure UNMS to use this machine as its SMTP host.
 4. Add an UNMS user with email address alerts@this-machines-fqdn and enable alerts for that user.
 5. Grab your webhook URLS from InStatus. They can be found under the Runscope section of the Apps tab.
 6. Edit /usr/local/etc/unms2instatus.map and add mappings for your UNMS devices. Each line should be like: `device-name https://webhook-url` (just one space separating the two).

Now, whenever UNMS sends out an alert, it will run the ingest script, which will update the corresponding component on the InStatus dashboard.

## Tips
You can also have multiple lines in the map file for each device, so if you need to update two components when a single UNMS device goes offline, you can do it. This would, for instance, allow you to provide "separate" human-readable statuses for the 1st floor and the 2nd floor, even though they are really served by the same UNMS device.

There is an example map file if that helps.

