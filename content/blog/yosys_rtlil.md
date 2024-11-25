+++
title = "The expert's guide to doing shit with Yosys RTLIL"
date = "2024-11-17T16:02:36+10:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["computer-engineering", "eda"]
+++

## Introduction
We ✨ _love_ ✨ [Yosys](https://github.com/YosysHQ/yosys)! It's a fantastic project, with a great community,
that remains the _only_ open-source, research-grade EDA synthesis tool. Its existence has enabled the
production of real, live ASICs designed using entirely open-source tools - something that was probably
considered a pipe dream a mere 10-15 years ago.

As someone writing [their thesis](/projects#tamara) as a Yosys plugin, I deal a lot with _RTLIL_ ("RTL
Intermediate Language"). This is Yosys' internal representation of RTL; think LLVM IR but for EDA.
