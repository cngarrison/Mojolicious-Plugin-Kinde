# NAME

Mojolicious::Plugin::Kinde - A Mojo helper and route condition to extract Kinde auth header, verify JWT token, and return the claims

# VERSION

version v0.0.1

# SYNOPSIS

    package MyApp;
    use Mojo::Base 'Mojolicious';

    sub startup {
            my $self = shift;

            $self->plugin( 'Mojolicious::Plugin::Kinde' ); # config can also be supplied here

            $self->routes->get('/api')->requires( kinde_auth => {} );

    }
    
    ...
    
    # myapp_mojo.pl (config)

    {
            kinde => {
                    jwks_url => 'https://your-domain.kinde.com/.well-known/jwks.json',
                    iss      => 'https://your-domain.kinde.com',
            },
    }
    
    ...
    
    my $claims = $c->get_kinde_claims

# DESCRIPTION

Mojolicious::Plugin::Kinde creates a helper method and a route condition. Both retrieve the JWT 
token from the `Authorization` header, verify the JWT, and do some sanity checks on the claims. 
The `get_kinde_claims` helper will return the claims extracted from the token.

The sanity checks include:

- confirm `iss` claim matches the value from `kinde` config
- confirm the `alg` is `RS256`
- confirm the `aud` array contains the value from `kinde` config (if not empty)

# HELPERS

## get\_kinde\_claims()

Verifies the JWT from `Authorization` header and returns the extracted claims.

# ROUTE CONDITIONS

## kinde\_auth

Verifies the JWT from `Authorization` header and returns boolean for the route condition.

# METHODS

Mojolicious::Plugin::Kinde inherits all methods from Mojolicious::Plugin and implements the following new ones.

## register

    $plugin->register(Mojolicious->new);

Register conditions in Mojolicious application.

# SEE ALSO

- [Mojo::JWT](https://metacpan.org/pod/Mojo%3A%3AJWT)
- [Kinde](https://kinde.com/) fantastic auth service

# SUPPORT

## Bugs / Feature Requests

Please report any bugs or feature requests through the issue tracker at
https://github.com/cngarrison/Mojolicious-Plugin-Kinde/issues. You will
be notified automatically of any progress on your issue.

## Source Code

This is open source software. The code repository is available for
public review and contribution under the terms of the license.

https://github.com/cngarrison/Mojolicious-Plugin-Kinde

    git clone git://github.com/cngarrison/Mojolicious-Plugin-Kinde.git

# AUTHOR

Charlie Garrison <cng@cngarrison.com>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2023 by Charlie Garrison.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
