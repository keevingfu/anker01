# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Marketing analytics dashboard collection for Anker and its sub-brands (SoundCore, Eufy), featuring standalone HTML dashboards that visualize KOL (Key Opinion Leader) performance, advertising campaigns, search analytics, and regional comparisons.

## Architecture

The project uses a self-contained architecture:
- **Standalone HTML files** - Each dashboard is completely self-contained with embedded CSS and JavaScript
- **ECharts 5.4.3** - Primary charting library loaded from CDN
- **No build dependencies** - Open HTML files directly in browser
- **No server requirements** - All data is embedded within files

## Portal System

The project now includes a centralized portal (`index.html`) that provides:
- **Left sidebar navigation** with collapsible two-level menu structure
- **iFrame-based content loading** for seamless dashboard switching
- **Default dashboard**: `anker-selfkoc-search-sessions-analysis.html`
- **Breadcrumb navigation** and refresh functionality

## Dashboard Categories

### KOL/Influencer Analytics
- `anker-kol-performance.html` - KOL performance metrics
- `anker-selfkoc-comparative.html` - Self-operated KOC comparative analysis
- `anker-selfkoc-search-sessions-analysis.html` - Full-funnel impact analysis from social views to search & sessions
- `eufy-selfkoc-performance.html` - Eufy brand KOC performance
- `soundcore-selfkoc-regional-comparison.html` - Multi-country performance comparison (Poland, Norway, Sweden)

### Search & SEO Analytics
- `anker-search-analytics.html` - Search performance dashboard
- `anker-search-nordic.html` - Nordic region search analytics
- `anker-search-volume.html` - Search volume trends

### Campaign & Session Analytics
- `anker-ads-video-june1-july29-2025.html` - Video advertising performance
- `anker-sessions-feb28-july26.html` - Website session analytics

### Social Media Analytics
- `soundcore-social.html` - Social media performance metrics

## Dashboard Components

### Standard Structure
```javascript
// 1. Header with gradient title and date range
// 2. Key metrics cards with hover effects
// 3. Interactive ECharts visualizations
// 4. Data tables with performance indicators
// 5. Strategic insights section
// 6. Footer with generation timestamp
```

### Chart Types Used
- **Pie/Donut**: Platform distribution, market share
- **Bar/Column**: Performance comparisons, time series
- **Line**: Trend analysis, growth metrics, correlation visualization
- **Scatter**: Efficiency analysis, correlation plots, volume vs performance matrix
- **Radar**: Multi-dimensional comparisons, platform efficiency
- **Funnel**: Conversion analysis, regional performance funnels
- **Heatmap**: Regional performance matrices
- **Sankey**: User journey and conversion paths
- **Sunburst**: Multi-touch attribution models

## Video Embedding

The project implements video preview functionality for social media content across multiple platforms. This is used in dashboards like `anker-kol-performance.html` to show top performing content with embedded video players.

### Implementation Pattern

```html
<!-- Video Card Structure -->
<div class="video-card">
    <div class="video-preview-wrapper">
        <iframe src="{embedUrl}" allowfullscreen loading="lazy"></iframe>
    </div>
    <div class="video-info">
        <div class="video-title">Title</div>
        <div class="video-platform">Platform</div>
        <div class="video-stats">Views, Type, etc.</div>
    </div>
</div>

<!-- CSS for Responsive Video Containers -->
.video-preview-wrapper {
    position: relative;
    padding-bottom: 177.78%; /* 9:16 for TikTok/Shorts */
    height: 0;
    overflow: hidden;
}

.video-preview-wrapper.youtube {
    padding-bottom: 56.25%; /* 16:9 for regular YouTube */
}

.video-preview-wrapper iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: none;
}
```

### Platform-Specific Embed URLs

```javascript
// TikTok
https://www.tiktok.com/embed/v2/{videoId}

// YouTube
https://www.youtube.com/embed/{videoId}

// Instagram (requires embed.js script)
// Note: Instagram embeds are more complex and may require server-side processing
<script async src="//www.instagram.com/embed.js"></script>
```

### Video ID Extraction Patterns

```javascript
// YouTube
if (url.includes('watch?v=')) {
    videoId = url.split('watch?v=')[1].split('&')[0];
} else if (url.includes('shorts/')) {
    videoId = url.split('shorts/')[1].split('?')[0];
}

// TikTok
// Video IDs are typically in the URL path
// Example: https://www.tiktok.com/@user/video/7475424968621313311

// Instagram
// Extract from /p/{postId}/ or /reel/{postId}/ URLs
```

### Performance Considerations

- Use `loading="lazy"` on iframes for better performance
- Implement placeholder content for failed embeds
- Consider limiting number of simultaneous video embeds
- Use responsive aspect ratios based on platform

## Responsive Design Patterns

```css
/* Grid layouts auto-adjust */
grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));

/* Charts resize on window change */
window.addEventListener('resize', () => {
    chart.resize();
});
```

## Data Structure Patterns

```javascript
// Platform-specific color schemes
const colors = {
    tiktok: '#000000',
    youtube: '#ff0000',
    instagram: '#e4405f',
    facebook: '#1877f2'
};

// Performance indicators
const performanceLevel = value > threshold ? 'high' : 'low';
```

## Recent Updates (2025-08-05)

### Video Content Analysis Feature
The `anker-kol-performance.html` dashboard has been significantly enhanced with:

1. **Comprehensive Video Preview System**
   - Complete video data from 110+ influencer campaigns
   - Real video URLs from Poland and Nordic regions
   - Support for YouTube, TikTok, and Instagram content
   - Automatic platform detection and embed URL generation

2. **Advanced Filtering and Sorting**
   - Filter by: Region (波兰/北欧), Platform, Product, Influencer
   - Sort by: Views (high/low), Region, Platform, Product
   - Real-time statistics showing total videos, views, and averages
   - Dynamic product dropdown populated from actual data

3. **Enhanced Video Display**
   - Consistent card layout for both "Top Performing Content" and "Video Content Analysis"
   - Clickable influencer names linking to their profiles
   - Color-coded badges for Organic (green) vs Boosted (red) content
   - Responsive grid layout with proper aspect ratios

4. **Video Data Structure**
   ```javascript
   {
       region: '波兰',
       product: 'PL X10 PRO',
       influencer: 'simonlosik',
       influencerUrl: 'https://www.tiktok.com/@simonlosik',
       videoUrl: 'https://www.tiktok.com/@simonlosik/video/7509873656676289814',
       views: 1000000,
       type: 'Boosted'
   }
   ```

### Portal System Updates
- Menu structure simplified: Removed "TikTok Video Preview" from Social Media Analytics
- All dashboards now accessible through unified navigation
- Consistent breadcrumb navigation across all pages

## Key Analytics Insights

### Full-Funnel Analysis
The `anker-selfkoc-search-sessions-analysis.html` dashboard reveals:
- **13.3%** view-to-search conversion rate (exceeds 8-10% industry benchmark)
- **83%** correlation between social media activity and search volume
- **7.2 days** average conversion time from initial view
- **34%** search volume lift after social campaigns

### Regional Performance
- **Poland**: 79.8% of traffic but lowest engagement (0.58%)
- **Nordic Markets**: Higher engagement rates (1.05-1.18%) with lower volume
- **Platform Distribution**: TikTok dominates with 68.5% of views

### Top Performing Content
Based on the influencer marketing data:
- **Highest Views**: iluzjonista_y (PL E25E28) - 1.85M views on TikTok
- **Best Organic Performance**: grafoman_tv (PL X10 PRO) - 932K organic views
- **Platform Leaders**:
  - TikTok: Average 500K+ views for boosted content
  - Instagram: Average 50-100K views for established influencers
  - YouTube: Variable performance, 5K-100K range

## Creating New Dashboards

1. Copy an existing dashboard as template
2. Update title, date range, and data
3. Modify chart configurations in the script section
4. Adjust color schemes to match brand guidelines
5. Test responsive behavior at different screen sizes
6. Add to portal navigation in `index.html`

## Portal Navigation Structure

```javascript
// Navigation categories in index.html:
- KOL & Influencer Analytics (5 dashboards)
- Search & SEO Analytics (3 dashboards)
- Campaign & Session Analytics (2 dashboards)
- Social Media Analytics (1 dashboard)
```