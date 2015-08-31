# NAME

Yaadgom - Yet Another Automatic Document Generator (On Markdown)

# SYNOPSIS

    use Yaadgom;

    # create an instance
    my $foo = Yaadgom->new;

    # call this methoed each request you want to document
    $foo->process_response(
        folder => 'test', # what 'folder' or 'category' this is
        weight => 1     , # default order
        req    => HTTP::Request->new ( GET => 'http://www.example.com/foobar' ),
        res    => HTTP::Response->new( 200, 'OK', [ 'content-type' => 'application/json' ], '{"ok":1}' ),
    );

    # iterate over processed document, for each file.
    # NOTE: This does not write to any file.
    $foo->map_results(
        sub {
            my (%info) = @_;

            is( $info{file},   'foobar', '"foobar" file' );
            is( $info{folder}, 'test',   '"test" folder' );
            ok( $info{str}, 'has str' );
        }
    );

# DESCRIPTION

Yaadgom helps you document your requests (to create an API Reference or something like that).

Yaadgom output string in markdown format, so you can use those generated files on http://daux.io or other tools.

# METHODS

## new

    Yaadgom->new(
        file_name => "$0"
    );

## process\_response

    $self->process_response(
        folder => 'General',
        weight => -150, # set as "first" thing on document
        req    => HTTP::Request->new ( GET => 'http://www.example.com/login' ),
        res    => HTTP::Response->new( 200, 'OK', [ 'content-type' => 'application/json' ], '{"has_password":1}' ),
        extra => {
             fields => { has_password => ['the user has password', 'but can came from facebook']},
             you_can_extend_using => { 'Class_Trigger' => 'to process something else' }
        }
    );

# Class::Trigger names

On each trigger, the returning is used as the new version of the input. Except for process\_extras, where all returnings are concatenated.

Updated @ Stash-REST 0.01

    $ grep  '$self_0_01->call_trigger' lib/Yaadgom.pm  | perl -ne '$_ =~ s/^\s+//; $_ =~ s/self-/self0_01-/; print' | sort | uniq

Trigger / variables:

    $self0_01->call_trigger( 'filename_generated', { req => $req, file => $file } );
    $self0_01->call_trigger( 'format_title', { header => $desc } );
    $self0_01->call_trigger( 'format_response_body', { response_str => $body } );
    $self0_01->call_trigger( 'format_before_extras', { str => $str } );
    $self0_01->call_trigger( 'format_after_extras', { str => $str } );
    $self0_01->call_trigger( 'process_extras', %opt );
    $self0_01->call_trigger( 'format_generated_str', { str => $format_time } );

# AUTHOR

Renato CRON <rentocron@cpan.org>

# COPYRIGHT

Copyright 2015- Renato CRON

Thanks to http://eokoe.com

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO

[Shodo](https://metacpan.org/pod/Shodo)

# SEE OTHER

[Stash::REST](https://metacpan.org/pod/Stash::REST), [Class::Trigger](https://metacpan.org/pod/Class::Trigger)
