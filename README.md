# perl-data-validate-email
NAME
    Data::Validate::Email - common email validation methods

SYNOPSIS
      use Data::Validate::Email qw(is_email is_email_rfc822);
  
      if(is_email($suspect)){
            print "Looks like an email address\n";
      } elsif(is_email_rfc822($suspect)){
            print "Doesn't much look like an email address, but passes rfc822\n";
      } else {
            print "Not an email address\n";
      }

      # or as an object
      my $v = Data::Validate::Email->new();
  
      die "not an email" unless ($v->is_email('foo'));

DESCRIPTION
    This module collects common email validation routines to make input
    validation, and untainting easier and more readable.

    All functions return an untainted value if the test passes, and undef if
    it fails. This means that you should always check for a defined status
    explicitly. Don't assume the return will be true. (e.g.
    is_username('0'))

    The value to test is always the first (and often only) argument.

FUNCTIONS
    new - constructor for OO usage
          new([\%opts]);

        *Description*
            Returns a Data::Validator::Email object. This lets you access
            all the validator function calls as methods without importing
            them into your namespace or using the clumsy
            Data::Validate::Email::function_name() format.

        *Arguments*
            An optional hash reference is retained and passed on to other
            function calls in the Data::Validate module series. This module
            does not utilize the extra data, but some child calls do. See
            Data::Validate::Domain for an example.

        *Returns*
            Returns a Data::Validate::Email object

    is_email - is the value a well-formed email address?
          is_email($value);

        *Description*
            Returns the untainted address if the test value appears to be a
            well-formed email address. This method tries to match real-world
            addresses, rather than trying to support everything that rfc822
            allows. (see is_email_rfc822 if you want the more permissive
            behavior.)

            In short, it pretty much looks for something@something.tld. It
            does not understand real names ("bob smith" <bsmith@test.com>),
            or other comments. It will not accept partially-qualified
            addresses ('bob', or 'bob@machine')

        *Arguments*

            $value
                The potential address to test.

        *Returns*
            Returns the untainted address on success, undef on failure.

        *Notes, Exceptions, & Bugs*
            This function does not make any attempt to check whether an
            address is genuinely deliverable. It only looks to see that the
            format is email-like.

            The function accepts an optional hash reference as a second
            argument to change the validation behavior. It is passed on
            unchanged to Neil Neely's Data::Validate::Domain::is_domain()
            function. See that module's documentation for legal values.

    is_email_rfc822 - does the value look like an RFC 822 address?
          is_email_rfc822($value);

        *Description*
            Returns the untainted address if the test value appears to be a
            well-formed email address according to RFC822. Note that the
            standard allows for a wide variety of address formats, including
            ones with real names and comments.

            In most cases you probably want to use is_email() instead. This
            one will accept things that you probably aren't expecting
            ('foo@bar', for example.)

        *Arguments*

            $value
                The potential address to test.

        *Returns*
            Returns the untainted address on success, undef on failure.

        *Notes, Exceptions, & Bugs*
            This check uses Casey West's Email::Address module to do its
            validation.

            The function does not make any attempt to check whether an
            address is genuinely deliverable. It only looks to see that the
            format is email-like.

    is_domain - does the value look like a domain name?
          is_domain($value);

        *Description*
            Returns the untainted domain if the test value appears to be a
            well-formed domain name. This test uses the same logic as
            is_email(), rather than the somewhat more permissive pattern
            specified by RFC822.

        *Arguments*

            $value
                The potential domain to test.

        *Returns*
            Returns the untainted domain on success, undef on failure.

        *Notes, Exceptions, & Bugs*
            The function does not make any attempt to check whether a domain
            is actually exists. It only looks to see that the format is
            appropriate.

            As of version 0.03, this is a direct pass-through to Neil
            Neely's Data::Validate::Domain::is_domain() function.

            The function accepts an optional hash reference as a second
            argument to change the validation behavior. It is passed on
            unchanged to Neil Neely's Data::Validate::Domain::is_domain()
            function. See that module's documentation for legal values.

    is_username - does the value look like a username?
          is_username($value);

        *Description*
            Returns the untainted username if the test value appears to be a
            well-formed username. More specifically, it tests to see if the
            value is legal as the username component of an email address as
            defined by is_email(). Note that this definition is more
            restrictive than the one in RFC822.

        *Arguments*

            $value
                The potential username to test.

        *Returns*
            Returns the untainted username on success, undef on failure.

        *Notes, Exceptions, & Bugs*
            The function does not make any attempt to check whether a
            username actually exists on your system. It only looks to see
            that the format is appropriate.

AUTHOR
    Richard Sonnen <sonnen@richardsonnen.com>.

COPYRIGHT
    Copyright (c) 2004 Richard Sonnen. All rights reserved.

    This program is free software; you can redistribute it and/or modify it
    under the same terms as Perl itself.

