---
layout: post
title:  "Go Gotcha"
date:   2016-03-01 10:14:00
categories: golang
---

I wandered into the trap today of go being lexically scoped today...

What is wrong with the following code?
[code language="go"]// Item
var item *Item

itemID := u.Query().Get("itemID")

if itemID != "" {
item, err := repository.GetItem(itemID)
}

log.Printf("The item %v", item)
[/code]
After reading the following:

<a href="https://golang.org/ref/spec#Declarations_and_scope">https://golang.org/ref/spec#Declarations_and_scope</a>

I find out that I am declaring item again in the if block.  Oooops.
