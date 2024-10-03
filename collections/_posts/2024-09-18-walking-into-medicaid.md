---
layout: post
title: "API Magic with Zoho Deluge: A Hands-On Guide for Data Science Enthusiasts"
date: 2024-09-18T09:49:03Z
authors: ["Andy Garcia"]
categories: ["Zoho Deluge", "REST API"]
description: This blog post will walk you through using external APIs within Zoho Deluge. You'll learn what Zoho Deluge is, how to leverage external APIs effectively, and see a real-world data science example in action. Additionally, the guide will cover best practices for storing and managing the data obtained from these APIs.
thumbnail: "/assets/images/gen/blog/blog-21-thumbnail.webp"
image: "/assets/images/gen/blog/blog-21.webp"
comments: false

meta_title: "API Magic with Zoho Deluge: A Hands-On Guide for Data Science Enthusiasts"
meta_description: This blog post will walk you through using external APIs within Zoho Deluge. You'll learn what Zoho Deluge is, how to leverage external APIs effectively, and see a real-world data science example in action. Additionally, the guide will cover best practices for storing and managing the data obtained from these APIs.
meta_image: "/assets/images/og/og-twitter-blog-image.webp"
---

## Introduction to Zoho Deluge and Its Relevance for Data Science

Picture this: You're at your desk working on a lead when your boss asks for a report that pulls data from multiple sources. You consider using Python or C#, but it could take days—and you're not confident in those languages.

That's where a CRM's proprietarys scripting language shines. Tools like Salesforce's Apex or Zoho's Deluge are built to steamline CRM tasks by making it easy to create functions, automate workflows, and generate reports—no deep programming skills required. Using a CRM's proprietary programming language is like having a specialized toolkit with high-performance tools crafted for your car model. It guarantees smoother, more powerful rides, maximizing coding efficiency and precision modifications compared to generic tools like Python or C#.

There's a lot you can do with these proprietary languages and there a designated web pages full of documentation that I cannot fully encapuslate in a single blog post. Instead, I want to focus on fulfilling your pretend boss' request using Zoho's scripting language, Deluge, and REST APIs.

## Zoho Deluge and REST API

Deluge, short for "Data Enriched Language for the Universal Grid Environment", comes with a variety of features such as:
- built-in functions and wrappers that are specifically tailored for Zoho applications
- no reliance on external libraries
- designed to be easy to read
- applications built with the language have fully normalized relational data models
- the language is query-integrated

Here are some examples of the language:

```js
// Fetch the contact record
contact = zoho.crm.getRecordById("Contacts", contact_id);

// Update the phone number
contact.get("Phone") = "123-456-7890";

// Save the updated record
zoho.crm.updateRecord("Contacts", contact_id, contact);
```




## Understanding External APIs and Their Use Cases

## Setting Up Zoho Deluge for API Integration

## How to Integrate External APIs with Zoho Deluge

## Storing and Managing API Data in Zoho

## Implementing a Real-World Use Case: A Step-by-Step Example

## Tips for Optimizing API Integration in Zoho Deluge

## Conclusion and Additional Resources

John Gruber created the [Markdown](#) language in 2004 in collaboration with Aaron Swartz on the syntax, with the goal of enabling people "to write using an easy-to-read and easy-to-write plain text format". Its key design goal is readability. That the language be readable as-is.

> "Markdown is a lightweight markup language with plain-text-formatting syntax"

To this end, its main inspiration is the existing conventions for marking up plain text in email, though it also draws from earlier markup languages, notably setext, Textile, and reStructuredText.

## Markdown Flavours

From 2012, a group of people including Jeff Atwood and John MacFarlane launched what Atwood characterized as a standardization effort. A community website now aims to "document various tools and resources available to document authors and developers, as well as implementors of the various markdown implementations".

![image]({{ site.baseurl }}/assets/images/gen/content/content-4.webp){:class="custom-image-class"}
{% include framework/shortcodes/image.html src="/assets/images/gen/content/content-3.webp" %}

### GitHub Flavored Markdown (GFM)

In 2017, GitHub released a formal specification of their GitHub Flavored Markdown (GFM) that is based on CommonMark. It follows the CommonMark specification exactly except for tables, strikethrough, autolinks and task lists, which the GitHub spec has added as extensions. GitHub also changed the parser used on their sites accordingly, which required that some documents be changed. For instance, GFM now requires that the hash symbol that creates a heading be separated from the heading text by a space character.he user to create their own.

{% include framework/shortcodes/figure.html src="/assets/images/gen/content/content-2.webp" title="There are many popular text editors for Markdown" caption="VSCode Editor" alt="Photo of designing a website in Figma" link="https://figma.com" target="_blank" %}

### Markdown Extra

Markdown Extra is a lightweight markup language based on Markdown implemented in PHP (originally), [Python](#) and [Ruby](#). It adds features not available with plain Markdown syntax. Markdown Extra is supported in some content management systems such as, for example, Drupal.

### MDX

At the same time, a number of ambiguities in the informal specification had attracted attention.These issues spurred the creation of tools such as Babelmark to compare the output of various implementations, and an effort by some developers of Markdown parsers for standardisation. However, Gruber has argued that complete standardization would be a mistake:

```js
$(window).scroll(function () {
  // this will work when your window scrolled.
  var scroll = $(window).scrollTop();
  if (scroll > 100) {
    $(".header").addClass("header-scrolled");
  } else {
    $(".header").removeClass("header-scrolled");
  }
});
```

Gruber avoided using curly braces in Markdown to unofficially reserve them for implementation-specific extensions. Markdown Extra adds the following features to Markdown:

- markdown markup inside HTML blocks
- elements with id/class attribute
- fenced code blocks that span multiple lines of code
- tables
- definition lists
- footnotes
- abbreviations
