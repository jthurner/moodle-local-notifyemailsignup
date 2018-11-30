## What this Moodle plugin is for ##

This Moodle plugin is a fork of [notifyemailsignup](https://github.com/iarenaza/moodle-local-notifyemailsignup) by Iñaki Arenaza, with some minor modifactions to require admin confirmation for any new account registerd with email self-registration.

The plugin immediately suspends all users created via the 'Email signup' authentication plugin and sends an email notification message to the 'Support email'. The notification message contains some essential details about the account just created (email address, full name and user account name) and a link to the user profile. Until an admin un-suspends the account, the user will not be able to login even if they confirm their account. 

The notification email is sent when the user signs up, not when the user account is confirmed. So the plugin will notify even about accounts that may never be confirmed.

### Drawbacks and alternatives ### 

- having to confirm their account AND wait for admin approval can be confusing for users,
the process has to be communicated in welcome/confirmation email text
- no automatic notification email is sent when the Admin unsuspends the account

The "[Email-based self-registration with admin confirmation](https://moodle.org/plugins/auth_emailadmin)" plugin by Felipe Carasso provides better workflow (confirmation email is sent to the user only after admin approval) but forks auth/email/auth.php from core instead of using the event system, which potentially breaks signup (e.g. currently custom/locked fields) if the code isn't updated in lockstep with moodle.


## Supported Moodle Versions ##

The plugin currently works with Moodle 2.7 or later versions.

## Installation ##

This is a standard [Moodle Local Plugin](https://docs.moodle.org/dev/Local_plugins),
so you can follow the standard installation instructions for Moodle
Plugins at https://docs.moodle.org/en/Installing_plugins . Note that
if you install this plugin manually at the server, you need to install
it inside the 'local' directory at the top of the moodle installation
directory.

## Configuration ##

The only configuration used by the plugin is the Support Contact
settings. It uses the 'Support name' and 'Support email' settings as
the recipient of the email notification messages it sends.

You can customise the content/wording of the notification messages by
editing the language strings of the plugin, e.g., through the built-in
'Language customisation' mechanism. All the ``user`` table fields and
custom profile fields are available in the $a object as
{$a->signup_*valuename*}. The syntax of *valuename* depends on whether
the value comes from the ``user`` table fields or from the custom profile
fields (this is due to an unfortunate limitation of the Moodle
language strings interpolation syntax).

* For the ``user`` table fields, the syntax for *valuename* is
  user\_*fieldname*, where *fieldname* is one of ``user`` table fields like
  ``id``, ``username``, ``auth``, ``firstname``, ``lastname``,
  etc. The ``password`` field does not contain the actual password,
  for security reasons.
* For the custom profile fields, the syntax for *valuename* is
  profile_*profileshortname*, where *profileshortname* is the
  **shortname** of the custom profile field.

The following examples may help understand the syntax. Assuming we
have two custom profile fields, whose short names are
``signupcategory`` and ``referralcode``, we could use the
following values in the notification message language string (only
some of the user table fields are shown for brevity purposes):

* ``{$a->signup_user_id}``: this will be substituted by the account ``id``.
* ``{$a->signup_user_username}``: this will be substituted by the account ``username``.
* ``{$a->signup_user_lastname}``: this will be substituted by the account ``lastname``.
* ``{$a->signup_user_city}``: this will be substituted by the account ``city``.
* ``{$a->signup_profile_signupcategory}``: this will be substituted by
  the content of the custom profile field whose shortname is ``signupcategory``.
* ``{$a->signup_profile_referralcode}``: this will be substituted by
  the content of the custom profile field whose shortname is ``referralcode``.
* ``{$a->user_profile_link}``: this will be substituted by the URL for editing the user's account.



