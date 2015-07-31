MVCS Antipatterns
=================

In simple terms, Model-View-Controller-Services add a few more layers to the
MVC pattern. The main one is the service, which owns all the core business
logic and manipulate the repository layer.

Creating entities for association tables
----------------------------------------

You'll often need association tables, for instance to set up a many to many
relationships between users and their toasters. Let's assume that a toaster can
be owned by multiple users.

It might be tempting to create a `UserToaster` entity for this relationship,
especially if this relationship has some complex attributes associated with
(for instance, since when the toaster is owned by the user).

Let me pull a few quotes from the [Domain Driven
Design](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215) by Eric Evans:

> Design a portion of the software system to reflect the domain model in a very
> literal way, so that mapping is obvious.

> Object-oriented programming is powerful because it is based on a modeling
> paradigm, and it provides implementations of the model constructs. As far as
> the programmer is concerned, objects really exist in memory, they have
> associations with other objects, they are organized into classes, and they
> provide behavior available by messaging.

> Does an object represent something with continuity and identity— something
> that is tracked through different states or even across different
> implementations? Or is it an attribute that describes the state of something
> else? This is the basic distinction between an ENTITY and a VALUE OBJECT.
> Defining objects that clearly follow one pattern or the other makes the
> objects less ambiguous and lays out the path toward specific choices for
> robust design.

Evans, Eric (2003-08-22). Domain-Driven Design: Tackling Complexity in the
Heart of Software. Pearson Education. Kindle Edition.

In that case, the `UserToaster` does not map to anything the business is
using. Using plain English, somebody might ask about "what toasters does user
A owns?" or "who owns toaster B and since when?" Nobody would ask "give me the
UserToaster for user A".

So in that case, I would recommend doing the following:

* Create a `User` and `Toaster` entity.
* Put the association properties on the entity that makes sense, for instance
  `owned_since` would be on `Toaster`.
* If filtering on association properties is required, put this logic in
  repositories.

Note that Domain Driver Design recommends avoiding many-to-many relationships
altogether:

> In real life, there are lots of many-to-many associations, and a great number
> are naturally bidirectional. The same tends to be true of early forms of
> a model as we brainstorm and explore the domain. But these general
> associations complicate implementation and maintenance. Furthermore, they
> communicate very little about the nature of the relationship.

> There are at least three ways of making associations more tractable.

> 1. Imposing a traversal direction
> 2. Adding a qualifier, effectively reducing multiplicity
> 3. Eliminating nonessential associations

Evans, Eric (2003-08-22). Domain-Driven Design: Tackling Complexity in the
Heart of Software (Kindle Locations 1642-1647). Pearson Education. Kindle
Edition.