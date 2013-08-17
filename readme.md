Huby
====
Huby is the reinvention of a very small wheel, used to make maintaining websites easier.

Why?
----
I got tired of copying and pasting changes to every page of a (mostly) static
website. Particularly for fonts (declared in the header) and changes to the 
navbar. Also, I wanted to make something myself. Pull requests are welcome!

Basics
------
To use huby, put the content of your
* *Header* from `<html>` to `<body>` inclusive in `huby-meat/header.huby`.
* *Footer* from whatever your footer is (still in `<body>`) to `</html>` 
in `huby-meat/footer.huby`.
* *The rest* of the content between the header and the footer goes in 
`huby-meat/bodies/*.[extension].huby`, where `[extension]` is `html`, `php`
, or whatever the extension of the file should be.

For every file in `huby-meat/bodies/`, huby will open a file for writing at 
`public/[filename]` where `[filename]` is simply whatever is in the bodies 
folder minus '.huby'.
Then it will write the header, then the content, then the footer. Simple.

Links
-----
If you wish to make a hyperlink (who would want that?), huby uses a 
pseudo-templating language to fill in what the exact url is. First, 
fill in the links hash with urls you will link to. For example, 

    links = {
        "index" => "index.html"
    }

Now, whenever huby finds an instance of '{{{index}}}' in any .huby files, 
it will replace it with 'index.html' unless the current file *is* index.html. 
In that case, a '#' is inserted.

Navbar
------
If you want to use a navbar, you have a couple options. You could put all of the
navbar content at the bottom of the header file, where it will get copied to the
top of every body. Or, you can add another entry to the `links` hash, where the 
value is simply the content of your navbar! For example,

    navbar = File.open("huby-meat/navbar.huby") {|nav| nav.read()}
    links = {
        "index" => "index.html"
        "navbar" => navbar
    }

Then, inside your header (or at the top of every body), put '{{{navbar}}}'. The 
same may obviously be done with the footer or any other piece of reused code.

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
