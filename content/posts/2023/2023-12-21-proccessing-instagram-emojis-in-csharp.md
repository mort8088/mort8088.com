---
title: Proccessing Instagram emojis in cSharp
description: Have you ever spent so long on a simple problem you forget why you started on something in the first place?
date: 2023-12-21T00:00:00.000Z
cover: images/InstaExport debug.webp
draft: false
tags:
    - dotNet
    - howto
    - json
categories: []
author: Dave
showFullContent: false
readingTime: true
hideComments: false
toc: false
---

Have you ever spent so long on a simple problem that you forget why you started on something in the first place?

I deleted my Facebook account years ago, and recently Twitter went the same way. So I pulled my data from Instagram in preparation to kill that account as well. Then I thought I should _do_ something with this data I've been liberating from these silos.

Because Instagram was the last one I'd downloaded, I thought I'd start there. Once I'd figured out which file contained the actual post data, I set about making a converter to consume the `content.json` file and spit it out into a format I could make something from. This is where the problem starts.

## Emoji Problem

Like most people, I'd taken to adding some flair to my Insta posts in the form of Emoji, mostly smiley faces, but how do you represent them when you are asked for a flat JSON file of strings?

Take, for example, this string:

```json
"title": "Gator bites \u00f0\u009f\u0098\u0081"
```

which, when I saved out the new file, became:

```text
"title" = "Gator bites ð"
```

## Solution

I spent far too long playing with file encoding and checking what the individual character values were that, in the end, I realized that the reason I couldn't find that character sequence I was expecting was that the `JsonSerializer.Deserialize` function had changed the encoding of the string values (badly), and what I needed to do was process the file before I deserialize the stream.

So this:

{{< image src="/images/InstaExport debug.webp" alt="Looking at the data in debug" style="position: relative; z-index:10; float: right;" >}}

```csharp
var jsonStr = File.ReadAllText("posts_1.json");

var jsonRoot = JsonSerializer.Deserialize<Root[]>(jsonStr);
```

Becomes this:

{{< image src="/images/InstaExport debug Fixed.webp" alt="Looking at the fixed data in debug" style="position: relative; z-index:10; float: right;" >}}

```csharp
var jsonStr = File.ReadAllText("posts_1.json");

string stringToFind = "\\u00";
jsonStr = jsonStr.Replace(stringToFind, "%");

var jsonRoot = JsonSerializer.Deserialize<Root[]>(jsonStr);
```

Then you can use:

```csharp
var title = HttpUtility.UrlDecode(media.Title);
```

to make it a nice string with the emoji character in it 👍
