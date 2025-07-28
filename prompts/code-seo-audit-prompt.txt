# Comprehensive SEO Audit Prompt

## Instructions
Copy this entire prompt and paste it into your AI assistant along with your project's source code to receive a detailed SEO audit.

---

## The Prompt

Analyze the source code of this website to identify SEO issues that can be detected through code inspection. Focus on the codebase structure, rendering logic, and implementation patterns that affect search engine optimization.

### Audit Scope:

- Examine source files (components, templates, views, controllers)
- Analyze routing configurations and URL generation
- Review data fetching patterns and rendering methods
- Check build/compilation settings that impact SEO

### Key Areas to Analyze:

#### 1. HTML Header Tag Structure (H1-H6):

Check for these specific issues:
- Missing H1 tags: Every page must have exactly one H1 tag
- Multiple H1 tags: Flag pages with more than one H1
- Skipped heading levels: H1 should be followed by H2, not H3 (no jumping from H1 to H3)
- Heading hierarchy: Ensure logical nesting (H2s under H1, H3s under H2, etc.)
- Empty headings: Headings with no text content
- Headings used for styling: Non-semantic use of heading tags just for font size
- Dynamic heading generation: Check if headings are hardcoded or dynamically generated

Code patterns to look for:
```javascript
// Bad patterns:
<h1></h1> // Empty
<h1>{title || ""}</h1> // Potentially empty
<h3>Subtitle</h3> // Missing H1 and H2
<h1>Page Title</h1>
<h1>Another Title</h1> // Multiple H1s

// Good patterns:
<h1>{title || "Default Title"}</h1>
<h2>Section Title</h2>
<h3>Subsection Title</h3>
```

#### 2. Meta Tags and SEO Elements:

**Title Tags:**
- Presence on every page
- Length between 50-60 characters
- Unique titles for each page
- Dynamic vs static implementation
- Fallback/default titles for dynamic pages
- Check for empty or duplicate titles

**Meta Descriptions:**
- Presence on every page
- Length between 150-160 characters
- Unique descriptions for each page
- Meaningful content (not just repeating title)
- Dynamic generation patterns

**Canonical URLs:**
- Presence of canonical tags
- Self-referencing canonicals
- Proper handling of pagination
- Dynamic route canonical generation

**Open Graph Tags:**
- og:title (required)
- og:description (required)
- og:image (required)
- og:url (required)
- og:type
- twitter:card and related tags

**Other Important Meta Tags:**
- viewport meta tag for mobile
- robots meta tag (index/noindex directives)
- lang attribute on HTML tag
- charset declaration

#### 3. Critical Content Dependencies:

Identify rendering methods for:
- Page titles and headings
- Main body content
- Navigation menus
- Product/article descriptions
- Image alt text
- Internal links

Flag these anti-patterns:
```javascript
// Bad: SEO content loaded client-side
useEffect(() => {
  fetch('/api/product/' + id)
    .then(res => res.json())
    .then(data => {
      setTitle(data.title); // SEO critical
      setDescription(data.description); // SEO critical
      setContent(data.content); // SEO critical
    });
}, []);

// Bad: Content behind user interaction
<button onClick={() => setShowContent(true)}>
  Show Description
</button>
{showContent && <div>{productDescription}</div>}

// Good: Server-rendered content
export async function getServerSideProps({ params }) {
  const data = await fetchProduct(params.id);
  return {
    props: {
      title: data.title,
      description: data.description,
      content: data.content
    }
  };
}
```

#### 4. URL Structure and Routing:

Check for:
- Clean URLs without excessive parameters (/products/shoes not /products?category=shoes&id=123)
- Descriptive slugs (/blog/seo-best-practices not /blog/12345)
- Consistent URL patterns
- Proper handling of trailing slashes
- Case sensitivity issues
- Dynamic URL generation patterns

Flag these patterns:
```
// Bad:
/page?id=123&cat=456&sort=new&filter=true
/Product/DETAIL/12345
/blog/post.php?postid=789

// Good:
/products/running-shoes
/blog/2024/seo-guide
/category/electronics/laptops
```

#### 5. Structured Data (Schema.org):

Check for implementation of:
- Organization schema
- Article/BlogPosting schema
- Product schema with required fields:
  - name
  - description
  - image
  - offers (price, availability)
  - review/aggregateRating
- BreadcrumbList schema
- FAQPage schema where applicable
- Local business schema if applicable

Verify:
- JSON-LD format preferred over microdata
- Valid schema syntax
- Required properties present
- Server-side rendering of structured data

#### 6. Image SEO Implementation:

Check all image elements for:
- Alt text presence and quality
- Descriptive file names (product-name.jpg not IMG_12345.jpg)
- Title attributes where appropriate
- Loading strategy (lazy loading implementation)
- Proper srcset/sizes for responsive images
- Image sitemap references

Code patterns:
```javascript
// Bad:
<img src="IMG_12345.jpg" />
<img src={product.image} alt="" />
<img src={url} alt="image" />

// Good:
<img 
  src="/products/red-running-shoes.jpg"
  alt="Red Nike running shoes with white soles"
  loading="lazy"
  srcset="..."
/>
```

#### 7. Internal Linking Structure:

Analyze:
- Anchor text variety (not just "click here")
- Proper use of rel attributes
- Internal link depth and distribution
- Orphaned pages detection
- Link implementation method (server vs client-side)

#### 8. Framework-Specific SEO Patterns:

**Next.js/React:**
```javascript
// Check for:
- next/head usage vs direct DOM manipulation
- _document.js customization for meta tags
- getStaticPaths for dynamic routes
- generateStaticParams in app directory
- metadata export in app directory
```

**Vue/Nuxt:**
```javascript
// Check for:
- head() method implementation
- asyncData vs mounted for data fetching
- nuxt.config.js global meta settings
- vue-meta usage patterns
```

**Django/Flask (Python):**
```python
# Check for:
- Meta tags in base templates
- Context processors for SEO data
- Sitemap.xml generation
- URL pattern naming and reversal
```

**Rails (Ruby):**
```ruby
# Check for:
- content_for :title implementations
- Meta tags helper usage
- Turbo/Stimulus SEO compatibility
- Proper HTML rendering in controllers
```

**Phoenix (Elixir):**
```elixir
# Check for:
- Meta tags in root layout
- LiveView SEO considerations
- Proper HTML responses
- URL helpers usage
```

### Output Format:

#### Part 1: Detailed SEO Issues Report

**Critical Issues (Must Fix):**

1. **Missing or Multiple H1 Tags**
   - Files affected:
     - /components/ProductDetail.tsx - No H1 found
     - /pages/BlogPost.vue - Contains 3 H1 tags
   - Current implementation: [show code]
   - Impact: Search engines cannot determine main page topic
   - Severity: CRITICAL

2. **Client-Side Rendering of Primary Content**
   - Files affected:
     - /pages/products/[id].js - Product details loaded in useEffect
     - /components/ArticleContent.tsx - Article body fetched client-side
   - Code example:
   ```javascript
   useEffect(() => {
     fetchProductData(id).then(setProduct);
   }, [id]);
   ```
   - Impact: Content invisible to search engines
   - Severity: CRITICAL

3. **Missing Essential Meta Tags**
   - Files affected:
     - /pages/category/[slug].js - No meta description
     - /layouts/BaseLayout.tsx - No Open Graph tags
   - Missing tags: description, og:title, og:image
   - Severity: CRITICAL

**High Priority Issues:**

1. **Improper Heading Hierarchy**
   - Files affected:
     - /components/Sidebar.tsx - H3 used without H2
     - /templates/article.html - Jumps from H1 to H4
   - Pattern: Skipping heading levels for visual styling
   - Severity: HIGH

2. **Non-Descriptive URLs**
   - Route configuration: /config/routes.js
   - Problems:
     - /p?id=123 instead of /products/product-name
     - /blog?post=456 instead of /blog/post-title
   - Severity: HIGH

**Medium Priority Issues:**

1. **Missing Alt Text on Images**
   - Files affected: Multiple component files
   - Pattern: `<img src={url} />` without alt attribute
   - Count: 47 instances found
   - Severity: MEDIUM

#### Part 2: Developer Fix Implementation Guide

**Task 1: Fix H1 Tag Structure**
- Priority: CRITICAL
- Files to modify:
  - /components/ProductDetail.tsx
  - /pages/BlogPost.vue
  - /layouts/MainLayout.jsx

Implementation:
```javascript
// Current (WRONG):
<div className="product-title">{product.name}</div>

// Fixed:
<h1 className="product-title">{product.name || "Product Details"}</h1>
```

**Task 2: Convert to Server-Side Rendering**
- Priority: CRITICAL

For Next.js:
```javascript
// Remove this:
export default function Product({ id }) {
  const [product, setProduct] = useState(null);
  
  useEffect(() => {
    fetchProduct(id).then(setProduct);
  }, [id]);
  
  return <div>{product?.name}</div>;
}

// Replace with:
export async function getServerSideProps({ params }) {
  const product = await fetchProduct(params.id);
  
  return {
    props: { product }
  };
}

export default function Product({ product }) {
  return (
    <>
      <Head>
        <title>{product.name} | Your Store</title>
        <meta name="description" content={product.description} />
      </Head>
      <h1>{product.name}</h1>
      <div>{product.description}</div>
    </>
  );
}
```

**Task 3: Implement Complete Meta Tags**
- Priority: CRITICAL

Create reusable SEO component:
```javascript
export function SEO({ title, description, image, url, type = "website" }) {
  const siteTitle = "Your Site Name";
  const fullTitle = title ? `${title} | ${siteTitle}` : siteTitle;
  
  return (
    <Head>
      <title>{fullTitle}</title>
      <meta name="title" content={fullTitle} />
      <meta name="description" content={description} />
      
      <meta property="og:type" content={type} />
      <meta property="og:url" content={url} />
      <meta property="og:title" content={fullTitle} />
      <meta property="og:description" content={description} />
      <meta property="og:image" content={image} />
      
      <meta property="twitter:card" content="summary_large_image" />
      <meta property="twitter:url" content={url} />
      <meta property="twitter:title" content={fullTitle} />
      <meta property="twitter:description" content={description} />
      <meta property="twitter:image" content={image} />
      
      <link rel="canonical" href={url} />
    </Head>
  );
}
```

### Testing Checklist:
- View page source - ensure all content is visible without JavaScript
- Disable JavaScript in browser - verify content still appears
- Use SEO browser extensions
- Validate with Google Rich Results Test
- Check heading hierarchy with HeadingsMap extension
- Verify all meta tags are present and correct length
- Test canonical URLs on all page types
- Validate structured data with Schema.org validator
- Run Google PageSpeed Insights for Core Web Vitals
- Check XML sitemap is accessible and valid
- Test image loading and alt text presence
- Verify internal links work and use descriptive anchor text
- Check robots.txt is not blocking important pages
- Test different page types (home, category, product, blog)
- Verify 404 pages return proper status code
- Check for duplicate content issues
- Test pagination SEO implementation
- Verify mobile responsiveness

### Common Pitfalls to Avoid:
- Don't rely on client-side routing for SEO-critical pages
- Avoid hiding content behind user interactions
- Don't use JavaScript to inject important meta tags
- Avoid generic or duplicate title/descriptions
- Don't skip heading levels for styling purposes
- Avoid query parameters for main navigation
- Don't forget fallback values for dynamic content
- Avoid blocking search engines in robots.txt during development
- Don't use placeholder alt text like "image" or "photo"
- Avoid orphaned pages with no internal links