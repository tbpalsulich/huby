##Introduction

Huby is the reinvention of a very small wheel, used to make maintaining small websites
easier.

Huby is a website templating program. You put common chunks of code (like headers and navbars)
in a single spot and all of the unique bits of code in another. Huby then combines them all into
pages ready for upload &mdash; doing text substitution (for links and styles) along the way.

##Installation

Clone the repository, (make sure huby is actually safe to be executed (that I'm not a bad guy)),
make huby executable, and make sure huby is on your path.

    git clone git@github.com:tpalsulich/huby.git
    cd huby
    chmod +x huby
    echo $PATH

To get started:

    cd $website_directory
    huby init

Then setup `config.huby` and all of your body files as described below.

##Getting Started

The huby-meat folder holds all of the information huby needs to run. The project
configuration file `huby-meat/config.huby` contains the settings (like output folder)
and desired text subsitutions (like for hyperlinks and your header/footer/etc
files). When you refer to files in `config.huby`, their location is taken as relative
to the `huby-meat` folder. See more detail in the Configuration section.

The `huby-meat/bodies/` folder holds all of the unique content of your site. These files
should be of the form '[name].huby', where [name] is your desired name of the output file
(for instance, 'index.html.huby' or 'contact.php.huby').

To get a sample `config.huby` and set of '*.huby' files, run

    huby init [output folder]

where `[output folder]` has all of your existing website files in it.

Huby will create `.huby` files inside `huby-meat/bodies/` for every file in `output folder`.
`huby init` will also generate a sample `config.huby` with the output folder initialized to
`[output folder]` and sample entries for all of the files in the same folder. Now, whenever
you run huby, it will generate the files and write them (maintaining the folder structure in
`huby-meat/bodies/`) to `[output folder]`.

TODO: Autodetect repeated chunks of code.

##Configuration

All of the settings for huby should be in `config.huby`. The format for entries in `config.huby`
is:

    output_folder               => public,

    @header                     => @header.huby,
    @footer                     => @footer.huby,
    @navbar                     => @navbar.huby,

    active_button               => btn active,

    index                       => index.html, 
    index_button                => btn,
    projects/palsulich          => projects/palsulich.html,
    projects/palsulich_button   => :projects_button,

Whitespace doesn't matter. "output_folder", ",", "=>", "@", "_button", "active_button", and ":" do, though.
* "," and "=>" are used to separate different entries and keys/values. 
* "@" signifies this content is in a file located at the specified name (relative to `huby-meat/`).
Use files for your common chunks of code like your navbar. 
* "_button" indicates elements that will need specific styling when that page is active. When it
is active, its value will be set to the value pointed to by "active_button". The part before the "_"
should be the same as the key for the page the button will be active on, as the index button is above.
Huby will automatically check if there is a "_button" entry.
* "active_button" indicates what the value a "_button" entry should have when that button is active.
* ":" indicates the state of this button is connected to the button after the ":". So, whenever the
projects/palsulich_button value would be activated, the projects_button value is instead.
This is useful for dropdown menus.

##Using the Configuration
Inside any `.huby` file, whenever you write `{{{key}}}`, it will be replaced by what is pointed to
by `key` in `config.huby`. When 

A line in `navbar.huby` could look like:

    <li><a href="{{{index}}}" class="{{{index_button}}}">Home</a></li>

It will be replaced by either,

    <li><a href="index.html" class="btn">Home</a></li>

or

    <li><a href="#" class="btn active">Home</a></li>

Depending on what file is currently being written.

The most basic `*.huby` file with the header, navbar and footer is:

    {{{header}}}
    {{{navbar}}}

    {{{footer}}}

Put whatever makes this page unique between.