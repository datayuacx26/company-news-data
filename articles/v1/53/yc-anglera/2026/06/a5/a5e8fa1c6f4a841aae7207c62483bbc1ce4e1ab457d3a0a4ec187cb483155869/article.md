---
schema_version: "1.0.0"
document_id: "a5e8fa1c6f4a841aae7207c62483bbc1ce4e1ab457d3a0a4ec187cb483155869"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/sap-commerce-data-to-page"
published_at: "2026-06-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:d17a42ca612cc1960c2008122eb1569e9c26da721700e91d44adb1e7c3d8f13e"
---

# Getting enriched product data onto SAP Commerce Cloud product pages

This guide walks through what happens after a product attribute is enriched and sitting in SAP Commerce Cloud — how it moves from the platform's data model, through the DTO and API layer, into whichever storefront you run, and finally into HTML a browser or crawler can read. The example uses an IP (ingress protection) rating, a common spec for distributors and manufacturers of electrical and outdoor equipment, but the mechanics apply to any enriched attribute (dimensions, certifications, compatible-with lists, use-case copy).


## Where the attribute actually lives


SAP Commerce Cloud gives you two places to store a product attribute, and the choice determines every downstream step:


- **Typed attributes** — defined on an item type (usually` Product` or a subtype) in` items.xml` and populated per SKU via ImpEx or Backoffice's Product Cockpit. These behave like first-class columns on the product.
- **Classification attributes** — defined in a classification system (classes, categories, and features) and attached to products or inherited from a classifying category. These are the right fit for attribute sets that vary by product type (an IP rating applies to enclosures and outdoor fixtures, not to software licenses). The classification system itself — classes, features, attribute assignments — is typically modeled in Backoffice's Administration Cockpit or via ImpEx; the value for a given SKU is then set on that product's own Classification tab inside Product Cockpit's editor area.


For an IP rating, most catalogs model it as a classification feature — e.g., a` Housing` class with a feature` ratingIP` of type` String` — assigned to a classifying category above the relevant product categories, so every enclosure or luminaire under it inherits the attribute.


Loading (or updating) the value with ImpEx looks something like this (exact column layout and modifiers vary by version and by how your classification catalog is structured):


```text
INSERT_UPDATE ClassAttributeAssignment;classificationClass(code,catalogVersion(catalog(id),version))[unique=true];classificationAttribute(code)[unique=true];position;attributeType(code[default=string])
;Enclosures;ratingIP;1;


$clAttrModifiers=system=Electronics,version=1.0,translator=de.hybris.platform.catalog.jalo.classification.impex.ClassificationAttributeTranslator,lang=en
UPDATE Product;code[unique=true];@ratingIP[$clAttrModifiers]
;ENC-4500;IP66


```


If Anglera (or any enrichment source) is writing the value, it ends up here — as a typed attribute or a classification feature value — via ImpEx, the Backoffice API, or a direct integration into these same item types.


## Binding it to the DTO layer


Nothing in the storefront reads` ProductModel` (the platform-side object) directly. Everything goes through the **Converters and Populators** framework, which maps the model onto a Data Transfer Object —` ProductData` for products. A populator is a small class that copies one or a few fields from model to DTO; converters chain populators together.


To surface` ratingIP` you add a populator:


```text
public class RatingIpPopulator<SOURCE extends ProductModel, TARGET extends ProductData>
extends AbstractProductPopulator<SOURCE, TARGET> {


@Override
public void populate(final SOURCE productModel, final TARGET productData) {
final String ratingIp = productModel.getRatingIP(); // typed attribute getter,
// or, for classification
// values, ClassificationService
if (StringUtils.isNotBlank(ratingIp)) {
productData.setRatingIP(ratingIp.trim());
}
}
}


```


then register it in the facade Spring config, appended to the existing populator list rather than replacing it:


```text
<bean id="ratingIpPopulator" class="com.yourcompany.facades.populators.RatingIpPopulator"/>
<bean parent="modifyPopulatorList">
<property name="list" ref="productBasicPopulatorList"/>
<property name="add" ref="ratingIpPopulator"/>
</bean>


```


` ProductData` itself needs the new field declared in your facades extension's` beans.xml` (the platform's own fields live in` commercefacades-beans.xml` ) so the code generator emits a getter/setter. For classification-based values, you'd typically pull the value through the classification feature-value API (` ClassificationService` ) rather than a generated model getter.


## Exposing it through the OCC API


OCC (Omni Commerce Connect) is the storefront-facing REST layer — for example` /occ/v2/electronics/products/ENC-4500` — but it serializes a separate` ProductWsDTO` , not` ProductData` directly, so a new field generally needs adding to both, with a mapping between them declared in the webservices extension's DTO config. What a client actually receives is then controlled by **field sets** —` BASIC` ,` DEFAULT` ,` FULL` , or custom sets — configured in` dto-level-mappings-v2-spring.xml` . Add the field to the level(s) you want visible:


```text
<entry key="DEFAULT" value="...,availability,price(DEFAULT),ratingIP"/>
<entry key="FULL" value="...,averageRating,ratingIP"/>


```


Skip this step and the field gets stripped from the response, even though the populator ran fine.


## Rendering it: Accelerator vs. Composable Storefront


Which storefront you run changes the last mile:


**Accelerator (JSP, server-rendered).** The controller resolves` ProductData` and hands it to the JSP view as a model attribute; you add a JSTL tag reference in the relevant product JSP (commonly under` /web/webroot/WEB-INF/views/responsive/product/` ):


```text
<c:if test="${not empty product.ratingIP}">
<div class="product-spec product-spec--rating-ip">
<span class="spec-label">Ingress protection:</span>
<span class="spec-value">${fn:escapeXml(product.ratingIP)}</span>
</div>
</c:if>


```


This produces attribute text in the initial server-rendered HTML — no JavaScript required for it to appear in view-source.


**Composable Storefront (Spartacus, Angular).** SAP renamed the Spartacus libraries "SAP Commerce Cloud, composable storefront" at version 5.0; it's now the primary storefront direction, with Accelerator in maintenance mode. Here the client never sees` ProductData` — it consumes OCC JSON and needs a normalizer plus a component:


```text
declare module '@spartacus/core' {
namespace Occ {
export interface Product {
ratingIP?: string;
}
}
export interface Product {
ratingIP?: string;
}
}


@Injectable({ providedIn: 'root' })
export class RatingIpNormalizer implements Converter<Occ.Product, Product> {
convert(source: Occ.Product, target?: Product): Product {
target = target ?? { ...(source as unknown as Partial<Product>) };
if (source.ratingIP) {
target.ratingIP = source.ratingIP;
}
return target as Product;
}
}


```


```text
@Component({
selector: 'app-rating-ip',
template: `<div class="product-spec" *ngIf="ratingIP">
Ingress protection: {{ ratingIP }}
</div>`,
})
export class RatingIpComponent implements OnInit {
ratingIP: string | undefined;
constructor(protected currentProductService: CurrentProductService) {}
ngOnInit(): void {
this.currentProductService
.getProduct()
.pipe(filter(isNotNullable))
.subscribe((product) => (this.ratingIP = product.ratingIP));
}
}


```


register the normalizer against` PRODUCT_NORMALIZER` , and slot the component into a CMS outlet (e.g.,` ProductDetailOutlets.PRICE` ) via` provideOutlet` . Because this is client-rendered, the attribute won't be in the initial HTML from the origin server — it arrives after Angular hydrates and the OCC call resolves, unless you're running SSR/prerendering, in which case it's baked into the served HTML on first response.


## How to validate


- **View-source vs. rendered DOM.** Load the PDP, then compare` curl -s https://yourdomain.com/p/ENC-4500 | grep -i "ratingIP\\|Ingress"` against the browser's Inspect Element. On Accelerator, both should show the value. On non-SSR composable storefront, view-source will be empty for this field while the rendered DOM has it — that gap is exactly what a JavaScript-blind[AI crawler](https://www.anglera.com/glossary/ai-crawler) will miss.
- **Confirm the API layer independently.** Hit the OCC endpoint directly:` curl "https://api.yourdomain.com/occ/v2/electronics/products/ENC-4500?fields=FULL"` and check the field is present. If it's missing here, the problem is the populator or field-set config, not the frontend.
- **Structured data.** If the attribute should also appear in` Product` /` schema:additionalProperty`[JSON-LD](https://www.anglera.com/glossary/json-ld) , run the page through Google's[Rich Results Test](https://search.google.com/test/rich-results) to confirm it's present in both the visible copy and the structured data.
- **Cache check.** SAP Commerce Cloud's CDN and page caching can serve a stale copy for a while after an update; purge or wait out the cache before concluding a change didn't take.


Verified as of July 2026 against current SAP Help Portal documentation for Converters and Populators, Classification Systems, and OCC field-set configuration, and current SAP Commerce Cloud, composable storefront (Spartacus 5.0+) product-model patterns. Field names, class names, and ImpEx modifiers are illustrative — confirm exact syntax against your installed version, since populator lists, DTO beans, and outlet IDs shift between releases.


- [Converters and Populators — SAP Help Portal](https://help.sap.com/docs/SAP_COMMERCE/9d346683b0084da2938be8a285c0c27a/8b937ff886691014815fcd31ff1de47a.html)
- [Classification Feature Value API — SAP Help Portal](https://help.sap.com/docs/SAP_COMMERCE/d0224eca81e249cb821f2cdf45a82ace/8b7a777486691014823afa6e0ed7bb61.html)
- [Spartacus (SAP/spartacus) — GitHub](https://github.com/SAP/spartacus)


None of this matters if the underlying data isn't there to begin with — a populator, field set, and component are only worth building if` ratingIP` , or whatever attribute you're wiring up, is actually filled in and correct across the catalog. That's the piece Anglera is built for: it enriches products continuously in the background — attributes, specs, identifiers, use-case copy — and writes them back into the same typed or classification attributes this guide assumes are already populated, so the plumbing above has something real to render.
