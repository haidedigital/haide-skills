---
name: strategic-technical-seo
description: Comprehensive mastery of technical SEO, site architecture, crawling, indexing, performance optimization, and migrations for premium organic growth strategies. Use when auditing website health, planning migrations or redesigns, debugging indexation or ranking issues, optimizing Core Web Vitals, implementing JavaScript SEO, handling international SEO, specifying technical requirements for developers, or architecting enterprise-level SEO infrastructure. Trigger terms include technical SEO, site architecture, crawl budget, indexing, Core Web Vitals, page speed, mobile-first, schema markup, JavaScript SEO, migration, site move, technical audit, robots.txt, XML sitemaps, canonicalization, hreflang, log file analysis, rendering, server-side rendering, client-side rendering.

trigger_terms_primary:
  - technical SEO
  - site architecture
  - crawl optimization
  - indexing strategy
  - Core Web Vitals
  - page speed optimization
  - technical audit

trigger_terms_contextual:
  - website migration
  - site performance issues
  - ranking drops after redesign
  - pages not indexing
  - crawl budget problems
  - JavaScript rendering issues

trigger_terms_client_language:
  - site is slow
  - Google isn't finding our pages
  - moving to new platform
  - redesigning website
  - technical SEO problems

trigger_terms_premium:
  - enterprise technical SEO
  - large-scale migration
  - technical infrastructure
  - performance optimization for premium sites

related_skills:
  required_foundation: []
  frequently_used_with: [content-strategy-ia, data-measurement, qa-seo-risk]
  advanced_integration: [web-ui-ux-design, tooling-api-literacy]

premium_context: true
complexity_level: advanced
---

# Strategic & Technical SEO Skill

## Philosophy

**Structure is strategy. Technical excellence is the foundation that amplifies every piece of content.**

A perfect technical foundation can make average content perform well. A weak technical foundation will suppress even brilliant content. Technical SEO isn't about checking boxes—it's about creating the optimal environment for search engines to discover, understand, and rank your content.

**Premium context**: At $25k+/month, technical SEO moves from tactical fixes to strategic architecture—designing systems that scale, perform, and dominate.

---

## When to Use This Skill

**Activate this skill when**:
- Auditing website health and identifying technical issues
- Planning website migrations, redesigns, or platform changes
- Debugging sudden ranking drops or traffic losses
- Troubleshooting indexation problems (pages not appearing in search)
- Optimizing site performance (Core Web Vitals, page speed)
- Implementing JavaScript frameworks (React, Vue, Next.js)
- Scaling SEO for large websites (10k+ pages)
- Specifying technical requirements for development teams
- Planning international or multi-regional SEO
- Conducting pre-launch technical QA

**Premium application scenarios**:
- Enterprise-scale technical architecture
- High-stakes migrations ($10M+ organic traffic value)
- Performance optimization for conversion-critical pages
- Technical SEO for complex web applications

---

## Core Competencies

### 1. Site Architecture & Crawl Optimization

**Definition**: Designing site structures and URL hierarchies that search engines can efficiently discover and understand.

**Premium application**: Enterprise sites with 100k+ pages require sophisticated architecture to ensure strategic pages get crawled frequently while low-value pages don't waste crawl budget.

**The Architecture Framework**:

**URL Structure Best Practices**:
```
✅ Good: example.com/category/subcategory/product
- Hierarchical and logical
- Descriptive and readable
- Shows content relationship

❌ Bad: example.com/p=12345&cat=7&ref=xyz
- Parameters and IDs
- Not human-readable
- No clear hierarchy
```

**Internal Linking Strategy**:
- **Priority pages**: 3-5 clicks from homepage
- **Supporting pages**: 4-7 clicks from homepage
- **Archive/low-value**: 7+ clicks (or noindex)

**Crawl Budget Optimization**:

```
Crawl Budget = Server capacity × Crawl demand

Maximize efficiency:
1. Eliminate crawl traps (infinite calendars, filters)
2. Noindex low-value pages (tags, archives, duplicates)
3. Use robots.txt strategically (block admin, search results)
4. Optimize internal linking (strategic pages get more links)
5. Fix redirect chains (direct 301s, not 301 → 301 → 200)
```

**The Pyramid Architecture Model**:
```
Level 1 (Homepage): 1 page - Maximum authority
Level 2 (Categories): 5-10 pages - High authority
Level 3 (Subcategories): 20-50 pages - Medium authority
Level 4 (Products/Articles): 100-10,000+ pages - Distributed authority
```

**Premium considerations**:
- Enterprise sites: Topic clusters and content hubs for authority concentration
- E-commerce: Faceted navigation without creating infinite parameter URLs
- Publishers: Pagination strategies that consolidate authority

---

### 2. Indexing Strategy & Control

**Definition**: Controlling exactly what pages search engines index and how they understand canonical versions.

**Premium application**: Strategic indexing decisions can focus authority on revenue-generating pages while excluding low-value content.

**The Indexing Control Framework**:

**1. XML Sitemaps** (Tell Google what to crawl)
```xml
Best practices:
- Submit only indexable URLs (no noindex, no 301s)
- Segment by content type (products.xml, articles.xml)
- Update dynamically (new pages added automatically)
- Include lastmod dates (helps prioritize crawling)
- Max 50k URLs per sitemap
- Compress large sitemaps (gzip)
```

**2. Robots.txt** (Block unwanted crawling)
```
User-agent: *
Disallow: /admin/
Disallow: /search?
Disallow: /*?filter= # Block filtered URLs
Allow: /products/
Sitemap: https://example.com/sitemap.xml

Disallow: /*?sort= # Block sorting parameters
```

**Strategic blocks**:
- Admin areas (no SEO value)
- Search result pages (thin, duplicate content)
- Filtered/sorted URLs (parameter variations)
- Staging environments (avoid indexing test content)

**3. Canonical Tags** (Consolidate duplicate content)
```html
Problem: Same product on multiple category pages
example.com/shoes/nike-air-max
example.com/running-shoes/nike-air-max
example.com/sale/nike-air-max

Solution: All point to canonical
<link rel="canonical" href="https://example.com/shoes/nike-air-max" />
```

**When to use canonicals**:
- E-commerce: Products in multiple categories
- Pagination: Point to view-all or leave self-referencing
- Parameter URLs: Sort, filter variations
- HTTP vs HTTPS: Canonical to HTTPS
- WWW vs non-WWW: Pick one, canonical to it

**4. Noindex Directive** (Exclude from index entirely)
```html
<meta name="robots" content="noindex, follow" />
```

**When to noindex**:
- Thin content (short, low-value pages)
- Duplicate content (can't canonical)
- Private pages (customer accounts)
- Thank you pages (conversion pages, not search targets)
- Filtered results (faceted navigation variations)

**The Indexing Decision Matrix**:

| Page Type | Index? | Canonical | Notes |
|-----------|--------|-----------|-------|
| Product pages | Yes | Self | Core revenue pages |
| Category pages | Yes | Self | Important landing pages |
| Tag pages | No | N/A | Too thin, noindex |
| Filtered results | No | Category | Duplicate of category |
| Pagination | Yes | Self | Each page indexable |
| Print versions | No | Main page | Canonical to screen version |
| Mobile URLs (m.) | No | Desktop | Canonical to responsive |

---

### 3. Core Web Vitals & Technical Performance

**Definition**: Optimizing page speed and user experience metrics that Google uses as ranking factors.

**Premium application**: For high-value clients, every 100ms of speed improvement can mean millions in revenue. Performance is a competitive advantage.

**The Core Web Vitals**:

**1. Largest Contentful Paint (LCP)** - Loading performance
- **Target**: <2.5 seconds
- **Measures**: Time until largest content element loads
- **Fixes**:
  - Optimize images (WebP, compression, lazy loading)
  - Use CDN for static assets
  - Minimize server response time (TTFB <600ms)
  - Preload critical resources
  - Remove render-blocking JS/CSS

**2. First Input Delay (FID)** - Interactivity
- **Target**: <100ms
- **Measures**: Time from user interaction to browser response
- **Fixes**:
  - Minimize JavaScript execution time
  - Code splitting (load only what's needed)
  - Use Web Workers for heavy processing
  - Defer non-critical JavaScript

**3. Cumulative Layout Shift (CLS)** - Visual stability
- **Target**: <0.1
- **Measures**: Unexpected layout shifts during page load
- **Fixes**:
  - Set explicit width/height on images and videos
  - Reserve space for ads and embeds
  - Avoid inserting content above existing content
  - Use font-display: swap carefully

**Performance Optimization Checklist**:

**Images**:
- [ ] Use next-gen formats (WebP, AVIF)
- [ ] Compress images (80-85% quality)
- [ ] Implement lazy loading (loading="lazy")
- [ ] Use responsive images (srcset)
- [ ] Serve from CDN

**JavaScript**:
- [ ] Minimize and bundle JS files
- [ ] Defer non-critical scripts
- [ ] Code splitting (route-based)
- [ ] Remove unused JavaScript
- [ ] Use async/defer attributes

**CSS**:
- [ ] Inline critical CSS (<14kb)
- [ ] Defer non-critical CSS
- [ ] Remove unused CSS
- [ ] Minimize CSS files

**Server**:
- [ ] Enable HTTP/2 or HTTP/3
- [ ] Enable gzip/brotli compression
- [ ] Use CDN for static assets
- [ ] Implement caching headers
- [ ] Optimize database queries
- [ ] Use server-side caching (Redis, Memcached)

**Premium Performance Targets**:
- **Standard**: LCP <2.5s, FID <100ms, CLS <0.1
- **Premium**: LCP <1.5s, FID <50ms, CLS <0.05
- **Enterprise**: LCP <1s, FID <25ms, CLS <0.01

---

### 4. JavaScript SEO & Rendering

**Definition**: Ensuring search engines can properly crawl and index JavaScript-heavy websites.

**Premium application**: Modern frameworks (React, Vue, Next.js) require sophisticated rendering strategies for optimal SEO.

**The Rendering Spectrum**:

**Client-Side Rendering (CSR)** - Traditional React/Vue
```
Browser downloads HTML → Downloads JavaScript → Executes JS → Renders content

Problems for SEO:
- Google must execute JavaScript (slower indexing)
- Initial HTML is empty (poor for crawlers)
- Slower perceived performance
```

**Server-Side Rendering (SSR)** - Next.js, Nuxt.js
```
Server executes JavaScript → Sends fully-rendered HTML → Browser displays instantly

Benefits for SEO:
- Crawlers see full content immediately
- Faster initial page load
- Better for Core Web Vitals
```

**Static Site Generation (SSG)** - Next.js, Gatsby
```
Build time: Generate static HTML for all pages
Runtime: Serve pre-rendered HTML instantly

Benefits for SEO:
- Fastest possible page loads
- Perfect for crawlers (static HTML)
- Excellent Core Web Vitals
```

**Hybrid Rendering** - Next.js ISR (Incremental Static Regeneration)
```
Combine SSG for most pages + SSR for dynamic content
Best of both worlds for large sites
```

**The JavaScript SEO Decision Matrix**:

| Site Type | Recommended Approach | Why |
|-----------|---------------------|-----|
| Marketing site | SSG (Gatsby, Next.js SSG) | Static content, maximum speed |
| Blog/Publisher | SSG + ISR | Mostly static, occasional updates |
| E-commerce | SSR or Hybrid | Dynamic pricing, inventory |
| Web App | CSR + Pre-rendering | App functionality matters more |
| Enterprise | Hybrid SSR/SSG | Flexibility for different content types |

**JavaScript SEO Checklist**:

- [ ] Implement server-side rendering or pre-rendering
- [ ] Test with Google Search Console (URL Inspection Tool)
- [ ] Ensure critical content in initial HTML
- [ ] Use semantic HTML (not just divs)
- [ ] Implement proper meta tags (rendered server-side)
- [ ] Handle internal linking correctly (no onclick navigation)
- [ ] Use pushState for URL changes (no hash fragments)
- [ ] Lazy load non-critical JavaScript
- [ ] Monitor rendering in Google Search Console

---

## Strategic Approach

### Structure as Competitive Advantage

**Volume approach**: Fix technical issues as they appear

**Strategic approach**: Design technical infrastructure that creates sustainable advantage

**The Technical Moat**:
1. **Superior crawl efficiency** → Faster indexing → Faster rankings
2. **Better performance** → Better UX signals → Higher rankings
3. **Cleaner architecture** → Easier to scale → Compound growth

**Premium positioning**:
```
"We don't just fix technical issues—we architect technical infrastructure
that becomes a competitive moat. Your site will index faster, perform better,
and scale more efficiently than competitors."
```

---

### Log File Analysis (The Ground Truth)

**Why log files matter**:
- Tools show what they think is happening
- Log files show what's actually happening

**What to analyze**:

```
Server logs show:
- Which pages Googlebot actually crawls
- Crawl frequency by page/section
- Wasted crawl budget (404s, redirects)
- Bot behavior patterns
- Server errors (500s, timeouts)
```

**The Log File Analysis Process**:

**1. Collect logs** (30-90 days)
- Webserver logs (Apache, Nginx)
- Separate Googlebot traffic

**2. Analyze crawl distribution**:
```
Question: Is Googlebot crawling strategically important pages?

Check:
- Crawl frequency by URL pattern
- % of crawl budget on priority pages
- Pages never crawled (potential issues)
```

**3. Identify problems**:
- High-value pages crawled infrequently (add internal links)
- Low-value pages over-crawled (noindex or robots block)
- 404s consuming crawl budget (fix or 410 delete)
- Redirect chains (consolidate to direct 301s)

**4. Optimize**:
- Increase internal links to priority pages
- Noindex/block low-value pages
- Fix crawl traps
- Improve server response time

**Tools**:
- Screaming Frog Log File Analyzer
- Botify (enterprise)
- OnCrawl (mid-market)
- Custom scripts (Python, grep)

---

## Detailed Methodologies

### 1. Technical SEO Audit Framework

**The Complete Pre-Flight Checklist**:

**Phase 1: Crawl & Indexing (Foundation)**

- [ ] **Crawlability**
  - Run full site crawl (Screaming Frog, Sitebulb)
  - Identify orphan pages (no internal links)
  - Check robots.txt blocks
  - Verify XML sitemap submission
  - Analyze crawl depth distribution

- [ ] **Indexation**
  - site: search in Google (indexed pages)
  - Compare to expected page count
  - Check for duplicate content
  - Verify canonical implementation
  - Audit noindex usage

- [ ] **Status Codes**
  - Identify 404 errors (broken pages)
  - Find 301 redirect chains
  - Check for 302 temporary redirects
  - Monitor 5xx server errors

**Phase 2: On-Page Technical (Content Layer)**

- [ ] **Meta Data**
  - Audit title tags (length, uniqueness, keywords)
  - Review meta descriptions (CTR optimization)
  - Check missing or duplicate meta tags
  - Verify Open Graph/Twitter Cards

- [ ] **Content**
  - Identify thin content (<300 words)
  - Find duplicate content (same content, different URLs)
  - Check for missing H1 tags
  - Audit heading hierarchy

- [ ] **URLs**
  - Review URL structure (descriptive, hierarchical)
  - Find parameter-based URLs
  - Check URL length (<100 characters ideal)
  - Identify non-HTTPS URLs

**Phase 3: Performance (Speed Layer)**

- [ ] **Core Web Vitals**
  - Test LCP, FID, CLS (PageSpeed Insights)
  - Analyze field data (Chrome UX Report)
  - Test across device types (mobile, desktop)
  - Monitor over time (Search Console)

- [ ] **Page Speed**
  - Check server response time (TTFB)
  - Analyze render-blocking resources
  - Review image optimization
  - Test CDN configuration

**Phase 4: Mobile & Accessibility**

- [ ] **Mobile-First**
  - Verify mobile-friendly design
  - Test mobile usability (Search Console)
  - Check viewport configuration
  - Review tap target sizes

- [ ] **Structured Data**
  - Audit schema markup implementation
  - Test with Rich Results Test
  - Monitor Search Console for errors
  - Identify schema opportunities

**Phase 5: Advanced Technical**

- [ ] **JavaScript**
  - Test rendering (URL Inspection Tool)
  - Check for client-side navigation issues
  - Verify lazy-loaded content is indexable

- [ ] **International**
  - Audit hreflang implementation
  - Check language/region targeting
  - Verify correct domain strategy

- [ ] **Security**
  - Confirm HTTPS implementation
  - Check for mixed content warnings
  - Verify SSL certificate validity

---

### 2. Website Migration Framework (Zero Traffic Loss)

**The Migration Safety Protocol**:

**Pre-Migration (4-8 weeks before)**:

**Week -8 to -6: Audit & Benchmark**
- [ ] Complete technical audit of current site
- [ ] Document all indexed URLs
- [ ] Benchmark current rankings (top 1000 keywords)
- [ ] Record traffic baseline (6 months data)
- [ ] Identify top-performing pages (revenue, traffic)
- [ ] Export all analytics data (permanent backup)

**Week -6 to -4: Planning**
- [ ] Map old URLs to new URLs (complete redirect map)
- [ ] Plan new site architecture
- [ ] Document technical requirements for dev team
- [ ] Set up staging environment
- [ ] Create rollback plan

**Week -4 to -2: Staging & Testing**
- [ ] Build new site on staging
- [ ] Implement 301 redirects on staging
- [ ] Test redirect map (100% coverage)
- [ ] Verify canonical tags
- [ ] Test JavaScript rendering (if applicable)
- [ ] Run technical audit on staging
- [ ] Fix all critical issues on staging

**Week -2 to 0: Final Prep**
- [ ] Create new XML sitemaps
- [ ] Prepare Google Search Console property
- [ ] Update robots.txt
- [ ] Schedule migration during low-traffic period
- [ ] Brief stakeholders on timeline
- [ ] Set up monitoring dashboards

**Migration Day: Launch**

**Hour 0-2: Deploy**
- [ ] Deploy new site to production
- [ ] Implement 301 redirects
- [ ] Update DNS (if domain change)
- [ ] Monitor server stability

**Hour 2-6: Immediate Checks**
- [ ] Test top 50 URLs (manually browse)
- [ ] Verify 301 redirects working
- [ ] Submit new XML sitemaps
- [ ] Check Google Search Console for errors
- [ ] Monitor server logs for errors

**Hour 6-24: Close Monitoring**
- [ ] Run full site crawl
- [ ] Check analytics tracking
- [ ] Monitor ranking fluctuations
- [ ] Watch for 404 errors
- [ ] Track crawl rate in GSC

**Post-Migration (Weeks 1-12)**:

**Week 1-2: Stabilization**
- [ ] Daily ranking monitoring
- [ ] Fix any 404s immediately
- [ ] Monitor organic traffic
- [ ] Check indexation status
- [ ] Review Search Console errors

**Week 3-4: Optimization**
- [ ] Analyze performance changes
- [ ] Optimize redirects (remove chains)
- [ ] Update internal links to new URLs
- [ ] Monitor Core Web Vitals

**Week 5-12: Recovery & Growth**
- [ ] Track traffic recovery to baseline
- [ ] Identify any permanent losses
- [ ] Optimize underperforming pages
- [ ] Create recovery report

**Migration Success Metrics**:
```
Great migration: 0-5% traffic loss (temporary)
Good migration: 5-15% traffic loss (recovers in 4-8 weeks)
Acceptable: 15-25% loss (recovers in 8-12 weeks)
Poor: 25%+ loss or doesn't recover
```

---

### 3. Schema Markup Strategy (Entity Modeling)

**Beyond Basic Schema**: Strategic structured data for rich results and entity understanding.

**The Schema Priority Hierarchy**:

**Tier 1: Essential (Implement First)**
- Organization schema (establishes entity)
- Local Business schema (if applicable)
- WebSite schema (with sitelinks search box)
- Breadcrumb schema (navigation)
- Article schema (for content)

**Tier 2: Rich Results (High ROI)**
- Product schema (price, availability, reviews)
- Review/Rating schema (star ratings in SERPs)
- FAQ schema (expandable FAQ boxes)
- How-To schema (step-by-step results)
- Video schema (video thumbnails)

**Tier 3: Advanced (Competitive Advantage)**
- Event schema (calendar integration)
- Recipe schema (recipe cards)
- JobPosting schema (job listings widget)
- Course schema (educational content)
- Speakable schema (voice search optimization)

**Implementation Best Practices**:

```json
Example: Article Schema (JSON-LD)

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Complete Guide to Technical SEO",
  "image": "https://example.com/article-image.jpg",
  "author": {
    "@type": "Person",
    "name": "Jane Doe",
    "url": "https://example.com/about/jane-doe"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Example Company",
    "logo": {
      "@type": "ImageObject",
      "url": "https://example.com/logo.png"
    }
  },
  "datePublished": "2025-01-15",
  "dateModified": "2025-01-20"
}
</script>
```

**Testing & Validation**:
- Use Google Rich Results Test
- Monitor Search Console (Enhancements)
- Track rich result impressions
- A/B test schema variations

---

### 4. International SEO Implementation

**The Hreflang Strategy**:

**Scenario 1: Multi-Language Site (Same Country)**
```html
Example: Canadian site with English and French

<link rel="alternate" hreflang="en-ca" href="https://example.ca/en/" />
<link rel="alternate" hreflang="fr-ca" href="https://example.ca/fr/" />
<link rel="alternate" hreflang="x-default" href="https://example.ca/" />
```

**Scenario 2: Multi-Regional (Different Countries)**
```html
Example: English site for US, UK, Australia

<link rel="alternate" hreflang="en-us" href="https://example.com/" />
<link rel="alternate" hreflang="en-gb" href="https://example.co.uk/" />
<link rel="alternate" hreflang="en-au" href="https://example.com.au/" />
<link rel="alternate" hreflang="x-default" href="https://example.com/" />
```

**Domain Strategy Decision Matrix**:

| Approach | Example | Pros | Cons | Best For |
|----------|---------|------|------|----------|
| ccTLD | example.co.uk | Strongest geo-signal | Expensive, complex | Large enterprises |
| Subdomain | uk.example.com | Easy to set up | Weak geo-signal | Quick launches |
| Subdirectory | example.com/uk/ | Authority consolidated | Hosting complexity | Most businesses |

**Hreflang Implementation Checklist**:
- [ ] Choose domain strategy
- [ ] Implement hreflang on all pages
- [ ] Use ISO language and country codes
- [ ] Include self-referencing hreflang
- [ ] Add x-default for global fallback
- [ ] Implement bidirectionally (both pages reference each other)
- [ ] Test with hreflang testing tools
- [ ] Monitor in Search Console

---

## Common Scenarios & Solutions

### Scenario 1: Site Redesign Caused Ranking Drop

**Context**: Client launched new website, organic traffic dropped 40%.

**Diagnosis Process**:
1. Check Google Search Console for indexation changes
2. Run site: search to count indexed pages
3. Crawl new site to identify technical issues
4. Compare old vs new URL structure
5. Audit redirect implementation

**Common Causes & Fixes**:

| Problem Found | Solution |
|---------------|----------|
| 404 errors (old URLs) | Implement 301 redirects immediately |
| Canonical to wrong page | Fix canonical tags |
| Noindex left on pages | Remove noindex from live pages |
| Content removed | Restore thin content or 301 to relevant page |
| Slow site speed | Optimize images, minify code, use CDN |
| JavaScript rendering | Implement SSR or pre-rendering |

---

### Scenario 2: Pages Not Indexing

**Context**: New pages published, but not appearing in Google after 4 weeks.

**Diagnosis Checklist**:
- [ ] Check robots.txt (are pages blocked?)
- [ ] Check noindex tag (inspect source code)
- [ ] Verify in XML sitemap
- [ ] Check canonical (pointing to self?)
- [ ] Analyze crawl depth (how many clicks from homepage?)
- [ ] Check server logs (is Googlebot crawling?)
- [ ] Test with URL Inspection Tool (rendering issues?)

**Common Solutions**:
- Add to XML sitemap → Submit in Search Console
- Increase internal links → Reduce crawl depth
- Remove noindex → Allow indexing
- Fix canonical → Self-referencing or remove
- Request indexing → URL Inspection Tool

---

### Scenario 3: Core Web Vitals Failing

**Context**: Site has poor LCP (4.5s), affecting rankings.

**Diagnostic Process**:
1. Test with PageSpeed Insights
2. Analyze field data (real user experience)
3. Identify bottleneck (server, images, JS?)
4. Prioritize highest-impact fixes

**LCP Optimization Checklist**:
- [ ] Optimize server response (TTFB <600ms)
  - Use faster hosting
  - Enable caching
  - Use CDN
- [ ] Optimize images
  - Compress largest image
  - Use WebP format
  - Implement lazy loading (below fold only)
- [ ] Remove render-blocking resources
  - Inline critical CSS
  - Defer non-critical JS
  - Use async for third-party scripts
- [ ] Preload critical resources
  - Preload hero image
  - Preload critical fonts

**Before/After**:
```
Before: LCP 4.5s (Poor)
After optimization: LCP 1.8s (Good)
Impact: +12% organic traffic, +5% conversion rate
```

---

## Integration Points

**Works With:**

- **Content Strategy & IA**: Technical structure enables content to perform
  - Example: Proper hub-and-spoke architecture requires technical implementation

- **Data & Measurement**: Performance tracking requires technical foundation
  - Example: Event tracking, conversion attribution, speed monitoring

- **Web/UI/UX Design**: Performance and user experience overlap
  - Example: CLS issues often require designer + developer collaboration

**Example Integration**:
"When planning a content hub strategy, work with Content Strategy for topical framework, then implement the technical architecture with proper internal linking, schema markup, and crawl optimization."

---

## Premium Application

**How this skill applies to $15-50k+/month engagements**:

### Enterprise Technical SEO

**Scope differences**:
- Standard: Fix technical issues as identified
- Premium: Architect scalable technical infrastructure

**Deliverables**:
- Technical roadmap (12-month optimization plan)
- Migration strategy (risk assessment, rollback plan)
- Performance SLAs (Core Web Vitals targets)
- Technical documentation for dev teams

**Value positioning**:
```
"For a site driving $10M annually in organic revenue, a 10% improvement
from technical optimization = $1M annual impact. Our $40k/month
engagement pays for itself in 2-3 months through performance gains."
```

### High-Stakes Migrations

**Volume agency**: Follow migration checklist
**Premium agency**: Full risk mitigation with guaranteed traffic protection

**Premium migration deliverables**:
- Complete pre-migration audit (200+ point checklist)
- Redirect strategy (100% URL mapping)
- Rollback plan (can revert within hours)
- Daily monitoring (weeks 1-4)
- Traffic insurance (commit to <10% loss)

---

## Quality Standards

**Excellence in technical SEO means**:

**Foundational**:
- Zero critical technical errors
- 100% of priority pages indexable
- Core Web Vitals passing (green)
- Mobile-friendly across all pages

**Advanced**:
- Log file analysis conducted quarterly
- Schema markup on all eligible pages
- Performance monitoring automated
- Technical documentation maintained

**Premium**:
- Technical infrastructure provides competitive moat
- Crawl efficiency optimized (90%+ of budget on priority pages)
- Performance in top 10% of industry
- Zero ranking drops from technical issues

**Premium Quality Checklist**:
- [ ] Core Web Vitals: All metrics in "Good" range (field data)
- [ ] Indexation efficiency: 95%+ of important pages indexed
- [ ] Site speed: LCP <1.5s, FID <50ms, CLS <0.05
- [ ] Crawl efficiency: 90%+ budget on strategic pages
- [ ] Zero broken links on priority pages
- [ ] 100% HTTPS (no mixed content)
- [ ] Mobile parity (mobile = desktop content)
- [ ] Structured data validated (zero errors)

---

## Pitfalls to Avoid

**Common mistakes**:

1. **Over-optimization**: Too many exact-match anchor links, keyword stuffing in meta
   → **Fix**: Natural language, user-focused optimization

2. **Ignoring mobile**: Desktop-focused optimization, mobile afterthought
   → **Fix**: Mobile-first approach, test on real devices

3. **Redirect chains**: 301 → 301 → 301 → 200 (wastes crawl budget, slow)
   → **Fix**: Direct 301s to final destination

4. **Blocking CSS/JS in robots.txt**: Google can't render page properly
   → **Fix**: Allow Google to crawl CSS/JS files

5. **Duplicate content**: Same content on multiple URLs, no canonical
   → **Fix**: Implement canonicalization or consolidate pages

6. **Parameter URLs**: Infinite filter/sort combinations
   → **Fix**: Use robots.txt, canonicals, or noindex

7. **Poor architecture**: Important pages 10+ clicks from homepage
   → **Fix**: Flatten architecture, add internal links

**Premium-specific pitfalls**:
- **Over-engineering**: Building complex solutions for simple problems
  → Focus on highest-impact optimizations first

- **Perfectionism paralysis**: Waiting for 100% perfection before launch
  → Ship at 90%, iterate to 100%

- **Ignoring business context**: Optimizing pages that don't drive revenue
  → Prioritize revenue-generating pages

---

## Key Principles

1. **Structure is strategy** - Technical architecture is competitive advantage
2. **Crawl efficiency matters** - Strategic pages should dominate crawl budget
3. **Performance is ranking factor** - Core Web Vitals impact visibility
4. **Mobile-first always** - Google indexes mobile version first
5. **Rendering matters** - Ensure Google can execute JavaScript if needed
6. **Log files don't lie** - Real bot behavior > tool assumptions
7. **Schema for entities** - Help Google understand content relationships
8. **Migrations require precision** - 100% redirect mapping, zero traffic loss
9. **Speed equals revenue** - Every 100ms improvement impacts conversions
10. **Technical excellence compounds** - Small optimizations accumulate into major advantage

---

## Resources

**Tools**:
- **Crawling**: Screaming Frog, Sitebulb, DeepCrawl
- **Performance**: PageSpeed Insights, WebPageTest, Lighthouse
- **Log analysis**: Screaming Frog Log Analyzer, Botify
- **Schema testing**: Google Rich Results Test, Schema Markup Validator
- **Monitoring**: Google Search Console, Ahrefs, Semrush

**References** (see knowledge base):
- `technical-seo-checklist.md`: Complete 200-point audit checklist
- `migration-playbook.md`: Step-by-step migration guide
- `core-web-vitals-optimization.md`: Detailed performance guide
- `javascript-seo-guide.md`: Rendering strategies and frameworks
- `schema-markup-templates.md`: JSON-LD templates for common schema types

---

*This skill transforms technical SEO from tactical fixes to strategic infrastructure that creates sustainable competitive advantage.*
