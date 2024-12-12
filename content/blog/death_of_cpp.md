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

## C++'s successor language(s)
Many languages have claimed to be the successor and killer of C++. Originally, it was Java. Some time later,
people thought it was D - which is actually a really nice language and chronically underrated in my opinion.
While D is compiled and Java is JIT, both languages share the common trait of having a garbage collector. It
wasn't until very recently, with the introduction of Rust, that memory safety could be assured at compiled
time with near zero runtime overhead [^1].

Personally, as someone who writes a lot of C++ at work and in their free time, I _ignored_ the calls that C++
was, or was becoming, a legacy language. When my favourite shell [fish](https://fishshell.com/) decided to
RIIR, I remember thinking "Neat, good for them." While I thought it wasn't a bad idea, I remember chuckling
when I read [their reasoning](https://github.com/fish-shell/fish-shell/blob/master/doc_internal/fish-riir-plan.md),
which included:

> C++ is becoming a legacy language.

"Yeah right!" I thought. "There's so much C++ around, Rust still has it's issues and isn't widely utilised in
industry, plus C++ is always evolving. They can't be serious."

Sigh. If only I could have known how wrong I was.

This is 2024, and Rust has been adopted by the big players including Microsoft and Google. RIIR is largely
pitched from a security POV, and sometimes a packaging issue as well, given C++'s complete and utter lack of a
sensible and coherent package management situation (yes, CMake and conan/vcpkg are nice, but no they don't
count [^2]).

I think what should be added to this list is: Why maintain an existing project, or even worse start a new
project, in a completely dead-end language? Given their absolute refusal to change the ABI or introduce any
semblance of modern safety features demanded by government and industry, the C++ committee have proudly
announced to the world that C++ is a legacy, dead-end language. As one Hacker News commenter put it recently,
the ISO committee has apparently locked in their plan to keep the C++ ABI identical to the heat death of the
universe. Given that's the case, who would want to start a new project in a dead-end language? Who would want
to maintain an existing project in it? It would seem that the best thing for teams to do is migrate (as time
permits) to a modern language, whose designers actually promise to support and improve it.

Funnily enough, and extremely ironically, the statement "C++ is becoming a legacy language" didn't come from
the C++ haters, it came from the language committee itself.

&nbsp;

I used to, and still kind of do, think that Rust makes it a bit hard to do something "quick and dirty" as a
test; and that this was an advantage that C++ had over Rust. This was covered well in [this legendary blog
post](https://loglog.games/blog/leaving-rust-gamedev/). However, while C++ allows you to do things quickly and
hacky, you have to wonder about the drawbacks of this. Combined with the overwhelming number of footguns that
can be hard to spot even for experienced developers, I feel like a lot of this "test code" has made it to
prod. And also, given how cumbersome managing dependencies in C++ projects is, one has to wonder if merely
using Cargo negates all the speed benefits of hacking things up in C++.


[^1]: I am not a Rust expert, but my understanding is that `Rc<>` for example adds some miniscule runtime
overhead due to ref-counting. Nevertheless, Rust often beats C and C++ in many speed benchmarks.

[^2]: CMake is a _de facto_ standard and while nice, cannot be compared to Cargo. Conan/vcpkg are also nice,
but widely underutilised.
