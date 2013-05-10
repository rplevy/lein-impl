lein-impl
=========

A lein plugin to verify API structure based on ns metadata.

# Motivation

There are various strategies for declaring code as API versus code that is not intended to be called
by its user. The most common strategy is to declare some functions as private and others as public.
The compiler ensures that private functions are not invoked outside the namespace in which they are
defined (although the var can be accessed directly as a way around this). This is often considered a
sufficient mechanism for declaring API.

Another approach, which was either originated by or popularized by the book The Joy of Clojure by
@fogus and @chouser, is to place non-API functions into a supporting namespace. For example, if the
ns is called some.things, the supporting functions should live in some.things.impl. These functions
need not be declared private, yet the ns naming convention makes explicit the notion that they are
non-API. This makes it easier for example to indicate that a public macro's expansion includes non-
API references. It also cleans up and shortens files and improves modularity.

The impl ns convention lacks the native automatic checking the private functions provide, but is more
flexible and expressive. A further example of the expressive power of impl namespaces however reveals
a limitation. Given the convention that some.things.impl is only to be used by some.things, and also
that some.things.impl.junk is only to be used by some.things, we soon find ourselves using this power
to specify more deeply nested hierarchies of internal API structure and are faced with the problem that
having multiple impl namespaces becomes unwieldly.

To address this problem, it makes sense to mark namespaces with metadata instead of having a special
naming convention and corresponding project structure. We can also use this metadata to check that
the vars from an `^:impl` ns are only used within the ns that they support. That is the purpose that
the lein-impl plugin serves. Similarly, libraries that generate documentation from code could be made
aware of this convention, so as to distinguish between API and other supporting code.

# Setup

Declare impl namespaces by annotating the ns definition with `^:impl`.

Add `[TODO]` to plugins vector in project.clj.

# Usage

Call `lein impl` within the project. This is recommended to be called as part of a build process along
with any other helpful tools such as eastwood, kibit, and slamhound.

# License

EPL, same as Clojure.

Copyright 2013 @rplevy
