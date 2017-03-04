# How-to-crate-PHPFormMail

# Overview

PHPFormMail is a universal WWW form to e-mail gateway.. PHPFormMail's main goal is to seamlessly take the place of it's PERL sister. All functions from PERL FormMail have an exact duplicate in PHPFormMail. PHPFormMail also has some extra functions built in, where ever you see a "*" (red star) is a new feature. Special thanks to Matt's Script Archive (http://www.worldwidemart.com/scripts) for if it wasn't for him/them/her (?) there would be no FormMail period. =:^)  There is only one required form input tag which must be specified in order for this script to work with your existing forms. Other hidden configuration fields can also be used to enhance the operation of FormMail on your site.  The anonymous WWW user must have the ability to read the PHPFormMail.php script. If you have problems running a script with the .php extension, try .php3, .phtm, .phtml. If those do not work, please check with your system administrator for more help on running PHP scripts.  Please be sure not to put the formmail.php in your cgi-bin directory unless your web host tells you to. There is no need to make your php scripts executable.

# Setting Up the PHPFormMail Script
The PHPFormMail.php script does not have to be extensively configured in order to work. There are only two variables in the script in which you will be required to modify. Note: For this section (Setting Up the PHPFormMail Script) you will need to edit the formmail.php.

# Necessary Variables

$referers = array("boaddrink.com","216.92.77.123");

This array allows you to define the domains that you will allow forms to reside on and use your PHPFormMail script. If a user tries to put a form on another server, that is not in your list, they will receive an error message when someone tries to fill out their form. By placing boaddrink in the $referers array, this DOES NOT allow www.boaddrink.com, ftp.boaddrink.com, any other http address with boaddrink.com in it. I included boaddrink.com's IP address to access this script as well, just to be on the safe side. As of version 1.5.0 you can now use regular expressions in the $referers array.

Note: The domains listed in the $referers array must also include the domain that the e-mail is going to.

Example: If the recipient of the e-mail is test@example.com and the form is hosted on boaddrink.com, your $referers array will have to look like

$referers = array("boaddrink.com", "example.com"); 

Remember, the domain name of the recipient's domain must be listed in the $referer array, even if the form isn't hosted on that domain. Rule of thumb, include the domain name that is hosting the form AND include the domain name of the recipients (This includes recipient, recipient_cc, and recipient_bcc).

 

$valid_env = array("REMOTE_HOST", "REMOTE_ADDR", "REMOTE_USER", "HTTP_USER_AGENT");

This array allows you to define enviromental variables that can be reported. The odds are you won't need to change these. For more information on environmental variables, please see env_report.

# Optional Variables

Line:	define('MANUAL','http://www.boaddrink.com/projects/phpformmail/readme.php');
Description:	Modifying the URL listed in this constant will change the address that the script points to in the event of an error. Nine times out of ten you won't have to change this.
Syntax:	define('MANUAL',url);
Valid Values:	Any valid URL that has the readme file
 

Line:	define('CHECK_REFERER', true);
Description:	This constant defines if the script should do referer checking. It it highly recomended that you keep this set to "true" since changing this to "false" could cause your script to become an open relay (ie: allowing spammers to use your server). The only time this constant comes up is when a user submits a form and has an overactive firewall that removes the HTTP_REFERER. In this case, you have two options, change this setting to false or tell the user to relax some of their security settings.
Syntax:	define('CHECK_REFERER',boolean);
Valid Values:	true, false
 

Line:	define('FROM', null);
Description:	This optional field will let you set what e-mail address the e-mail comes from. It will not affect the e-mail address listed in the email field. It's purely for looks it will also not affect the Return-Path field, that is *only* set by the mail server for security reasons.
Syntax:	define('FROM'',boolean);
Valid Values:	A valid e-mail address defined in RFC 2821 or null.
 

Line:	$recipient_array = array();
Description:	This is the only Optional Variable that I recomend changing and using. This array allows you to list valid key/value pairs of e-mail addresses so you don't have to expose your e-mail address to the net. If you define ANY e-mail addresses in the $recipient_array then all forms using this script must pass keys to the script and not email addresses. The format is $recipient_array = array(key => value) and the key can be whatever you like. You'll want the value to be a valid e-mail address. Note: The domain of the e-mail address DOES NOT have to be added to the $referers.
Syntax:	
Example #1
$recipient_array = array(1 => 'email@example.com', 2 => 'somebodyelse@example.com');

Example #2
$recipient_array = array('bob' => 'email@example.com', 'sally' => 'somebodyelse@example.com');

Example #3
$recipient_array = array('fcf7b465cc30923b02a8cecc2ede239331c7990e' => 'email@example.com', ' 2bd7df9fbc9f87040e617b5c09ad16aa85d73ecc ' => 'somebodyelse@example.com');

Example #1 is the least secure, spammers could just keep trying numbers (1, 2, 3) to see who they can get.

Example #2 is more secure since it doesn't follow a sequential pattern.

Example #3 is the most secure of the three examples since it uses completely random number/letters.

Valid Values:	See examples
 

# Form Configuration
The action of your form needs to point towards this script (obviously), and the method must be POST or GET. Below is a list of form fields you can use and how to implement them. All fields listed below need to be added to an HTML form that you create.

# Necessary Form Fields
There is only one form field that you must have in your form, for PHPFormMail to work correctly. This is the recipient field.

Field:	recipient
Description:	This form field allows you to specify to whom you wish for your form results to be mailed. Most likely you will want to configure this option as a hidden form field with a value equal to that of your e-mail address.
Syntax:	
Single Recipient 
<input type="hidden" name="recipient" value="email@example.com">

Single Recipient (using $recipient_array example #2):
<input type="hidden" name="recipient" value="bob">

Multiple Recipients (using $recipient_array example #2):
<input type="hidden" name="recipient" value="bob,sally">

Valid Values:	Any valid e-mail address or key from the $recipient_array
Optional Form Fields

Field:	subject_prefix
Description:	Allows you to specify text to go in front of the subject. This is only usefull when you are letting the user define the subject.
Syntax:	<input type="hidden" name="subject_prefix" value="My website Feedback: ">
Valid Values:	Any text
 

Field:	subject
Description:	The subject field will allow you to specify the subject that you wish to appear in the e-mail that is sent to you after this form has been filled out. If you do not have this option turned on, then the script will default to a message subject: WWW Form Submission
Syntax:	If you wish to choose what the subject is:
<input type="hidden" name="subject" value="Your Subject">
To allow the user to choose a subject:
<input type="text" name="subject">

Valid Values:	Any text
 

Field:	email
Description:	
This form field will allow the user to specify their return e-mail address. If you want to be able to return e-mail to your user, I strongly suggest that you include this form field and allow them to fill it in. This will be put into the Reply-To: field of the message you receive. If you want to require an email address with valid syntax, add this field name to the 'required' field.

Version 1.07.2 changed this from using the From SMTP field to use the Reply-To SMTP to better accommodate SPF.

Syntax:	<input type="text" name="email">
Valid Values:	Any text
 

Field:	recipient_cc*
Description:	This form field will allow you to carbon copy (CC) the the e-mail to the e-mail address listed. Use this only if you have the email field specified.
To send e-mail to more than one address, use commas to seperate the addresses.

Syntax:	Single Recipient:
<input type="hidden" name="recipient_cc" value="nobody@example.com">
Multiple Recipients:
<input type="hidden" name="recipient_cc" value="nobody@example.com,nobody@example1.com">

Single Carbon Copied Recipient (using $recipient_array example #2):
<input type="hidden" name="recipient_cc" value="bob">

Multiple Carbon Copeid Recipients (using $recipient_array example #2):
<input type="hidden" name="recipient_cc" value="bob,sally">

Valid Values:	Any valid e-mail address or key from the $recipient_array
 

Field:	recipient_bcc*
Description:	This form field will allow you to blind carbon copy (BCC) the the e-mail to the e-mail address listed. To use this field, you don't have to specify the email field. This will cause the e-mail to be sent to the recipients listed in the recipient_bcc without the other recipients being able to see who it was sent to.
To send e-mail to more than one address, use commas to seperate the addresses.

Syntax:	Single Recipient:
<input type="hidden" name="recipient_bcc" value="nobody@example.com">
Multiple Recipients:
<input type="hidden" name="recipient_bcc" value="nobody@example.com,nobody@example1.com">

Single Blind Carbon Copied Recipient (using $recipient_array example #2):
<input type="hidden" name="recipient_bcc" value="bob">

Multiple Blind Carbon Copied Recipients (using $recipient_array example #2):
<input type="hidden" name="recipient_bcc" value="bob,sally">

Valid Values:	Any valid e-mail address or key from the $recipient_array
 

Field:	realname
Description:	The realname form field will allow the user to input their real name. This field is useful for identification purposes and will also be put into the From: line of your message header.
Note: If realname does not exist as a field but firstname or lastname does, PHPFormMail will combine firstname and lastname to create the realname.

Syntax:	<input type="text" name="realname">
Valid Values:	Any text
 

Field:	redirect
Description:	If you wish to redirect the user to a different URL, rather than having them see the default response to the fill-out form, you can use this hidden variable to send them to a pre-made HTML page.
Syntax:	<input type="hidden" name="redirect" value="http://your.host.com/to/file.html">
Valid Values:	Any text
 

Field:	redirect_values
Description:	This sends the form results on to the redirect page (for further processing by your own script). You must specify redirect and/or missing_fields_redirect for this feature to work. NOTE: You must use the GET method for this feature to work.
Syntax:	<input type="hidden" name="redirect_values" value="true">
Valid Values:	Any text
 

Field:	required
Description:	You can now require for certain fields in your form to be filled in before the user can successfully submit the form. Simply place all field names that you want to be mandatory into this field. If the required fields are not filled in, the user will be notified of what they need to fill in, and a link back to the form they just submitted will be provided.
To use a customized error page, see 'missing_fields_redirect' 
This will only check to see if there is something in the field it will not validate the syntax of what is in that field. To do that see _regex

Syntax:	If you want to require that they fill in the email and phone fields in your form, so that you can reach them once you have received the mail, use a syntax like:
<input type="hidden" name="required" value="email,phone">

Valid Values:	Valid field names seperated by commas
 

Field:	sort
Description:	There are three ways to sort the output of the form.
Alphabetic*: An alphabetic listing based on the field names.
Reverse Alphabetic*: A reverse alphabetic listing based on the field names.
Order: Allows you to specify the order that you would like the fields to be in. Note: If you have five form variables, and you only wish to move two of them to the begining of the e-mail and allow the others to stay in their natural order, you just need to specify the two variables. The remaining variables will be included in the e-mail.
Syntax:	If you want to require that they fill in the email and phone fields in your form, so that you can reach them once you have received the mail, use a syntax like:
For Alphabetic listings*:
<input type="hidden" name="sort" value="alphabetic">

For Reverse Alphabetic listings*:
<input type="hidden" name="sort" value="ralphabetic">

For Order listings (This would list the fields in the order of: name, comment, email):
<input type="hidden" name="sort" value="order:name,comment,email">

Valid Values:	alphabetic, ralphabetic, order
 

Field:	hidden*
Description:	Allows you to hide the results of a field from the HTML output. The results will be replaced with (hidden) in the HTML output but will still appear in the e-mail you receive. Fields are comma separated.
Syntax:	If you want to hide the results of the fields my_password and home_address, you would use syntax like:
<input type="hidden" name="hidden" value="my_password,home_address">

Valid Values:	Valid field names seperated by commas
 

Field:	alias*
Description:	
Allows you to add a more user friendly name for the field. This user friendly name is only displayed in the HTML output and as no effect on the e-mail you receive. Format is fieldname=friendlyname and is comma separated.

Please use alias_method if you would like the alias to be shown in the e-mail.

Syntax:	To add the alias of "E-mail Address" to the field of "email" and the alias of "First Name" to the field of firstname use:
<input type="hidden" name="alias" value="email=E-mail Address,firstname=First Name">

Valid Values:	Valid field names followed by "=" and then the text you wish to be shown. Fields should be seperated by commas
 

Field:	alias_method*
Description:	Allows to specify if the alias will be used in the HTML results, e-mail or both. This field is not required if you only wish for the alias to be shown in the HTML output.
Syntax:	To show the alias in both the HTML output and the e-mail:
<input type="hidden" name="alias_method" value="both">

Valid Values:	html, email, both
 

Field:	env_report
Description:	Allows you to have Environment variables included in the e-mail message you receive after a user has filled out your form. Useful if you wish to know what browser they were using, what domain they were coming from or any other attributes associated with environment variables. Each enviroment variable is sepperated by a comma. The following is a short list of valid environment variables that might be useful:
 

REMOTE_HOST	-	Sends the hostname making the request.
REMOTE_ADDR	-	Sends the IP address of the remote host making the request.
REMOTE_USER	-	If server supports authentication and script is protected, this is the username they have authenticated as. *This is not usually set.*
HTTP_USER_AGENT	-	The browser the client is using to send the request.
Remember to allow the enviromental variables in $valid_env.

Syntax:	If you wanted to see the host making the request and the browser the user is using in your our e-mail , you would put the following into your form:
<input type="hidden" name="env_report" value="REMOTE_HOST,HTTP_USER_AGENT">

Valid Values:	See http://hoohoo.ncsa.uiuc.edu/cgi/env.html
 

Field:	priority*
Description:	priority allows you to modify the priority of the e-mail you will receive. 3 is normail priority for an e-mail (default) and 1 is the highest priority.
Syntax:	If you wish to have the highest priority:
<input type="hidden" name="priority" value="1">
If you wish to have the lowest priority (normal):
<input type="hidden" name="priority" value="3">

Valid Values:	1, 2, or 3
 

Field:	print_blank_fields
Description:	print_blank_fields allows you to request that all form fields are printed in the return HTML and e-mail, regardless of whether or not they were filled in. PHPFormMail defaults to turning this off, so that unused form fields aren't e-mailed.
Syntax:	If you want to print all blank fields:
<input type="hidden" name="print_blank_fields" value="true">
Valid Values:	Booleans (True or False)
 

Field:	title
Description:	This form field allows you to specify the title and header that will appear on the resulting page if you do not specify a redirect URL.
Syntax:	If you wanted a title of 'Feedback Form Results':
<input type="hidden" name="title" value="Feedback Form Results">

Valid Values:	Any text
 

Field:	return_link_url
Description:	This field allows you to specify a URL that will appear, as return_link_title, on the following report page. This field will not be used if you have the redirect field set, but it is useful if you allow the user to receive the report on the following page, but want to offer them a way to get back to your main page.
Syntax:	<input type="hidden" name="return_link_url" value="http://your.host.com/main.html">
Valid Values:	Any text
 

Field:	return_link_title
Description:	This is the title that will be used to link the user back to the page you specify with return_link_url. The two fields will be shown on the resulting form page as:
Syntax:	<input type="hidden" name="return_link_title" value="Back to Main Page">
Valid Values:	Any text
 

Field:	missing_fields_redirect
Description:	This form field allows you to specify a URL that users will be redirected to if there are fields listed in the required form field that are not filled in. This is so you can customize an error page instead of displaying the default.
Syntax:	<input type="hidden" name="missing_fields_redirect" value="http://your.host.com/error.html">
Valid Values:	Any text
 

Field:	background
Description:	This form field allow you to specify a background image that will appear if you do not have the redirect field set. This image will appear as the background to the form results page.
Syntax:	<input type="hidden" name="background" value="http://your.host.xxx/image.gif">
Valid Values:	Any text
 

Field:	bgcolor
Description:	This form field allow you to specify a bgcolor for the form results page in much the way you specify a background image.
Syntax:	For a background color of White:
<input type="hidden" name="bgcolor" value="#FFFFFF">

Valid Values:	Any text
 

Field:	text_color
Description:	This field works in the same way as bgcolor, except that it will change the color of your text.
Syntax:	For a text color of Black:
<input type="hidden" name="text_color" value="#000000">

Valid Values:	Any text
 

Field:	link_color
Description:	Changes the color of links on the resulting page. Works in the same way as text_color.
Syntax:	For a link color of Red:
<input type="hidden" name="link_color" value="#FF0000">

Valid Values:	Any text
 

Field:	vlink_color
Description:	Changes the color of visited links on the resulting page. Works exactly the same as link_color.
Syntax:	For a visited link color of Blue:
<input type="hidden" name="vlink_color" value="#0000FF">

Valid Values:	Any text
 

Field:	alink_color
Description:	Changes the color of active links on the resulting page. Works exactly the same as link_color.
Syntax:	For a active link color of Blue:
<input type="hidden" name="alink_color" value="#0000FF">

 

Field:	gmt_offset*
Description:	Allows you to specify your GMT offset (+/-) so the time reported in the e-mail will match your time zone. (Default is your servers time zone)
Syntax:	To change the time into American EST:
<input type="hidden" name="gmt_offset" value="-5">

Valid Values:	Any integer (positive or negative)
 

Field:	mail_newline*
Description:	
Allows you to specify the type of linebreak in your e-mail output. Only change if your e-mail is listed all on one line.

1 \n (default)
2 \r\n
3 \r

Syntax:	To change your linebreak to \r\n:
<input type="hidden" name="mail_newline" value="2">

Valid Values:	1, 2, or 3
 

Field:	css*
Description:	Allows for the results/error page to link to a Cascading Style Sheet (css).
Syntax:	To link to main.css:
<input type="hidden" name="css" value="http://your.host.xxx/main.css">

Valid Values:	Any text
 

Field:	_regex*
Description:	Allows for regular expression comparisons on form results. For a field to be checked, it must also be listed in required.
To check a field, append _regex to the hidden field name.

Note: This function uses eregi() so you must use postix regular expressions.

Syntax:	To check that the date field is in propper date format:
<input type="hidden" name="date_regex" value="$[0-9{4}-[0-9]{2}-[0-9]{2}$">

Valid Values:	Any valid postix regular expression
 

Any other form fields that appear in your script will be mailed back to you and displayed on the resulting page if you do not have the redirect field set. There is no limit as to how many other form fields you can use with this form, except the limits imposed by browsers and your server.

 

# Additional Notes

# Multiple Results

When using checkboxes and multiple selection lists, you'll need to add [] to the end of the name. When the [] is added, PHPFormMail can then properly process the field and print it out in a comma separated format.

# Reserved Words

Here is the list of reserved words. Naming a field name any of the following could cause unexpected results:
recipient, subject, required, redirect, print_blank_fields, env_report, sort, missing_fields_redirect, title, bgcolor, text_color, link_color, alink_color, vlink_color, background, subject, title, link, css, return_link_title, return_link_url, recipient_cc, recipient_bcc, priority, redirect_values, hidden, alias, mail_newline, gmt_offset, alias_method, subject_prefix

# Security

While PHPFormMail uses referer checking, recipient checking, recipient arrays and body/header regular expression checks it could still be possible for there to be exploits from spamers. Please always keep an eye on your logs to verify there aren't excessive amounts of e-mail going through your formmail.

E-mails that fail the regular expression check will enter "Injection characters found from IP {ip_address}" in the logs. E-mails that pass all the tests and are sent will have a log entry of "Normal e-mail sent from IP {ip_address}". Also all e-mails will have headers stating the form referer and the senders IP address.

# Additional Help

If you do need any additional help stop by and check out our message boards. Please be sure to check the things to do before posting and also check the frequently asked questions before posting your questions.

# Donations
As you know, most of the software I release is free, and will always be free. Keep in mind that this donation page isn't for me to profit, it's to allow me to continue developing free software and to keep boaddrink.com for you, the user. Please remember that this software is free for you to download and use but that doesn't mean it's not worth anything. You can donate any amount of money you wish... from $1 to $1,000,000. While a million dollars would be nice don't be pressured, any amount helps, even one dollar.

If you wish to donate, please visit http://www.boaddrink.com/donate.php

Thanks,
Andrew Riley

Copyright 2002 Andrew Riley
