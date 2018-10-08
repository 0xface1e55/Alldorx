# Alldorx
Allworx "Reset Code" generator

![alt text](https://github.com/billchaison/Alldorx/blob/master/awx.png)

On Allworx 6x servers, the "Allworx System Administration" page provides a "Lost Password" link that allows you to reset the admin user's password.  This link takes you to the device's "lostPass.asp" form.  This form requires a "Reset Code" to be entered, which is usually obtained by contacting technical support or the authorized reseller who sold you the equipment.  If you have inherited an Allworx server and do not have the registration information you can use this script to generate the "Reset Code" needed to change the admin password.

To generate the "Reset Code" from this script you must enter the execution code, which I added to make it a little more interesting.  Hey, I'm not just going to give it away.  It is up to you to guess what the execution code is.  After all, I like a puzzle and I'm sure you do too.  Let's just say that after I deconstructed the algorithm I wanted a beer from a certain BREWER.  That's the only clue I will give you.

[ bash script ]<br />

```bash
#!/bin/bash

echo "Type in the execution code to run this script:"
read SCRIPTCODE

echo "Enter the Allworx \"System Unique ID\":"
read UNIQUEID

echo "Enter the Allworx \"Security Key\":"
read SECURITYKEY

AWXCONSTANT=$(echo -n U2FsdGVkX19GET8j70xSIag/8r56Bh+CkC0DEvELav6gVx0Uz1n4J0ZLxgNaUHJpng9qiiec5ijyHrzxK0H0r2P0YOcALntNc1COfeOUFs14AfPjfC5ChGXlCPz6yLV0S50ROvLacZ7KxNU7q4gkcY/m3+Mp6BRWLYD8SBPG6PPzoDGY5kByI3tLfDUbhdpNPY90BVYmtuHzixnbm4dz+/UsH1cdO94z1npw+tXHiOu+TMhNO9tK4301us3bDOmU6d2U7REIdNkKR37DnNxv0a3GfiHV12NrY5ncbvp1VXBNwE0tAAjopXITzId2Rw2GzBl9/Nrm0yUX8kzaCF5lU0SnFH2vwCppuL4qM1W5R54= | base64 -di | openssl bf -md md5 -d -pass pass:$SCRIPTCODE 2>/dev/null)

if [ $? -ne 0 ]; then
	echo "Failed to decrypt AWXCONSTANT"
	exit 1
fi

RSTCODE=$(echo -n "$UNIQUEID$SECURITYKEY$AWXCONSTANT" | md5sum | cut -c 25-32)

echo -e "\nThe \"Reset Code\" is:"
echo $RSTCODE
```
