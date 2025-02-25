[![Promo](https://github.com/bright-cn/Google-News-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner%20CN.png?md5=105367-daeb786e)](https://www.bright.cn/?promo=github15) 

本项目演示了如何在 Visual Studio 和 .NET 7 中使用 C# 配置代理服务器进行网页抓取（参考：https://www.bright.cn/blog/how-tos/web-scraping-with-c-sharp），并借助 HtmlAgilityPack 进行 HTML 解析。通过使用代理的 IP 地址进行遍历，可以保护您的数字身份，避免 IP 封禁和地域限制。

### 前置条件
-   [Visual Studio 2022](https://visualstudio.microsoft.com/vs/) 或 [Visual Studio Code](https://code.visualstudio.com/)
-   [.NET 7 或更高版本](https://dotnet.microsoft.com/en-us/download)
-   [HtmlAgilityPack NuGet 包](https://www.nuget.org/packages/HtmlAgilityPack/)
    
### 配置本地代理

-   下载并安装 mitmproxy。
-   通过命令 mitmproxy 启动 mitmproxy。
    
### 网页抓取设置

**ProxyHttpClient** - 用于配置 HttpClient 实例通过指定的代理服务器发送请求。

**ProxyRotator** - 管理一组代理，并提供随机选择代理的方法以处理每个网络请求。通过随机化在多个代理之间发送请求，可以有效减少被检测及 IP 封禁的风险。

当 `isLocal` 设置为 True 时，会使用本地 mitmproxy 代理；若设置为 False，则会使用公共代理 IP。

**ProxyChecker** - 用于验证代理服务器列表。当您使用 `GetWorkingProxies` 方法并传入代理 URL 列表时，该方法会通过异步调用 `CheckProxy` 来检查每个代理的状态，并把可以正常工作的代理收集到 `workingProxies` 列表中。在 `CheckProxy` 方法中，您会为每个代理 URL 建立一个 `HttpClient`，向 http://www.google.com 进行测试请求，并使用信号量（semaphore）安全地记录进度。

`IsProxyWorking` 方法通过检查响应的状态码，确认代理是否可用，如果可用则返回 true。此类能够帮助您从给定的代理列表中识别出可用的代理。

**WebScraper** - 封装了网页抓取功能。当您调用 `ScrapeData` 方法时，需要传入一个 `ProxyRotator` 实例和目标 URL。接下来会使用 `HttpClient` 异步地向该 URL 发送 GET 请求，获取 HTML 内容，并用 `HtmlAgilityPack` 库进行解析。然后使用 XPath 查询从特定 HTML 元素中定位并提取链接及对应的标题。如果找到任何文章链接，就会打印它们的标题和完整 URL；否则会输出一条提示消息，说明没有找到链接。

### 使用 Bright Data 代理

-   注册 Bright Data 并创建 [住宅代理](https://www.bright.cn/proxy-types/residential-proxies)。    
-   在 **WebScrapeBrightdata** 项目的 **appsettings.json** 文件中更新您的凭证。
    
### 运行应用程序

-   使用命令 `dotnet build` 和 `dotnet run -- --url https://www.wikipedia.org/` 编译并执行程序，运行后会显示抓取到的维基百科文章标题和链接。 

[Bright Data 的代理服务](https://www.bright.cn/proxy-types) 为 C# 的匿名和高效网页抓取提供可扩展的解决方案，帮助规避 IP 封禁。通过本教程，您可以学习如何在网页抓取项目中集成代理服务器，并遵循最佳实践来确保数据采集的可靠性。
