---
schema_version: "1.0.0"
document_id: "4b41370903c61da77de4ccbcf39d20a5213a151ce6ac447c21a23b67b57a85be"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/3-ways-to-setup-multi-brand-design-systems-in-supernova"
published_at: "2024-05-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:e65e66b8aa2f24d21b2547b59347e16ccb1ad5c5f3d4bfd9971e37628efc1d0b"
---

# 3 Ways to Setup Multi-brand Design Systems in Supernova

Multi-brand design systems are an excellent way to support teams with more than one brand, product, or platform. Instead of duplicating design and development efforts, you can streamline your design system to support your different brands effectively. In this post, we will be exploring three different ways to establish a flexible and scalable multi-brand design system structure using Supernova.


We will use a hypothetical design system, the Solar System, which serves three main brands: Jupiter, Saturn, and Mercury, as an example.


Before we delve into the details of setting up each brand, it's essential to understand Supernova's ecosystem and how it supports multi-brand architecture.


A workspace is the core of your design system ecosystem in Supernova. It is where your team works, and your viewers consume the design system. Inside a workspace, you can host multiple design systems, each housing elements like components, assets, and tokens. Each design system can have a single documentation website, which we'll discuss in more detail later.


## Multi-brand setup type A


The first approach to a multi-brand setup is using themes for each brand in the system. These themes can be represented as modes in Figma, and with[Supernova’s Figma variables plugin](https://www.figma.com/community/plugin/1303357900761384370/supernova-figma-variables-sync) , adding modes couldn't be easier. After selecting your workspace and the design system you want to map in Supernova, click 'push to Supernova,’ and you will see your modes mapped out as themes.


Documenting these themes is simple. You select them in the token blocks and use the block properties to show them in a split view or override the base to display the themes individually for each brand. Other elements, like components and assets, can be uploaded to the design system under design data.


Also, in the component manager, create new custom properties to document the components for each of your brands. Then, showcase these components in the documentation using the "component table" block.


Here's a live example of a documentation website using the type A setup:


[Welcome! | Made with Supernova](https://multi-brand-setup-type-a.supernova-docs.io/latest/welcome-QuyqvdaQ)


## Multi-brand setup type B


If the brands in your design system share some elements but are mostly different, and you want them to share the same documentation website, then setup type B is for you. This setup type adds an additional layer to your design system in Supernova, allowing for a greater separation of concerns.


To enable brands, go to S **ettings** , turn on the **Enable multiple brands** toggle, and add each brand you want to include. This creates a sub-layer to your design system, allowing you to add a file for each of the brands and import the styles that will be converted into tokens for each. The same goes for all other design system elements.


Remember that each design system corresponds to one set of documentation, so all of the brand elements will be accessible within that documentation. You will have access to them as an editor in each content block. Using a tab block can help document the different brand elements and better communicate those differences to your audiences.


Here's a live example of a documentation website using the type B setup:


[Welcome! | Made with Supernova](https://multi-brand-setup-type-b.supernova-docs.io/latest/welcome-xzOWmcbp)


## Multi-brand setup type C


The last setup type offers the most flexibility. It allows you to map each brand to a different design system in your workspace, which means each brand can have its own individually branded documentation.


To enable this, go to your documentation settings, click on 'enable multi-design system navigation' for each of the brand documentation settings, and select the corresponding ones you want to enable navigation for.


Publish each of the documentation, and you should be able to access the different design systems through the main navigation. This also enables URL matching, so your design system consumers will be able to navigate through the same content type across the different brands without losing context.


In the **Look & feel** section of your documentation settings, you can edit the overall styling of your documentation. This ensures that your documentation aligns with your brand's aesthetic and fits well within your multi-brand design system. Furthermore, the **Theme override** feature provides added customization and flexibility. It lets you adjust specific elements of the documentation's design, enabling you to match each brand's documentation to its unique style.


Here's a live example of a documentation website using the type C setup:


[Welcome! | Made with Supernova](https://multi-brand-setup-type-c.supernova-docs.io/latest/welcome-W1CtgHZy)


## Multi-brand setup workshop


Recently, we hosted a workshop specifically focusing on multi-brand setup within Supernova. During this session, we delved deeper into each of these setup types, sharing practical tips and examples. We also talked about potential challenges and how to overcome them. The workshop was recorded and is available for everyone to watch.


Additionally, here is the[FigJam of the session](https://www.figma.com/board/VI8jDP3SVuDFQaJWYrtjVh/Setting-up-a-multi-brand-design-system-workshop?node-id=2%3A1885&t=rODrkLVO3tjasEi1-1) with a template you can use as a starting point for your multi-brand design system setup.


That concludes our exploration of the three ways you can set up[Supernova](https://www.supernova.io/) for a multi-brand design system. We hope you found this post useful, and we look forward to sharing more tutorials in the future. Happy designing!


Explore three ways to establish a flexible and scalable multi-brand design system structure using Supernova. This post offers practical tips, examples, and resources for each setup type.
