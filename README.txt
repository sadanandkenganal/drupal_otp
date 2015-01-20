OTP provides an additional layer of security to the login process by
requiring users to enter a randomly generated one-time password (OTP)
in addition to their regular Drupal password.

One-time passwords can be sent via email or SMS and is configurable 
at the user level. 

Not all users require OTP access on a Drupal system. Therefore, OTP 
provides the option to enable OTP authentication based
on user roles.   


Pre-Requisites
--------------
- Drupal 7.14
- SMS Framework module
- Clickatell module
- an Clickatell (www.clickatell.com) account or something similar
 
 
Installation
------------
1. Download and extract the OTP archive file. Copy the OTP folder to your 
   appropriate Drupal modules directory.

2. If email is used to send OTP then download phpMailer from 
   http://phpmailer.worxware.com. Extract and place the class files in a 
   subfolder named "phpMailer" within the OTP directory. There should be 
   3 files: class.phpmailer.php, class.pop3.php and class.smtp.php. 
   
   The directory structure should look something like this:
   ../modules/otp/phpMailer/class.phpmailer.php
   ../modules/otp/phpMailer/class.pop3.php
   ../modules/otp/phpMailer/class.smtp.php
   
   Note: class.pop3.php can be omitted since the OTP module does not perform
   any POP mail functions.
   
   You will also need to enable OpenSSL extension in your PHP configurations. 
 
3. If SMS is used to send OTP then enable cURL extension in your PHP configurations. 
    
   Next, download the file "cacert.pem" from http://curl.haxx.se/docs/caextract.html 
   and place the file in the main OTP module directory. 
    
   For example: ../modules/otp/cacert.pem
  
3. Enable the OTP module on the modules administration page.


OTP Module Configuration
------------------------
1. Decide whether OTP will be sent via email or SMS or both. 
   
   For email, it will have to be with a secure SMTP server. I find GMail 
   works well for me. I create 2 GMail accounts, one for the server (ie. OTP module)
   to send the OTP and one for the mobile client (ie. email enabled phone) to
   receive the OTP. Keeping the two email account with the same provider will
   provide the fastest response. With GMail, the OTP is usually received 
   within a minute of sending.
   
   For SMS, go to www.clickatell.com and get yourself an account. Use the
   HTTPS API format.
   
2. From the Drupal Administration screen, go to "One-Time Password" under 
   "Site Configuration".

3. Specify the password length. It must be a minimum of 6 characters and a
   maximum of 30. 
   
4. Specify the OTP expiry. This is the length of time (in minutes) which the
   generated OTP is valid for. The minimum time is 1 minute and the maximum is
   1440 minutes or 24 hours. 
   
   Note: once an OTP is used, it cannot be used again even if it is used within
   the expiry time limit.
 
5. Specify the user roles which will require OTP. If you check "authenticated user"
   then all users will be required to login using OTP.

6. Check the SMTP or Clickatell checkboxes to enable the services. If either or 
   both checkboxes are marked, then enter the appropriate values as obtained in
   Step 1.
   
7. Create a user role that will be responsible for administering OTP users. Then, go
   to User Permissions and assign the permission "otp settings" to that role.
   Note: You will also need to assign the usual Drupal permissions like "access user 
   profiles", "adminster users" and "administer permissions"  
   
User Configuration
------------------
1. Go to the user account edit screen

2. Enable the OTP role (as set in step 5 above) for the user.

3. Select the OTP sending method (ie. email, SMS)

4. Enter the email address or SMS number to send the OTP.
   Note: If using SMS, then a telephone (without prefix or punctuation marks and 
   with the country code) must be provided. However, if using email and the
   user's OTP email field is left blank, the OTP will be sent to the user's default
   Drupal email address.
   
   
OTP Login Process
-----------------
* Visit http://www.sitename/otp/login

1. When the OTP module is enabled, the standard Drupal login block and login form will 
   have a new hyperlink pointing to the OTP Login form. Normal users will continue to
   login using the standard Drupal login. OTP users will click on the new hyperlink
   to bring up the OTP login form.

1. From the OTP login form, the user enters their user name and click on "Request OTP". 
   The OTP module will generate a random password and send it to the user's email address
   or SMS number.
   
   Note: If the user enters an invalid username, the OTP module will still behave as
   if a valid username was provided rather than generate an error message. This is
   intentional and is to prevent hackers from trying to guess what are valid usernames
   on the system. When an invalid username is entered, an entry is created in the
   system log. 
   
2. The OTP login form is redrawn with the following fields:
   - User Name (pre-filled and non-edittable)
   - Password (this is the user's standard Drupal password)
   - OTP Password
   
3. The user receives the OTP as a SMS or email message on their mobile phone

4. The user enter both their static and OTP passwords and successfully log
   into the system.

Notes
-----
1. Drupal 7.14 or higher is required
