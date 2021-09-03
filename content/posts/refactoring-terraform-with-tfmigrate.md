---
title: "Refactoring Terraform with tfmigrate"
date: 2021-09-03T12:32:39-07:00
tags: ["terraform"]
draft: false
---

# So you want to refactor your terraform

You have some terraform. It was written in haste and you have many regrets. You
would like to refactor but you worry about the state transition. If you move a
resource under a different path it will not match and will require painful
destroy / create cycles.

# tfmigrate

The solution is [tfmigrate](https://github.com/minamijoyo/tfmigrate). It allows
you to write clear migrations for your state and lets you test your migrations
without worrying about clobbering your existing state.

Here is an example of a migration:

{{< highlight hcl >}}
migration "state" "test" {
    actions [
        "mv 'foo' 'bar'",
        "mv 'baz' 'quux'"
    ]
}
{{< / highlight >}}
