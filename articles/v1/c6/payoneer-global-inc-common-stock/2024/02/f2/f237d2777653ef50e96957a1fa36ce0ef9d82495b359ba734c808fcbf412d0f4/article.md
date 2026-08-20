---
schema_version: "1.0.0"
document_id: "f237d2777653ef50e96957a1fa36ce0ef9d82495b359ba734c808fcbf412d0f4"
company_key: "payoneer-global-inc-common-stock"
company: "Payoneer Global Inc."
source_id: "payoneer-global-inc-common-stock-rss-9fa92a296e72"
canonical_url: "https://engineering.payoneer.com/integrating-css-gradient-into-font-awesome-icons-a3cc20bdd0fa"
published_at: "2024-02-13T07:41:23+00:00"
first_seen_at: "2026-07-20T23:18:22.924920+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:f52de4d262d99d1d120fd7dd59757293b4207280c95bdbdf32d163ba8f87f716"
---

# Integrating CSS gradient into Font Awesome icons

# **Integrating CSS gradient into Font Awesome icons**


[Diana Nemolkin](https://medium.com/@womanwithalaptop?source=post_page---byline--a3cc20bdd0fa---------------------------------------)


5 min read


·


Feb 13, 2024


--


Press enter or click to view image in full size


Payoneer’s universal branding strategy is characterized by striking gradient patterns encompassing a full spectrum of color shades. The patterns are featured in the company logo, backgrounds, UI elements, and custom-designed intricate icon images. To simplify the overall design process we integrated the open-source Font Awesome icons and toolkit. My team was tasked with branding the icons with[Payoneer](https://www.payoneer.com/) ’s brand gradient patterns. This post describes the related technologies and offers a simple solution.


> **As a software engineer at a large corporate company, I love solving challenges with a visual impact.**


In today’s world, it’s a complicated task to build web applications. Modern web apps require characteristics such as being lightweight, scalable, maintainable, versatile, and visually appealing. The CSS style sheet language has evolved to fulfill all these requirements.


## CSS gradient images


CSS gradient images are a powerful resource for adding depth and visual appeal to your web designs. Its ability to blend multiple colors seamlessly and create intricate gradient effects has become an integral component of modern web development. Here are some of the benefits:


S


**calable and maintainable** — CSS gradients are highly controllable through code. You can animate, adjust the size, change the colors, and even dynamically alter patterns based on scroll behavior.


L


**ightweight** — CSS gradients are preferable as they load faster than raster images. Here’s a quick[performance comparison](https://copyprogramming.com/howto/which-one-has-better-performance-css-gradient-or-background-image#what-is-the-background-image-property-in-css) of CSS gradient image vs. background image file.


V


**ersatile** — Over the last three decades, CSS has undergone significant evolution, transforming into a creative tool that artists can utilize to bring their visions to life on the web. CSS empowers artists to explore various design techniques, such as intricate layouts, elaborate animations,[sophisticated patterns](https://cssgradient.io/blog/gradient-patterns/) , and dynamic visual effects. Artists like[Ben Evans](https://codepen.io/ivorjetski) and[Julian Garnier](https://twitter.com/JulianGarnier) have embraced CSS as a medium for artistic expression. They have harnessed the potential to create stunning and visually captivating web experiences, pushing the boundaries of what is possible in digital art.


Press enter or click to view image in full size


“Pure CSS Landscape — An Evening in Southwold” by BenEvans Press enter or click to view image in full size


“CSS-Only Solar System” by Julian Garnier


## **Font Awesome icons**


The popularity of icon fonts has soared for several compelling reasons. They offer the advantage of smooth scalability and improved image quality compared to raster images. Furthermore, being treated as text, font icons can be effortlessly customized using CSS to alter their solid color and adjust their size. Notably, font icons boast exceptional performance and aesthetics, maintaining their excellent appearance even on high pixel-density displays. Within the Font Awesome library, you’ll find over 30,000 icons — 2,000+ of them for[free](https://fontawesome.com/search?m=free&o=r) .


Font Awesome[Image Icon](https://fontawesome.com/icons/image?f=classic&s=regular)


## The technical challenge


The Font Awesome icon color property supports the[<color>](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value) CSS data type. This means that the default icon color accepts only **solid** color values like ‘#000000’ or ‘black’.


Default image with solid color value


## The technical solution


Implement the[<gradient>](https://developer.mozilla.org/en-US/docs/Web/CSS/gradient) CSS image as the icon color by switching color and background image values.


## Get Diana Nemolkin’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**LETS GO!**


1. Define the` background-image` property as a CSS gradient image.
2. Define a solid` color` property value. Remember, this value will be used in a minute, as the background color.


Color and CSS background image values


3. Use`[faSquareFull](https://fontawesome.com/icons/square-full?f=classic&s=solid)` solid icon as a[mask](https://fontawesome.com/v5/docs/web/style/mask) , to switch between the values.


Color values switched


Voila!


> **“Great things are done by a series of small things brought together.”** —[Vincent Van Gogh](https://codepen.io/petrek/pen/JjjKByO)


Next, you can review the working code, including an additional script to change the icon’s background color on hover. There is also a complementary implementation for gradient text and a circle icon with a circular gradient image background just for fun!


```text
/*App.tsx*/  import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";  import { faSquareFull } from "@fortawesome/free-solid-svg-icons";  import { faImage, faCircleDot } from "@fortawesome/free-regular-svg-icons";  import "./styles.css";   export default function App() {    return (      <main className="hover-me">        <h1>Gradient Icon</h1>        <FontAwesomeIcon icon={faImage} mask={faSquareFull} />        <FontAwesomeIcon          className="circle"          icon={faCircleDot}          mask={faSquareFull}        />      </main>    );  }
```


```text
/*styles.css*/  svg {    margin: 30px;    font-size: 140px;    color: #ffffff;    background-image: linear-gradient(      150deg,      #ff4800 13.4%,      #da54d8 50%,      #0092f4 86.6%    );  }   /* BONUS */  /* Gradient Text */  h1 {    font-family: Courier;    font-size: 72px;    background-image: linear-gradient(150deg, #ff4800, #da54d8, #0092f4);    background-clip: text;    color: transparent;  }   /* Circle icon with a circular gradient image background */  svg.circle {    background-image: conic-gradient(      from 270deg,      #ff4800 10%,      #dfd902 35%,      #20dc68,      #0092f4,      #da54d8 72% 75%,      #ff4800 95%    );  }   .hover-me {    text-align: center;  }   /* Try to comment this code and hover the icon */  .hover-me:hover {    background-color: #a1fff9;  }   .hover-me:hover svg {    color: #a1fff9;  }
```


The result of the code above


An Update:


Some icons do not conform to the square mask as defined in the code above, causing them to be displayed only partially.


Press enter or click to view image in full size


The wide[icon](https://fontawesome.com/icons/network-wired?f=classic&s=solid) and the square mask


There are two methods to address this issue:


The first method involves using a[pro plan](https://fontawesome.com/plans) icon[faRectangle](https://fontawesome.com/icons/rectangle?f=classic&s=solid) , as the mask icon.


```text
<FontAwesomeIcon icon={faNetworkWired}  mask={faRectangle} />
```


Alternatively, if you prefer to use free icons, you can adjust the[transform property](https://docs.fontawesome.com/web/style/power-transform) of the FontAwesomeIcon React component.


```text
<FontAwesomeIcon icon={faNetworkWired} mask={faSquareFull} transform="  shrink-3.5  " />
```


If you found this post helpful, please show your appreciation by clicking the Clap button👏 as many times as you can. This will make it easier for others to discover this valuable information.
