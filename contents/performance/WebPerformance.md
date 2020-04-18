<frontmatter>
  title: Web Performance
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Web Performance

**Author(s): [Marvin Chin](https://github.com/marvinchin)**

Reviewers: [Lu Yang Kenneth](https://github.com/luyangkenneth), [Monika Manuela Hengki](https://www.github.com/monmanuela)

<box id="article-toc">

* [What is Web Performance‎](#what-is-web-performance)
* [Why Web Performance Matters‎](#why-web-performance-matters)
* [Key Ideas in Improving Web Performance‎](#key-ideas-in-improving-web-performance)
* [Measuring Web Performance‎](#measuring-web-performance)
* [Additional Resources‎](#additional-resources)
</box>

## What is Web Performance?

Web Performance is a broad term that refers to how performant a web application *feels* to its users. This includes many aspects such as how long the site takes to load, how quickly the site becomes interactive, and how responsive it feels when the user interacts with the site.

As web applications grow more complex, it becomes increasingly important for web developers to be aware of the factors that affect performance, and consider how performance can impact the user experience.

## Why Web Performance Matters?

The performance of your web application has a direct impact on its ability to attract and retain users:
- Sites which take more than 3 seconds to load are more likely to be abandoned by users [[source](https://developer.akamai.com/blog/2016/09/14/mobile-load-time-user-abandonment)].
- Conversely, reducing page load times have been shown to have significant impact on improving user engagement [[source](https://medium.com/carousell-insider/how-we-made-carousells-mobile-web-experience-3x-faster-bbb3be93e006)].
- In addition, search engines have begun to use page load times as a factor in determining search rankings [[source](https://webmasters.googleblog.com/2018/01/using-page-speed-in-mobile-search.html)].
- Improving web performance is also essential in making your web application accessible to users from emerging markets, where low-end devices and limited bandwidth are the norm [[source](https://building.calibreapp.com/beyond-the-bubble-real-world-performance-9c991dcd5342)].

In order to deliver a positive user experience, web developers must ensure that their applications meet acceptable performance standards.

## Key Ideas in Improving Web Performance
There are many factors which affect web performance. Here, we give an overview of some of the key ideas that can be used to improve the performance of your web application.

**Idea 1: Reduce Javascript Payloads**

Loading and executing Javascript is often the slowest part of the page load process [[source](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4)]. Reducing the amount of Javascript that needs to be loaded thus significantly reduces the time taken for your site to load.

Here are some steps that web developers can take to reduce the amount of Javascript that clients need to load:
- Use code splitting to load only the Javascript required for the page being accessed [[source](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/)].
- Remove unused code (often parts of libraries that are not used in the application) with tree shaking [[source](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/tree-shaking/)].
- Ensure that the delivered Javascript is minified [[source](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/javascript-startup-optimization/)].
- Create performance budgets that specify the maximum size of Javascript payloads, and enforce them to ensure that the size of your Javascript payloads remain under control [[source](https://web.dev/fast/incorporate-performance-budgets-into-your-build-tools)].

**Idea 2: Optimize Images**

Images form a significant portion of the resources loaded on web applications [[source](https://httparchive.org/reports/page-weight)]. To improve performance and speed up the site for users, web developers should thus try to reduce the amount of bandwidth used to load images.

Images can be optimized for the web in the following ways:
- Use the correct file format that provides the appropriate balance between quality, detail, and file size. For example, you should use JPEG for photographs that contain lots of colors, PNG for logos and icons which have less colors but contain sharper details, and GIF for animated images [[source](https://medium.com/beginners-guide-to-mobile-web-development/web-image-formats-googles-webp-17e2fe5fc53e)].
- Compress your images to reduce the amount of data that needs to be loaded [[source](https://www.html5rocks.com/en/tutorials/speed/img-compression/)]. Be careful to strike a balance between image size and quality for optimal user experience.
- Deliver appropriately sized images based on the resolution of the client device [[source](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)]. This prevents smaller (often mobile or lower-end) devices from wasting bandwidth loading large images that cannot be displayed at their full resolution on the device.

**Idea 3: Use Progressive Rendering**

The sooner the user sees content being displayed on the page, the faster they perceives the site to be. Progressive rendering achieves this by avoiding rendering the entire page all at once, but instead ordering the loading of content in a manner that allows *some* parts of the page to be rendered as quickly as possible.

Here are several ways how progressive loading can be effectively applied:
- Load only the most critical content as part of the initial render, and fetch remaining resources asynchronously after the render [[source](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/)]. Code splitting can also be helpful here to reduce the amount of Javascript that needs to be loaded for the initial render [[source](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/)].
- Use lazy loading to defer requesting non-critical resources until they are needed [[source](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/)].
- Show content placeholders while fetching resources to indicate loading progress to the user [[source](https://medium.com/@praveencnaik/content-placeholder-the-new-design-trend-for-audience-involvement-e2ab533d7304)].

## Measuring Web Performance

Improving web performance is a continuous, ongoing process. The ability to measure and track performance is necessary for developers to monitor the impact of their changes over time.

Here are some ways that you can measure the performance of your web applications:
- Test your site under various network conditions with [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/network/network-conditions). This is helpful to get a feel for how the site feels for users on slower devices during the development process.
- Audit web performance using [Lighthouse](https://developers.google.com/web/tools/lighthouse/). This provides many useful metrics for understanding the performance of your site and identifying performance bottlenecks under controlled conditions.
- Use [WebPageTest](https://www.webpagetest.org/) to test the performance of your website from multiple locations around the world at real consumer connection speeds. This provides real-world performance indicators that is especially useful if the target audience of your site resides in a different region.
- Collect user-centric performance metrics to help you to monitor how your application actually performs in a real-world scenario, and how your site performance relates to other user engagement metrics [[source](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics)].


## Additional Resources
- [Google's Web Performance Fundamentals](https://developers.google.com/web/fundamentals/performance/why-performance-matters/) is a comprehensive resource that explains many factors that affect web performance.
- [Kinsta's Beginner Guide to Website Speed Optimization](https://kinsta.com/learn/page-speed/) is a guided resource for improving website speeds.
- [Twitter Lite Case Studey](https://medium.com/@paularmstrong/twitter-lite-and-high-performance-react-progressive-web-apps-at-scale-d28a00e780a3) is a real-world study that highlights how web performance principles can be applied to great effect.

</div>
