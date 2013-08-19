Huby
====
Huby is the reinvention of a very small wheel, used to make maintaining websites easier.

Why?
----
I got tired of copying and pasting changes to every page of a (mostly) static
website. Particularly for fonts (declared in the header) and changes to the 
navbar. Also, I wanted to make something myself. Pull requests are welcome!

Getting Started
------
You have a couple options when you first start using huby. You can create
the folder structure yourself or you can let huby do it for you. To do it yourself,
move to whatever folder you have your website source files:
    mkdir huby-meat
    cd huby-meat
    touch header.huby
    touch footer.huby
    touch config.huby
    mkdir bodies
Then fill in the files/folders as described below. Alternatively, run 
`huby init [output folder] [ignore folder]` inside your root website directory.
Huby will create the folders and files above then parse through `output folder`,
ignoring items in `ignore folder`, creating .huby files within the `bodies` folder.
`huby init` will also initialize config.huby with the specified output folder, and
sample entries for all of the files. Read the links section for more information on
how each entry is structured. You'll want to look through the created files and delete
ones you don't want huby managing. In other words, pages that don't have a common header
and footer. TODO: Autodetect the header and footer.

Using your already existing website, put the content of your
* *Header* from `<html>` to `<body>` inclusive in `huby-meat/header.huby`. This should
be the same in every file you're going to have huby manage.
* *Footer* from whatever your footer is (still in `<body>`) to `</html>` 
in `huby-meat/footer.huby`. Again, this should be the same in every file.
* *The rest* of the content between the header and the footer goes in 
`huby-meat/bodies/*.[extension].huby`, where `[extension]` is `html`, `php`
, or whatever the extension of the file should be. This body portion will probably be mostly
unique. It will get sandwiched between the header and footer. You will also probably have
many of these files.

Configure config.huby next. Ensure you have the proper output folder specified. If you don't
specify any in this file, huby will default output to `public`.

Then execute `huby run`.
For every file in `huby-meat/bodies/`, huby will write the header, the content of the file,
then the footer in `[output foder]/[filename]` where `[filename]` is simply whatever is in
the bodies folder minus '.huby'. Simple.

Links
-----
If you wish to make a hyperlink (who would want that?), huby uses a 
pseudo-templating language to fill in what the exact url is. First, 
fill config.huby with urls you will link to. For example, 

    index => index.html

Now, whenever huby finds an instance of '{{{index}}}' in any .huby files, 
it will replace it with 'index.html' unless the current file *is* index.html. 
In that case, a '#' is inserted.

Navbar
------
If you want to use a navbar, you have a couple options. You can put all of the
navbar content at the bottom of the header file, where it will be copied to the
top of every body. Or, you can add another entry to the config.huby, where the 
value is simply the name of the file with the content of your navbar! You must
put an '@' symbol before the key and value. For example,

    @navbar        => @navbar.huby,
    index          => index.html


Then, inside your header (or at the top of every body), put '{{{navbar}}}'. The 
same may obviously be done with the footer or any other piece of reused code.
Huby will replace whatever it finds with the value in config.huby.

Output Directory
----------------
If you want an output folder different than `public`, you must specify it in
`config.huby`. Simply add an entry as follows,

    output_folder  => example_folder,
    @navbar        => @navbar.huby,
    index          => index.html
Huby will create the folder if it doesn't exist, then write all of its output inside
that folder.

**NOTE**: The syntax is not very friendly in config.huby. Each entry must be separated by
a comma (','), every key/value must be separated by "=>", and there must not be a comma
after the last entry.

Active Buttons
--------------
The links logic from above can be applied to buttons as well. If you have several 
buttons in your navbar, but you want the current page button to be styled differently 
than the rest of the buttons, then create an entry in the `links` hash with the key 
`[key of links hash entry of the page being linked to]_button` with whatever the 
default button class is (like Bootstrap `btn`) as the value. Then, huby will catch 
when a button should be changed to active (when that body is being written), and 
append `active` to the value in the hash. For example,

    navbar = File.open("huby-meat/navbar.huby") {|nav| nav.read()}
    links = {
        "index" => "index.html"
        "index_button" => "btn"
        "navbar" => navbar
    }

And now, the navbar entry in `navbar.huby` for index would look like:

    <li><a href="{{{index}}}" class="{{{index_button}}}">Home</a></li>

If huby is writing index.html, '{{{index}}}' will be replaced by '#' and 
'{{{index_button}}}' will be replaced by 'btn active'. Other wise, they will be 
'index.html' and 'btn', respectively.
