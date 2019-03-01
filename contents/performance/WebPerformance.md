<frontmatter>
  title: Web Performance
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Web Performance

Author: [Marvin Chin](https://github.com/marvinchin)

## What is Web Performance

Web Performance is a broad term that refers to how performant a web application *feels* to its users. This includes many aspects such as how long the site takes to load, how quickly the site becomes interactive, and how responsive it feels when the user interacts with the site.

As web applications grow more complex, it becomes increasingly important for web developers to be aware of the factors that affect performance, and consider how performance can impact the user experience.

## Why Web Performance Matters

The performance of your web application has a direct impact on its ability to attract and retain users: 
- Sites which take more than 3 seconds to load are [more likely to be abandoned by users](https://developer.akamai.com/blog/2016/09/14/mobile-load-time-user-abandonment).
- Conversely, reducing page load times have been shown to have significant impact on [improving user engagement](https://medium.com/carousell-insider/how-we-made-carousells-mobile-web-experience-3x-faster-bbb3be93e006).
- In addition, search engines have begun to use page load times as a [factor in determining search rankings](https://webmasters.googleblog.com/2018/01/using-page-speed-in-mobile-search.html).
- Improving web performance is also essential in making your web application [accessible to users from emerging markets](https://building.calibreapp.com/beyond-the-bubble-real-world-performance-9c991dcd5342), where low-end devices and limited bandwidth are the norm.

In order to deliver a positive user experience, web developers must ensure that their applications meet acceptable performance standards.

## Key Ideas in Improving Web Performance
There are many factors which affect web performance. Here, we give an overview of some of the key ideas that can be used to improve the performance of your web application.

**Reduce Javascript Payloads**

Loading and executing Javascript is often [the slowest part](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4) of the page load process. Reducing the amount of Javascript that needs to be loaded thus significantly reduces the time taken for your site to load.

Here are some steps that web developers can take to reduce the amount of Javascript that clients need to load:
- Use [code splitting](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/) to load only the Javascript required for the page being accessed.
- Remove unused code (often parts of libraries that are not used in the application) with [tree shaking](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/tree-shaking/).
- Ensure that the delivered Javascript is [minified](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/javascript-startup-optimization/).
- Create [performance budgets](https://infrequently.org/2017/10/can-you-afford-it-real-world-web-performance-budgets/) that specify the maximum size of Javascript payloads,and [enforce them](https://web.dev/fast/incorporate-performance-budgets-into-your-build-tools) to ensure that the size of your Javascript payloads remain under control.

**Optimize Images**

Images form a [significant portion](https://httparchive.org/reports/page-weight) of the resources loaded on web applications. A significant amount of time and bandwidth is used to load images, making the site feel slower to users. To improve performance, Web developers should thus try to reduce the amount of bandwidth used to load images.

Images can be optimized for the web in the following ways:
- Use the [correct file format](https://medium.com/beginners-guide-to-mobile-web-development/web-image-formats-googles-webp-17e2fe5fc53e) for your images.
- [Compress your images](https://www.html5rocks.com/en/tutorials/speed/img-compression/) to reduce the amount of data that needs to be loaded. Be careful to strike a balance between image size and quality for optimal user experience.
- Deliver [appropriately sized images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images) based on the resolution of the client device. This avoids smaller (often mobile or lower-end) devices from wasting bandwidth loading large images that cannot be displayed at their full quality on the device.

**Progressive Loading**

The sooner the user sees content being displayed on the page, the faster he perceives the site to be. One way to reduce the time taken for content to be shown is to include only the critical parts of the page in the initial render, and progressively load the remaining resources and update the page as they become available. Though the page may not contain all the content upon the initial render, the partial render makes the site feel faster and more responsive to the user.

Here are several ways how progressive loading can be effectively applied:
- Load only the [most critical content](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/) as part of the initial render, and fetch remaining resources asynchronously after the render. [Code splitting](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/) can also be helpful here to reduce the amount of Javascript that needs to be loaded for the initial render.
- Use [lazy loading](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/) to defer requesting non-critical resources until they are needed.
- Show [content placeholders](https://medium.com/@praveencnaik/content-placeholder-the-new-design-trend-for-audience-involvement-e2ab533d7304) while fetching resources to indicate loading progress to the user.

## Measuring Web Performance

Improving web performance is a continuous, ongoing process. The ability to measure and track performance is necessary for developers to monitor the impact of their changes over time.

Here are some ways that you can measure the performance of your web applications:
- Test your site under various network conditions with [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/network/network-conditions). This is helpful to get a feel for how the site feels for users on slower devices during the development process.
- Audit web performance using [Lighthouse](https://developers.google.com/web/tools/lighthouse/). This provides many useful metrics for understanding the performance of your site and identifying performance bottlenecks under controlled conditions.
- Collect [user-centric performance metrics](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics). This helps you to monitor how your application actually performs in a real-world scenario, and how your site performance relates to other user engagement metrics.


## Additional Resources
- [Google's Web Performance Fundamentals](https://developers.google.com/web/fundamentals/performance/why-performance-matters/)
- [Kinsta's Beginner Guide to Website Speed Optimization](https://kinsta.com/learn/page-speed/)
- [Twitter Lite Case Studey](https://medium.com/@paularmstrong/twitter-lite-and-high-performance-react-progressive-web-apps-at-scale-d28a00e780a3)

</div>