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

# Tests Coverage

I'm always trying to improve those numbers.
Improve branch number is a very time-consuming task. There is a room for test all checking and defaults on tests.

    @ version 0.05
    ---------------------------- ------ ------ ------ ------ ------ ------ ------
    File                           stmt   bran   cond    sub    pod   time  total
    ---------------------------- ------ ------ ------ ------ ------ ------ ------
    blib/lib/Stash/REST.pm         96.4   74.5   68.4  100.0  100.0  100.0   86.7
    Total                          96.4   74.5   68.4  100.0  100.0  100.0   86.7
    ---------------------------- ------ ------ ------ ------ ------ ------ ------

# Class::Trigger names

Updated @ Stash-REST 0.05

    $ grep  '$self0_05->call_trigger' lib/Stash/REST.pm  | perl -ne '$_ =~ s/^\s+//; $_ =~ s/self-/self0_04-/; print' | sort | uniq

Trigger / variables:

    $self0_05->call_trigger( 'after_stash_ctx', { stash => $staname, results => \@ret });
    $self0_05->call_trigger( 'before_rest_delete', { url => $url, conf => \%conf } );
    $self0_05->call_trigger( 'before_rest_get', { url => $url, conf => \%conf } );
    $self0_05->call_trigger( 'before_rest_head', { url => $url, conf => \%conf } );

# AUTHOR

Renato CRON <rentocron@cpan.org>

# COPYRIGHT

Copyright 2015- Renato CRON

Thanks to http://eokoe.com

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO

[Stash::REST::TestMore](https://metacpan.org/pod/Stash::REST::TestMore), [Class::Trigger](https://metacpan.org/pod/Class::Trigger)
