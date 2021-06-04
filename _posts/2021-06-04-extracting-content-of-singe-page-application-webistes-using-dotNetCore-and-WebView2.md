---
layout: post
title:  Extracting content of Single-page application websites using .NET Core & WebView2
author: Nghia Nguyen
tags:   [SPA, SPA Extractor, .NET Core, WebView2]
comments_id: 2
---
One of my project needs is to extract some texts from Single-page application (SPA) websites. There are several ways to do so, but here is the simple way I have done to extract content of SPA websites using .NET Core and [Microsoft Edge WebView2](https://docs.microsoft.com/en-us/microsoft-edge/webview2/).

First, I use `webView21.CoreWebView2.ExecuteScriptAsync("document.documentElement.outerHTML")` to get the whole content of the HTML page which is a JSON-encoded string.

Then, I need to use `JsonSerializer.Deserialize<string>()` to decode this JSON-encoded string to a HTML string.

Finally I use Regex pattern to extract content I need from the HTML string.

Here is the code. The Regex pattern I am using here is working well to extract trending search terms or topics from Google Trends at the time I am writing this post. You need to change the Regex pattern to extract whatever you need from your websites.

```csharp
private async void btnGetTrends_Click(object sender, EventArgs e)
{
    var html = await webView21.CoreWebView2.ExecuteScriptAsync("document.documentElement.outerHTML");

    var htmlDecoded = System.Text.Json.JsonSerializer.Deserialize<string>(html);

    var regex = new System.Text.RegularExpressions.Regex("<div bidi=\"value\" dir=\"ltr\"><span ng-bind=\"bidiText\">((.)*)</span></div>");
    var matches = regex.Matches(htmlDecoded);

    lstTrends.Items.Clear();
    foreach (System.Text.RegularExpressions.Match m in matches)
    {
        lstTrends.Items.Add(m.Groups[1].Value);
    }
}
```

Here is an example that I successfully extract trending search terms or topics from [Google Trend](https://trends.google.com/trends/yis/2020/US/) at the time I am writing this post.

![](/asset/images/2021-06-04/spa-extractor-sample-app.png)

You can download the entire source code from [GitHub repo](https://github.com/nghianguyen09/spa-content-extractor-using-webview2)

#### How to use the application:
1. Enter __URL__ pointing Google Trends by geo & year in format `https://trends.google.com/trends/yis/{year}/{geo}/`, for example `https://trends.google.com/trends/yis/2020/US/`
2. Click the __GO__ button
3. Wait until the __Get Trends__ button is enabled, click on it to extract trending search terms or topics listed (that you are able to view) in the page.
4. Now you will see all extracted trending search terms or topics in the box next to the left of the __Get Trends__ button.

That's all. It's simple, huh?