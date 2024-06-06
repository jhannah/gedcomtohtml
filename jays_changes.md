In 2005 my entry in my GEDCOM looked like this:

    0 @I1265@ INDI
    1 NAME Jay Weston /Hannah/
    1 SEX M
    ...
    1 NOTE Homepage: http://jays.net
    2 CONT There you will find more info about me than you ever wanted. :)

But now in 2024, using a new editing tool [GEDitCOM II](https://geditcom.com/GCFeat.html), my entry has changed to this:

    0 @I1265@ INDI
    1 NAME Jay Weston /Hannah/
    1 SEX M
    ...
    1 NOTE @N493@
    ...
    0 @N493@ NOTE
    1 CONC Homepage: http://jays.net
    1 CONT There you will find more info about me than you ever wanted. :)
    1 REFN 31 MAY 2024 20:18:02
    2 TYPE Creation Date
    1 _GCID 84F5B72D-A7CC-4276-8B88-2CCB053B0504

And Dan Pidcock's GedcomToHTML v1.5.6 (~2006) doesn't render the note anymore. So I'm attempting to update GedcomToHTML.pl [in my fork in GitHub](https://github.com/jhannah/gedcomtohtml).

Debugging Dan's GEDCOM parsing is quite a bit of work, there's no test framework. Maybe I should update Dan's script to use CPAN for GEDCOM parsing instead of doing the parsing itself?

If I use Paul Johnson's [Gedcom.pm](https://github.com/pjcj/Gedcom.pm) from CPAN (~2019) `->resolve()` is available, which seems to work fine on my data:

    use 5.38.0;
    use Gedcom;

    my $ged = Gedcom->new("/Users/jhannah/Desktop/jay_new.ged");
    my $me = $ged->get_individual("I1265");
    say $me->note;
    say "---";
    say $me->resolve($me->note)->full_value;

    N493
    ---
    Homepage: http://jays.net
    There you will find more info about me than you ever wanted. :)

I'm open to any feedback from Dan or Paul about how folks are maintaining GEDCOM Perl software nowadays. And how my contributions can be most helpful. If there are any active mailing lists or chat rooms or anything nowadays (2024), I'd love to join those. The links I've found seem to be dead?

Cheers,

[Jay Hannah](http://jays.net/genealogy)


## Options

1. Refactor `gedcomToHTML.pl`
   - to resolve INDI NOTEs.
   - `use strict; use warnings;` so every scalar isn't a global floating around.
   - replace parsing code, use Gedcom.pm instead.
2. Extend [`Gedcom::CGI`](https://github.com/pjcj/Gedcom.pm/blob/master/lib/Gedcom/CGI.pm#L86-L122)
   - to do all the slick things `gedcomToHTML.pl` does.
   - to generate static files instead of relying on CGI. (e.g. [all these links](https://pjcj.net/genealogy.html) are 404.
     Static HTML tends to last decades, CGI tends to fail faster. Most people's GEDCOMs aren't millions of people,
     so static is fine, faster, simpler to host.)
3. Switch from `gedcomToHTML.pl` to [`ged2site`](https://github.com/nigelhorne/ged2site)
   - 2024-06-06: Opening lots of tickets, working with Nigel <3 e.g. https://github.com/nigelhorne/ged2site/issues/114
