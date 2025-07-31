# AI SEO Tools by RivalSee

This is an open-source collection of tools and prompts to help developers identify and fix SEO issues in modern web applications, with a focus on AI-driven SEO optimization.  

It is provided by [RivalSee](https://www.rivalsee.com), a leading AI search visibility platform that monitors and boosts your brands' mentions in ChatGPT, Claude, and Google AI. 


## The Problem: Vibe-Coded Sites and AI SEO

Modern web applications built with AI coding tools are prioritizing speed to develop and deploy and visual appeal over search engine optimization fundamentals, creating significant challenges:

- **Client-side API calls**: Content for many vibe-coding tools is fetched from the client using Supabase, Firebase, or other APIs after page load
- **Poor semantic structure**: Missing or improper heading hierarchies
- **Incomplete meta tags**: Missing descriptions, Open Graph tags, or structured data
- **Non-descriptive URLs**: Query parameters instead of clean, semantic URLs
- **Hidden content**: Important information behind user interactions or lazy loading

**Critical insight**: AI platforms are scraping websites without JavaScript execution. Content retrieved from Supabase calls or other client-side API requests to render content will appear blank to AI crawlers and search engines. If AI can't see your content, your site becomes invisible in the age of AI-powered search.

## Available Tools and Prompts

### AI-SEO Code Analysis Prompt
**File**: [`prompts/code-seo-audit-prompt.txt`](prompts/code-seo-audit-prompt.txt) 


This prompt analyzes your entire codebase to identify technical SEO issues that prevent AI crawlers and search engines from properly reading your content.

**What it finds**:
- HTML heading hierarchy issues (missing H1s, multiple H1s, skipped levels)
- Missing or duplicate meta tags (title, description, Open Graph)
- Client-side rendering of critical content (Supabase calls, API fetches)
- URL structure problems (query parameters, non-descriptive slugs)
- Missing structured data (Schema.org markup)
- Image SEO issues (missing alt text, poor file names)
- Internal linking problems

**How to use**:
1. Copy the entire prompt from [`prompts/code-seo-audit-prompt.txt`](prompts/code-seo-audit-prompt.txt)
2. Paste it into your AI assistant (Claude, ChatGPT, etc.)
3. Provide your project's source code for analysis
4. Receive a detailed report with specific fixes

**Framework support**:
- Next.js/React
- Vue/Nuxt  
- Django/Flask
- Ruby on Rails
- Phoenix/Elixir

**Output includes**:
- Severity ratings (Critical/High/Medium)
- Affected files with line numbers
- Before/after code examples
- Step-by-step implementation guides


## Example: Client-Side API Problem and Fix

❌ **Bad** (Supabase call on client):
```javascript
export default function Product({ id }) {
  const [product, setProduct] = useState(null);
  
  useEffect(() => {
    // AI crawlers never see this content
    supabase
      .from('products')
      .select('*')
      .eq('id', id)
      .then(({ data }) => setProduct(data[0]));
  }, [id]);
  
  return <h1>{product?.name}</h1>;
}
```

✅ **Good** (Server-side data fetching):
```javascript
export async function getServerSideProps({ params }) {
  const { data: product } = await supabase
    .from('products')
    .select('*')
    .eq('id', params.id)
    .single();
    
  return { props: { product } };
}

export default function Product({ product }) {
  return (
    <>
      <Head>
        <title>{product.name} | Your Store</title>
        <meta name="description" content={product.description} />
      </Head>
      <h1>{product.name}</h1>
    </>
  );
}
```

## What This Tool DOES NOT Fix

This tool focuses on technical implementation issues. It does NOT help with:

- **Backlink strategies**: Building domain authority through external links
- **Content creation**: Writing quality, engaging content
- **Keyword research**: Finding the right terms to target
- **Site architecture**: Overall information architecture and user experience
- **Entity-based SEO**: Establishing topical authority and expertise
- **SEO strategy**: High-level planning and competitive analysis
- **Core Web Vitals optimization**: Performance beyond basic rendering
- **Local SEO**: Geographic and location-based optimization
- **Social media integration**: Building social signals
- **Link building campaigns**: Outreach and relationship building

This tool specifically addresses **technical SEO implementation issues** that prevent AI crawlers and search engines from properly reading your content.

## Testing Your Fixes

After implementing fixes, verify with:
1. View page source (content visible without JS)
2. Disable JavaScript (content still appears)
3. Google Rich Results Test
4. Schema.org validator
5. SEO browser extensions
6. Use an AI SEO Visibility tool like [RivalSee](https://www.rivalsee.com) to benchmark your current visibility on ChatGPT etc and watch your AI mentions grow.

## License

MIT License - see [LICENSE](LICENSE) for details



## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Ways to Contribute
- Add new SEO audit prompts
- Share framework-specific solutions
- Submit example fixes
- Improve documentation
- Report issues


