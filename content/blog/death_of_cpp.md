+++
title = "If C++ dies, it will not be caused by the language itself"
date = "2024-11-20T18:36:52+10:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["programming-languages", "c++"]
+++

In this blog post, I'm making a prediction: that if C++ dies, its death will not be caused by the language
itself. It will be caused by the _management_ of the language, particularly the committee.

## On the quality of C++
_Much_ has been written about the quality, or lack thereof, of C++ as a programming language. Even beyond the
more recent criticism that C++ gets for its lack of memory safety, it's widely been known to be a poorly
designed, cumbersome and extremely complex language for decades.

My personal opinion basically boils down to this: I agree with a lot of the criticism the language receives,
and I do think it gets a _lot_ wrong. That being said, I don't think that C++ is an irredeemably bad language,
is useful in quite a few applications, and does in fact get some things right. It's the type of situation
where the old Churchill quote applies, "C++ is the worst language, except for all the others" - however, that
is less and less true nowadays with the introduction of newer and better languages.

Despite it already being written about ad-nauseum, let's go over my problems with modern C++. In no particular
order:
- C++20's UTF-8 support is literally [completely broken](https://stackoverflow.com/a/65468019/5007892) and
[entirely unusable](https://stackoverflow.com/questions/55556200/convert-between-stdu8string-and-stdstring).
It's a joke of a feature, and it boggles the mind why something so poorly designed was shipped.
- the 2d graphics api thing? todo
- Why is there a [294 page book](https://www.amazon.com.au/dp/B0BW38DDBK?ref_=mr_referred_us_au_au) on
initialisation?
- On the above, it's been well written that the language is riddled with foot-guns
