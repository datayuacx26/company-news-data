---
schema_version: "1.0.0"
document_id: "3cc9d405c381a00e57815ae41356e1e675d7c1738ae7f72ceb64974909cb62eb"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/improved-performance-for-tailwind-emails"
published_at: "2023-12-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:2bf0c5bee9e973fb0653ee1b70d49dceb83c5d98c20001a8090bc36d9288b20e"
---

# Improved Performance for Tailwind Emails

When we started[react.email](https://react.email/docs/introduction) , the idea was to make modern web development tooling, like[Tailwind CSS](https://tailwindcss.com/) , available for the email ecosystem.


But for some time, the` <Tailwind />` component had a few problems, leading users to go back into using inline styles. This is not the experience we wanted for developers.


That's why we're excited to announce a major performance improvement. The render time for Tailwind emails is now **23x** faster.


~3681ms before and now 156ms


These are numbers from[benchmarks](https://github.com/resend/react-email/tree/main/benchmarks/tailwind-component) on real-world[email templates](https://github.com/resend/react-email/blob/main/benchmarks/tailwind-component/src/emails/with-tailwind.tsx) .


## Benchmark Results


We used the[Tinybench](https://github.com/tinylibs/tinybench) library to run the tests. The tests were run on a Linux machine with a 3.6 GHz 6-core AMD Ryzen 5 4500 CPU and 16 GB 3600 MHz DDR4 memory.


The sample size was 100 email renders for each test, and the[same email template](https://github.com/resend/react-email/blob/main/benchmarks/tailwind-component/src/emails/with-tailwind.tsx) was used for all the tests. The results are in nanoseconds per operation.


```text
┌─────────┬───────────┬─────────┬────────────────────┬──────────┬─────────┐    │     (  index  )   │ Task Name │ ops  /  sec │ Average   Time     (  ns  )    │  Margin  │ Samples │   ├─────────┼───────────┼─────────┼────────────────────┼──────────┼─────────┤   │      0      │  after    │      8      │   120386175.23986846   │  ±  2.07  %    │     100     │   │      1      │  before   │      0      │   3326383692.750111    │  ±  0.72  %    │     100     │   └─────────┴───────────┴─────────┴────────────────────┴──────────┴─────────┘
```


Here are the results **before** the performance improvements:


- **p75** : 3385ms
- **p99** : 3681ms


And here are the results **after** the performance improvements:


- **p75** : 126ms
- **p99** : 156ms


We can see that the p99 is **23x** faster, and the p75 is **26x** faster.


## What changed


The greatest point of performance degradation that we had before was with the way we ran Tailwind to generate the styles for classes. Every time the component would come across a` className` , it would start` tailwind` alongside` postcss` , which was unnecessary.


To fix this, we changed the component to render the whole React element tree. Then, we ran Tailwind on it to know which inline styles to substitute for each class name.


We then created a map of the inline styles associated with the class names as *keys* . After that, we added these inline styles from the map to each element that uses these class names. This means the second traversal grows **linearly** instead of **exponentially** .


## Upgrading to the new version


**1. Upgrade package versions**


If you are importing the Tailwind component from` @react-email/components` you can update it to version` 0.0.12` .


```text
npm i @react  -  email  /  components@  0.0  .12
```


Or if you are importing it directly from` @react-email/tailwind` you can update it to version` 0.0.13` .


```text
npm i @react  -  email  /  tailwind@  0.0  .13
```


**2. Change component order if you use media queries**


If you are using Tailwind classes that render into media queries like dark mode, you'll have to change the order in which the components are defined.


You need to move the Tailwind component from wrapping the` <Html />` component followed by the` <Head />` component to just directly wrapping the` <Head />` component.


```text
const     Email     =     (  )     =>     (        <  Html  >          <  Tailwind  >            <  Head   /  >          <  /  Tailwind  >        <  /  Html  >     )  ;
```


## Conclusion


If you want to know more about this update, including technical details, and stability improvements, check the[React Email Changelog](https://react.email/docs/changelog) .


If you have any problems upgrading, feel free to share on[GitHub](https://github.com/resend/react-email) or[X](https://x.com/resend) .
