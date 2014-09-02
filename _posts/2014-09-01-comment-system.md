---
layout: post
title: Comment system
---

By default, GitHub Pages do not have a commenting system, being just
static sites.

Many Jekyll-based blogs use Disqus[^1]. The idea is that each post
embeds an `iframe` that fetches comments from an external site and
also provides a comment form. When that form is submitted, the new
comment goes again to that external site which stores it.

[^1]: Intentionally not giving a link.

However, on several sites that use Disqus, I have seen a feature that
I very much dislike. They call it Promoted Discovery. Near the comment
list, they show a selection of a few posts of your own that are
somehow related to the current, and a selection of a few pages by
other users. What drives Promoted Discovery is apparently how much the
promotee paid to Disqus, and some percentage of that payment goes to
the users on whose blogs these ads are displayed.

Excuse me while I go compose an [AdBlock Plus][] element hiding rule
for that abomination.

[AdBlock Plus]: https://adblockplus.org/en/firefox

```
disqus.com###discovery
```

OK, that was easy. And they actually do not just allow anyone into the
program; it’s invite-only. Which means I’m initially ineligible and
will have the chance to reject their offer if and when they make it.
But still, I do not want to support and endorse a system that does
that to its users.

----

So what do I want from a commenting system?

* **It shall not force any advertisement on me or my readers.**
  Preferably, it shall have no advertising program at all. (Yes, I
  understand they need to make money. Offer paid hosting. Offer
  advanced features. Offer support. But do not bribe users to display
  ads.)
* **It shall not require registration from commenters.** Commenters
  should be able to log in with their Google, Facebook, Twitter or
  OpenID accounts. Preferably, that should be the only UI displayed
  (no account creation dialog after first login).
* **It shall authenticate commenters.** I don’t want to hear from
  anonyms, and I don’t want commenters to be able to spoof each other.
* **It shall have notifications.** Both for me when someone comments
  on my post, and for the commenters when I or someone else replies to
  them. Preferably, notifications should be by email.
* **It shall support Markdown.** I use Markdown for my blog and expect
  to reply to commenters in the same format. Preferably, it should
  support the same dialect of Markdown as Jekyll does.
* **It shall not lock me in.** At any time I should be able to export
  all comments posted in my blog and to re-import them into another
  system of my choice.
* **It shall be Free.** As in speech, according to Stallman’s Four
  Freedoms. I should be able to deploy my own instance, if need
  arises, and to modify it as I see fit.
* **It shall not bring cruft in.** I don’t want to increase my
  dependence on certain technologies, including, but not limited to:
  PHP, Perl and MySQL. Ubuntu packages are preferable to PyPI or Cabal
  packages or Ruby gems.

----

I have investigated a few options and compiled the following table:

|Name                |Ads |Login            |Account|Auth|Notify    |Markup      |Export |Free |Deps|
|--------------------|----|-----------------|-------|----|----------|------------|-------|-----|----|
|IntenseDebate       |no? |Wp, Fb, Tw       |?      |yes |email, rss|HTML        |Wp     |no   |    |
|[Discourse][]       |no  |G, Fb, Tw, Y!, Gh|req    |yes |email     |Md, Bb, HTML|yes    |GPL2 |RoR, Pg, Redis|
|Muut                |self|G, Fb            |req    |yes |email     |Md-like     |planned|no   |    |
|[Commentary][]      |    |                 |       |    |          |            |       |     |Ruby|
|[Juvia][]           |no  |no               |no     |no  |email?    |Md          |no     |AGPL |RoR |
|[talaria][]         |no  |Gh               |no     |yes |Gh        |Md          |no?    |MIT  |Gh  |
|[Isso][]            |no  |no               |no     |no  |no?       |Md          |db     |MIT  |Py, SQLite|
|[Open Comment Box][]|    |                 |       |    |          |            |       |     |    |
|[Talkatv][]         |no  |O                |       |yes?|          |            |       |AGPL3|Py  |
|[HashOver][]        |no  |                 |no     |no  |email     |HTML        |       |AGPL |PHP |

[Discourse]: http://www.discourse.org/
[Commentary]: https://github.com/sdqali/commentary
[Juvia]: https://github.com/phusion/juvia
[talaria]: https://github.com/m2w/talaria
[Isso]: http://posativ.org/isso/
[Open Comment Box]: https://github.com/arunoda/open-comment-box
[Talkatv]: https://github.com/talkatv/talkatv
[HashOver]: http://tildehash.com/?page=hashover

And the only viable solution seems to be Discourse. It has a minor
annoyance that it displays an account creation dialog after logging in
with Google (and probably other social services), but I expect this
can be removed easily.