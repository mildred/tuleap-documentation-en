Tuleap Customisations
=====================

System configuration file (local.inc)
-------------------------------------

The PHP scripts take many values from a configuration file located in /etc/codendi/conf/local.inc. Make sure that you read this file where the role of each variable is well documented.


Organization Logo
-----------------

At the top of the left hand side Tuleap menu pane (just above the Logged In section) is an empty area where you can dock the logo of your organization. This can be achieved by putting your nice logos in /etc/codendi/themes/common/images/organization_logo.png.

This is how Tuleap displays the logo :

    - browser width > 1366px: organization_logo.png (recommended size: 200x45px)
    - browser width <= 1366px AND browser width > 1300px: organization_logo_medium.png (recommended size: 155x45px)
    - browser width < 1300px: organization_logo_small.png (recommended size: 45x45px)

    Background colour: you need to add a background to your image if you want another background colour than the navbar colour. Note: the Tuleap FlamingParrot theme has multiple colour variants.

Site content
------------

Several PHP scripts in the source code contain pieces of text or code that are generally here to give instructions to the users or to provide site-specific information. Typical examples are: the introductory text at the top of the home page, instructions on how to connect to the SVN repository, instructions and guidelines in the project registration process, LDAP repository information, etc.

These pieces of text or code are isolated from the PHP scripts themselves and they are all placed under the top directory 'site-content'. The file path to the content of a given script (or part of a script) follows the following pattern:

site-content/LANG_COUNTRY/src_path/sometext.txt

where:

    LANG_COUNTRY is the standard ISO naming for language and countries: either en_US or fr_FR.
    src_path is the name of the directory under src which contains the script the text belongs to.
    sometext.txt is a file name that contains the piece of text itself.

To customise the content of a given script for your site go through the following steps:

    - Centos6: under /etc/tuleap create the site-content directory if it doesn't exist.
    - Other OS: under /etc/codendi create the site-content directory if it doesn't exist.

For each piece of text that you want to customize, copy the original sometext.txt file under the /etc/tuleap/site-content/
with the exact same path. For instance if you want to customise the introductory text of the home page,
copy /usr/share/tuleap/site-content/en_US/homepage/welcome_intro.txt into /etc/site-content/en_US/homepage/welcome_intro.txt
and edit it as you like.
Be careful that some of these files contain PHP scripting that you probably want to preserve in your customised version.
Deleting the PHP scripting could break the entire PHP scripts.
Ask on the Tuleap devel mailing list if you are unsure of what you are doing.

Read also Localization below for additional information on how to customise interface messages.

Localization
------------

Tuleap source code is localized so the interface is displayed in the user-selected language. Currently, Tuleap supports the English and French languages.

Messages are stored in the site-content directory: there is one message file per service, with the same name as the service, ending with '.tab'. E.g. 'site-content/en_US/tracker/tracker.tab' contains all tracker messages in English.

The format of the message files is very simple: one line per message, with the following format
    key1 tab? key2 tab? your message here

As with other site-content files, you may customize the language files, so that you can change a few specific messages:

    Copy the language file you need to customize from the /usr/share/tuleap/site-content subdirectory to the corresponding subdirectory in /etc/tuleap/site-content (see Site content above).
    Remove all lines that you don't need to change and only keep the lines you will modify.
    Change the messages.
    Repeat the operation for all the languages you need to support on your server.

Custom Tuleap Tours
-------------------

Tours are step-by-step instructions in the form of help buttons in the UI. You can add your own on most parts of Tuleap.
First, create the folder /etc/tuleap/site-content/en_US/tour. **The tours are localised** so you'll need to create ones in each language.

Next, you need to create a file that list which tours are available at which URL.
This is a JSON file and it **must be named tour.json**. It's content must be an array of tour references, e.g.

::

    #contents of /etc/tuleap/site-content/en_US/tour/tour.json:

    [
        {
            "tour_name" : "my_first_tour",
            "url"       : "/plugins/tracker/?tracker={attribute_value}"
        },
        {
            "tour_name" : "my_other_tour",
            "url"       : "/svn/?group_id={project_id}"
        }
    ]

There are 3 placeholders that can be used in the url:
    - **{project_id}** This will match against any numeric project ID, e.g. 114, 256, 8569
    - **{project_name}** This will match against a project short (or linux) name
    - **{attribute_value}** This will match against any attribute value. The value can be a string or an integer.

The **tour_name** must correspond to a JSON file located in the same folder. E.g. my_first_tour.json

::

    #contents of /etc/tuleap/site-content/en_US/tour/my_first_tour.json:

    {
        "steps" : [
            {
                "element"  : "#tracker_report_config_options",
                "title"    : "How to save a tracker report",
                "content"  : "First click here"
            },
            {
                "element"  : "#tracker_report_updater_duplicate",
                "title"    : "How to configure a tracker report",
                "content"  : "Then click here"
            }
        ]
    }

Note that the element corresponds to a standard css selector. It is the element to which the help bubble is binded.
Further documentation on writing steps can be found here: http://bootstraptour.com/api/#step-options Bearing in mind that
the JSON of this file has to be valid.



Finally, each tour is shown on the page until the user decides to "End" the tour. Upon clicking this, a user will not see a tour
by that name again.

Project Web Site
~~~~~~~~~~~~~~~~

When a project is registered on Tuleap a new web site is created for that project. A default home page is installed for that project from the /usr/share/codendi/site-content/en_US/others/default_page.php file. You may want to create your own custom file for your own Tuleap site. To do so, copy the /usr/share/codendi/site-content/en_US/others/default_page.php file in the /etc/codendi/site-content/en_US/others/ directory if not already there. Then, edit the custom file and customize it to your liking
