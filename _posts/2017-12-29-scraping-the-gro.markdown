---
layout: post
title: Scraping GRO search results using AngleSharp
date: '2017-12-29 16:51:49'
---

Python has [Scrapy](https://scrapy.org/), [BeautifulSoup](http://www.crummy.com/software/BeautifulSoup/) and [lxml](http://lxml.de/). Ruby has [Mechanize](http://docs.seattlerb.org/mechanize/GUIDE_rdoc.html) and [nokogiri](http://www.nokogiri.org/). What about .NET?

## Requirements
To scrape websites you generally need a library which will allow you to: navigate the site; log in if necessary; submit search forms; page through results; extract the data. Other considerations include: performance; tolerance of malformed markup. Tools usually fall into one of two categories: website crawlers or HTML parsers with some having elements of both.

Most articles I've so far found using .NET to scrape websites seem to focus only on parsing a single page of results, which is frustrating as there's usually a lot more process before you get to that part.

## Contenders
There appears to be no particular de facto "go to" library for .NET. The options are:

* [HtmlAgilityPack](http://html-agility-pack.net/?z=codeplex) - this is possibly the most established library for parsing HTML, though appears to now be under new ownership. The new website seems a bit sparse on details (and a bit broken). Its an HTML parser rather than a full scraping library.
* [ScrapySharp](https://bitbucket.org/rflechner/scrapysharp/wiki/Home) - provides scraping functionality on top of HtmlAgilityPack. Last updated over 18 months ago, precious little documentation.
* [IronWebScraper](http://ironsoftware.com/csharp/webscraper/) - this looked promising, though again very little "getting started" type information, just the source code generated documentation. I installed the NuGet package and started to use it, but there seemed to be a strange model of saving scraped results to the file system. Abandoned.
* [AngleSharp](https://anglesharp.github.io) - seems like development has stalled recently and documentation is scattered and not linked from the main "showcase" website. Nevertheless a bit of digging reveals the crawling and parsing aspects seem reasonably functional.
* [DotnetSpider](https://github.com/dotnetcore/DotnetSpider) - referenced from the HtmlAgilityPack third party library page - by all accounts a port of Scrapy / WebMagic - looks interesting, not yet tried.
## Supporting Tools
* [Fiddler](https://www.telerik.com/fiddler) - when you're scraping and submitting forms and nothing is working, its useful to check out exactly what the scraping library is actually doing. Fiddler is a debugging proxy which allows you to see the requests and responses going back and forwards on the wire.

## The GRO website
The [General Register Office website](https://www.gro.gov.uk/gro/content/certificates/Login.asp) allows searching the historic indexes for births and deaths registered in England or Wales since 1837. The information available is great, but searching is slightly frustrating since you have to specify the gender and can only search within a five year range at a time. I originally developed a scraper in Ruby using Mechanize and nokogiri, but for a recent project in .NET I decided to rewrite the scraper rather than calling the existing Ruby version from the .NET code (I might yet change my mind).

## Scraping all records for one surname

### AngleSharp documentation

The GitHub repo homepage has brief getting started instructions, but the best documentation is linked from the wiki area: https://github.com/AngleSharp/AngleSharp/wiki

###  Set up

Logging in to the GRO site requires submitting a form and thereafter keeping track of cookies. In AngleSharp, this requires setting up and reusing a "browsing context". The brief instructions on the github homepage in conjunction with [an article on codeproject.com](https://www.codeproject.com/Articles/1017190/Submitting-Forms-with-AngleSharp) cover the basics.

```
var config = Configuration.Default.WithDefaultLoader().WithCookies();
var context = BrowsingContext.New(config);
```

### Log in

Having set up the context, browsing to the initial login page is as simple as `await context.OpenAsync("login-page-url")`. The object `context.Active` contains details of the current document being "browsed" i.e. the login page here. Various selectors are available to locate the login `<form>`. The `SubmitAsync` method can then be used to submit it. 
![Login page](https://www.dropbox.com/s/gu7sziv3pbw5hnc/Screenshot%202017-12-29%2017.15.32.png?raw=1)
This didn't work for ages and inspecting the differences in Fiddler between the calls AngleSharp was submitting and a basic HttpClient implementation revealed the name of the button being clicked wasn't getting sent. This was due to me submitting the form rather than "clicking the button". I eventually found [this issue](https://github.com/AngleSharp/AngleSharp/issues/354) from Florian which described the problem and the solution. Bingo.

```
var loginPage = "https://www.gro.gov.uk/gro/content/certificates/login.asp";
await context.OpenAsync(loginPage);

await context.Active
    .QuerySelector<IHtmlFormElement>("form")
    .QuerySelector<IHtmlInputElement>("input.formButton")
    .SubmitAsync(new
    {
        username = _username,
        password = _password
    });
```

### Select 'Search the GRO Indexes'

![Menu page](https://www.dropbox.com/s/odx11js39lu1wxm/Screenshot%202017-12-29%2017.26.35.png?raw=1)

There are various ways to find a link and click it. There is no `id` attribute so the least fragile option seemed to be to locate the link by the text. The selectors use CSS which aren't good at finding links by text. AngleSharp can alternatively use LINQ type queries to locate elements.

```
var result = context.Active.Links.Single(a => a.TextContent == "Search the GRO Indexes") as IHtmlAnchorElement;
await result.NavigateAsync();
```

### Choose births or deaths

The search form first requires selecting either the Birth or Death index so the remaining search fields can be tailored accordingly.

![Choose index](https://www.dropbox.com/s/5v5a2ced2khyg1e/Screenshot%202017-12-29%2017.46.36.png?raw=1)

This time simply submitting the form having selected the required radio button works fine.

```
await context.Active
    .QuerySelector<IHtmlFormElement>("form")
    .SubmitAsync(new { index = "EW_Birth" });
```

### Fill in and submit search form

### Parse first page







