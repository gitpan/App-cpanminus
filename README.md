# NAME

cpanminus - get, unpack, build and install modules from CPAN

# SYNOPSIS

    cpanm Module
    cpanm -f Module
    cpanm --notest Module
    cpanm --sudo Module
    cpanm .

# DESCRIPTION

cpanminus is a script to get, unpack, build and install modules from CPAN.

Its catch? Deps-free, zero-conf, standalone ~200 LOC script
(i.e. hackable) and requires only 10MB RAM. See below for its cons.

# INSTALLATION

Grab the file from <http://bit.ly/cpanm>, chmod +x, put it into your
PATH. Adjust shebang if needed.

# DEPENDENCIES

perl 5.8 or later (Actually I believe it works with pre 5.8 too but
haven't tested).

- *

LWP or 'wget' to get files over HTTP.

- *

'tar' executable or Archive::Tar to unpack files.

- *

C compiler, if you want to build XS modules.

And optionally:

- *

make, if you want to more reliably install MakeMaker based modules

- *

Module::Build (core in 5.10) if you want to install MakeMaker based modules without 'make'

# QUESTIONS

## Should I really use this?

No. Use CPAN or CPANPLUS.

## WTF? What's the point?

OK, the first motivation was this: CPAN shell gets OOM (or swaps
heavily and gets really slow) on Slicehost/linode's most affordable
plan with only 256MB RAM. Should I pay more to install perl modules
from CPAN? I don't think so.

Yes, I know there are tools like CPAN::SQLite that can fix that
problem (and yes I use it on my Macbook Pro!) but installing it and
its 14 non-core dependencies without CPAN shell (because CPAN shell
doesn't work) feels like yak shaving.

So, imagine you don't have CPAN or CPANPLUS. What you're going to do
is to search the module on the CPAN search site, download a tarball,
unpack it and then run `perl Makefile.PL` (or `perl Build.PL`) and
then `make install` (or `./Build install`). If the module has
dependencies you probably have to recurively resolve those
dependencies by hand before doing so.

This script just automates that.

## Zero-conf? How does this module get/parse/update the CPAN index?

It scrapes the site <http://search.cpan.org/>. Yes, it's horrible and
fragile.

Fetched files are unpacked in `~/.cpanm` but you can configure with
`CPANMINUS_HOME` environment variable.

## Are you on drugs?

Yes, I think my brain has been damaged since I looked at PyPI, gemcutter, pip and rip.

## Where does this install modules to?

It installs to wherever ExtUtils::MakeMaker and Module::Build are
configured to (i.e. via `PERL_MM_OPT` and `MODULEBUILDRC`). So if
you use local::lib then it installs to your local perl5
directory. Otherwise it installs to siteperl directory, so you might
need to run `cpanm` command as root, or run with `--sudo` option to
auto sudo when running the install command.

## Does this really work?

I tested installing MojoMojo, KiokuDB, Catalyst, Jifty and Plack using
cpanminus and the installations including dependencies were mostly
successful. So multiplies of _half of CPAN_ behave really nicely and
appear to work.

However, there are some distributions that will miserably fail,
because of the nasty edge cases (funky archive formats, naughty
tarball that extracts to the current directory, META.yml that is
outdatd, META.yml that is not really YAML, circular dependencies etc.)
while CPAN and CPANPLUS can handle them.

## Quick Install?

    env PERL_MM_USE_DEFAULT=1 cpanm --notest Module

Oh, don't do that. It's too useful.

## So you're ignoring the CPAN toolchain ecosystem with this?

No, that's not my intention. This tiny script actually respects and
plays nice with all the toolchain ecosystem that has been developed
for years, such as: [Module::Build](http://search.cpan.org/search?mode=module&query=Module::Build), [Module::Install](http://search.cpan.org/search?mode=module&query=Module::Install),
[ExtUtils::MakeMaker](http://search.cpan.org/search?mode=module&query=ExtUtils::MakeMaker) and [local::lib](http://search.cpan.org/search?mode=module&query=local::lib). It just provides an
alternative to (but __NOT__ a replacement for) [CPAN](http://search.cpan.org/search?mode=module&query=CPAN) or [CPANPLUS](http://search.cpan.org/search?mode=module&query=CPANPLUS),
so that it doesn't require any configuration, any dependencies and has
no bootstrap problems.

The thing is, I've learned that often for less experienced users, or
even really experienced users who knows how to shoot in their feet,
setting up a CPAN toolchain _in the right way_ feels like just
another yak to shave when all they want to do is just to quickstart
writing perl code by installing CPAN modules. cpanminus is a super
tiny shaver to eliminate the big yak really quickly in that case, and
does nothing else.

## Should I really use this?

No. Use CPAN or CPANPLUS.

# COPYRIGHT

Copyright 2010- Tatsuhiko Miyagawa

[Parse::CPAN::Meta](http://search.cpan.org/search?mode=module&query=Parse::CPAN::Meta), included in this script, is Copyright 2006-2009 Adam Kennedy

# LICENSE

Same as Perl.

# AUTHORS

Tatsuhiko Miyagawa, Goro Fuji, Kazuhiro Osawa, Tokuhiro Matsuno, Ken-ichi Ishigaki, Ian Wells

# SEE ALSO

[CPAN](http://search.cpan.org/search?mode=module&query=CPAN) [CPANPLUS](http://search.cpan.org/search?mode=module&query=CPANPLUS)