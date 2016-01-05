Spectrum
========

A multi-line text panel *SPECTRUM* has 15 rows by 30 columns and it hangs out at a few schools and canteens here in the Czech Republic. So instead of dull data like opening hours wouldn't it be nice to have a room(s) schedule(s) there?

![SPECTRUM display](d/display.jpg)

The display needs to load all the files at once. We use [Windows Task Scheduler](https://en.wikipedia.org/wiki/Windows_Task_Scheduler) to run a yet-to-be-written script every morning just before classes start.

[BAT](https://en.wikipedia.org/wiki/Batch_file) file of commands reloading the display (every morning as said) may contain

    @echo off
    title classes

    rem UPDATE
    start "" /I /B "C:\Dwimperl\perl\bin\perl.exe" C:\PANEL\classes.pl 1>>log.txt 2>>&1

    rem LOAD
    c:
    cd\panel\sendpanel
    keybuf.exe /zeeeey
    spectrum.exe
    time /t >>log.txt

`keybuf.exe` is a DOS keyboard buffer which simulates a key press. Colleague of mine wrote this. `eeeey` is a sequence of chars whose are send to `spectrum.exe` that actually reloads the text panel (the display) with just updated files.

![spectrum.exe](d/spectrum.png)

Files are `0730.325, 0915.325, 1100.325, 1245.325, 1430.325, 1615.325 and 1800.325`. You're right, these're classes start times. 325 is a proprietary file format used by the display. Now, where's the actual data, i.e. subjects, teachers, rooms and times? Luckily the [university information system](http://www.uis-info.com/en/index) we use has an [API](https://en.wikipedia.org/wiki/Web_API). You request a given URL with a room id as a parameter and get the room schedule for today. And because the world isn't perfect, it's not [JSON](https://developer.mozilla.org/en-US/docs/Glossary/JSON) but HTML. So we'll do some [web scraping](https://en.wikipedia.org/wiki/Web_scraping). The SPECTRUM text panel has of course it's own [charset table](https://en.wikipedia.org/wiki/ASCII#ASCII_printable_code_chart) to display czech chars and not exceed one byte. So we ought to remap non-ASCII chars and output bytes. So [pick the right tool for the job](http://c2.com/cgi/wiki?PickTheRightToolForTheJob). We'll do some [Perl](http://qntm.org/files/perl/perl.html)!

On GNU/Linux you're all set but on Windows [Perl has to be installed](http://dwimperl.com/windows.html) first. We also install the [`LWP::Simple`](https://metacpan.org/pod/LWP::Simple) module to send [HTTP requests](https://pretty-rfc.herokuapp.com/RFC2616#GET) and to keep it *simple* at the same time

    % cpan LWP::Simple

OK, the `classes.pl` script checklist

- A)
  - download schedule for given classrooms
  - extract data, purify and parse teacher's surname
- B)
  - store data in a right data structure (hash of hashes)
  - iterate over and prepare display screens
- C)
  - ensure the 325 format
  - encode (or replace) chars beyond ASCII
  - write files

First lines of [Perl](http://perltricks.com/learn) would go

    use strict;
    use warnings;
    use LWP::Simple;
    use File::Spec;
    #use Data::Dumper;
    use utf8;

    # CHANGELOG
    # v1, 08/2015, Your Name

    # TODO
    # —

Display's on a particular floor; 5 classrooms there. The chosen text format is

    /NOW/
    ROOM  SubjID SURNAME(of teachers)
    5x

    /NEXT/
    ROOM  SubjID SURNAME(of teachers)
    5x

So *start times* and *classrooms* are known

    ### ============== MAIN =============
    use constant API
        # live
        => 'http://REDACTED/?room='
        # debug
        # => 'http://localhost:8080/classes.html?'
    ;
    use constant CLASSES
        => qw( 7:30 9:15 11:00 12:45 14:30 16:15 18:00 19:45 )
    ;
    my %room = (
                # room id
        'JM 357'  => 752,
        'JM 359'  => 746,
        'JM 360'  => 2281,
        'JM 361'  => 790,
        'JM 382'  => 700,
    );

    print_panels(classes(values %room));
    ### ============ END ================

You might've noticed we add API URL and call subroutines "from the future". Well, here's `sub classes`

    =head2 classes

    get room schedules for today

     Returns : % class_ref
     Args    : @ rooms 

    =cut

    sub classes {
        my @rooms_id = @_;

        my %class;
        for my $room (@rooms_id) {
            # HTTP GET :iso-8859-2
            my $html = do {
                my $r;
                # 3 attempts to connect
                for (1..3) {
                    $r = get(API . $room);
                    last if $r;
                }

                $r;
            };
            die "(!) can't connect\n" unless $html;

            for my $_class (_html($html)) {
                my @subject = @{ $_class };

                my $from = shift @subject;
                my $to   = shift @subject;
                # min. column width
                my $subj = sprintf "%-6s " x @subject, @subject;
                
                # remove white space
                $subj =~ s/^\s+|\s+$//g;
                
                for my $time (CLASSES) {
                    my ($_from, $_to, $_time) = map {
                        # v5.14 just s/\D//gr
                        (my $num = $_) =~ s/\D//g;
                        $num;
                    } ($from, $to, $time);
                    
                    $class{$time}{$room} = $subj
                        if ($_time >= $_from and $_time < $_to);
                }
            }
        }

        return \%class;
    }

From there we call `sub _html` which extracts data from __HTML__

    <table>
    <thead>
    <tr>
    <th><small>From</small></th>
    <th><small>To</small></th>
    <th><small>Room</small></th>
    <th><small>SubjID</small></th>
    <th><small>Subject</small></th>
    <th><small>Teacher</small></th>
    </tr>
    </thead>
    <tr>
    <td><small>9:15</small></td>
    <td><small>10:45</small></td>
    <td><small>ZB 243 (ZB)</small></td>
    <td><small>4CS101</small></td>
    <td><small>Introduction to Computer Science</small></td>
    <td><small>Ph.D. John Doe, MBA</small></td>
    </tr>
    </table>

We can parse it with a little of [regex](https://www.cs.tut.fi/~jkorpela/perl/regexp.html)

    =head2 _html

    extract %From%, %SubjID% and %Teacher% surname
    from the HTML room schedule

     Returns : @ subjects
     Args    : text

    =cut

    sub _html {
        my $html = shift;

        # the room is empty for today
        return if $html !~ /<thead>/;

        my (@subjects, @tr);
        for (split m|</td>|, $html) {
            # chomp;
            if (m|</tr| && @tr > 5) {

                # second name of teachers
                my ($surname) = split(/,/, $tr[5], 2);
                ($surname) = $surname =~ /[ ]?(\S*)$/;
                
                # SubjID
                $tr[3] = 'RESERVED'
                  if $tr[4] =~ s/reservation:&nbsp;//i;

                push @subjects, [
                                $tr[0],   # From
                                $tr[1],   # To
                                $tr[3],   # SubjID
                                $surname, # Teacher
                              ];
                # ugly, but works
                @tr = ();
            }

            my ($td) = m|>([^<]+)</small>$|;
            # no space, tab or newline
            $td =~ s/^\s+|\s+$//g if $td;

            push @tr, $td || ' ';
        }

        # the room schedule
        return @subjects;
    }

The most important part of the script is done, i.e. mining the data. But there's still a fence to put down. We'll map the SPECTRUM charset table. This we know

            lower   upper
    A       97      65
    Á       160     128
    Ä       132     142
    B       98      66
    C       99      67
    Č       159     172
    D       100     68
    Ď       136     210
    E       101     69
    É       130     144
    Ě       216     183
    Ë       137     69
    ...

And this we want

    'A'  => "\x{41}",
    'Á'  => "\x{80}",
    'Ä'  => "\x{8E}",
    'B'  => "\x{42}",
    'C'  => "\x{43}",
    'Č'  => "\x{AC}",
    'D'  => "\x{44}",
    'Ď'  => "\x{D2}",
    'E'  => "\x{45}",
    'É'  => "\x{90}",
    'Ě'  => "\x{B7}",
    'Ë'  => "\x{45}",

Perl can print a specific char if you know its hexadecimal (byte) number. So translate upper chars to get lower ones too. To generate the encoded hash map keys

    use strict;
    use warnings;
    use v5.10.1;
    use open qw/:std :utf8/;

    # SYNOPSIS
    # declare a charset table stored in a Perl hash

    @ARGV = 'spectrum_charset.txt';

    while (<>) {
        chomp;
        s/^\s+//;

        # [0] CHAR
        # [1] LOWER decimal
        # [2] UPPER decimal
        my @chr_set = split /\s+/;

        if (@chr_set == 3) {
            # LOWER
            #my $hex = sprintf("%02X", $chr_set[1]);
            #say " " x 4, "'", lc $chr_set[0], "' =>  ", '"\x{' . $hex . '}",';

            # UPPER 
            my $hex = sprintf("%02X", $chr_set[2]);
            say " " x 4, qq('$chr_set[0]' =>  ), '"\x{' . $hex . '}",';
        }
    }

And the `sub encode` is born

    =head2 encode

    encode text to the SPECTRUM charset

     Returns : text
     Args    : text

    =cut

    sub encode {
        my $text = shift;

        # SPECTRUM text panel
        my %charset= (
            'A'  => "\x{41}",
            'Á'  => "\x{80}",
            'Ä'  => "\x{8E}",
            'B'  => "\x{42}",
            'C'  => "\x{43}",
            'Č'  => "\x{AC}",
            SKIPPED
            'ú'  => "\x{A3}",
            'ů'  => "\x{85}",
            'ü'  => "\x{81}",
            'v'  => "\x{76}",
            'w'  => "\x{77}",
            'x'  => "\x{78}",
            'y'  => "\x{79}",
            'ý'  => "\x{EC}",
            'z'  => "\x{7A}",
        );

        return join '', map {
              $charset{$_} || (ord $_ < 128 ? $_ : '?')
          } split //, $text
        ;
    }

Eventually the crucial `sub print_panels` comes

    =head2 print_panels

    write text panel screens to 325 files

    (!) all above 127 of ASCII has to encoded

     Args    : % class_ref

    =cut

    sub print_panels {
        my ($class_ref, @panel) = shift;

        for (my $i = 0; $i < (CLASSES); $i++) {
            my $time = (CLASSES)[$i];
            my @_panel = ' ' x 9 . "/starts ${time}h/";
            for my $room (sort keys %room) {
                my $subj = \$class_ref->{$time}{ $room{$room} };
                push @_panel, $room . '  ' . ( $$subj || 'OFF' );
            }

            push @panel, [ @_panel ];

            # panel has 2 subpanels now,
            # i.e. /NOW/ and /NEXT/
            if (@panel == 2) {

Now first text panel (screen) is gonna be written. If a classroom is empty we show "OFF". Filename is 4 chars long (as we've agreed on) and file path points to `C:\PANEL\TEXTS\{filename}.325`

                FILE:
                {
                    (my $file = (CLASSES)[$i-$#panel]) =~ s/\D//g;
                    $file = sprintf "%04s", $file;

                    my $path = File::Spec->catpath(
                        'C:',
                        File::Spec->catdir('', 'PANEL', 'TEXTS'),
                        $file . '.325'
                    );

                    open(my $fh, '>', $path)
                        or die "(!) cannot write to '$path'\n";

We [output bytes](http://perldoc.perl.org/PerlIO.html) because this is a data file

                    binmode $fh, ':bytes';

The 325 file format has its specifics. First row is metadata about the text animation (2 digits). Next 2 digits is a delay between screens

                    print $fh
                        '33', # 3/3 text show on/off animation
                        '10', # stay for 10 sec
                        "\n"
                    ;

As you see below, char code for a month or a second is very very specific

                    #====== SCREEN ======

                    # heading
                    print $fh
                        # day   # month
                        "%d. \x{BC}\x{BD}.",
                        ' ' x 2,
                        encode('VŠE v Praze'),
                        ' ' x 2,  # seconds
                        "%H:%M:\x{B8}\x{B9}",
                        "\n"
                    ;

This prints/saves classroom schedules

                    # body ~ 14 rows
                    print $fh
                        encode(
                            join "\n", map {
                                # $_ // ''
                                defined $_ ? $_ : ''
                            } (map @$_, @panel[0..$#panel])[0..13]
                        ),
                        "\n"
                    ;

                    #====== END ======

Matrix at the end of a file stores control chars for each row. Number of lines — of matrix and actual content — must equal. The code above always outputs 14 lines no matter what. __NEXT__ subpanel of this hour changes to __NOW__ subpanel of next hour. And obviously the very last file has no NEXT subpanel, so we redo our named code block

                    # control chars ~ 15 rows
                    print $fh
                        "\x{0F}" x 30, "\n" for (1..15)
                    ;

                    close $fh;

                    # NEXT => NOW
                    shift @panel;

                    # the very last schedule of today
                    redo FILE if $i + @panel == (CLASSES);
                }
            }
        }

        print "(i) written\n";
    }

Done.

![classes.pl does room schedules](d/display2.jpg)

Last thing, we set times on each file as when to show off

![spectrum.exe](d/spectrum2.png)

For the source code, just [let me know](http://paveljurca.com).

p.s. The sub `print_panels` is a mess. It should rather go with one sub to create panels and one to print or save them. At least we've experienced Perl named blocks.


