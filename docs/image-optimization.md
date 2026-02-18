# Image Optimization Guide

This document explains the image optimization implementation for the Data Analyst Portfolio website.

## Overview

The portfolio implements modern image optimization techniques to improve performance and loading times:

1. **WebP format with fallbacks** - Uses modern WebP format with automatic fallback to original formats
2. **Lazy loading** - Images below the fold are lazy-loaded to improve initial page load
3. **Responsive images** - Images scale appropriately for different screen sizes
4. **Error handling** - Graceful fallback to placeholder images if loading fails

## OptimizedImage Component

The `OptimizedImage` component is a wrapper that provides all optimization features:

```tsx
import { OptimizedImage } from '../components/common/OptimizedImage';

<OptimizedImage
  src="/images/projects/my-project.jpg"
  alt="Descriptive alt text"
  loading="lazy"  // or "eager" for above-the-fold images
  className={styles.image}
/>
```

### Props

- `src` (required): Original image URL (JPG, PNG, etc.)
- `webpSrc` (optional): WebP version URL (auto-generated if not provided)
- `alt` (required): Descriptive alt text for accessibility
- `loading` (optional): "lazy" (default) or "eager" for above-the-fold images
- `className` (optional): CSS class name
- `fallbackSrc` (optional): Fallback image if loading fails (default: `/images/placeholder.png`)

### How It Works

The component uses the HTML `<picture>` element to provide format fallbacks:

```html
<picture>
  <source srcSet="/images/project.webp" type="image/webp" />
  <img src="/images/project.jpg" alt="..." loading="lazy" />
</picture>
```

Browsers that support WebP will load the WebP version, while others fall back to the original format.

## Image Preparation

### 1. Original Images

Place original images in the `public/images/` directory:

```
public/
  images/
    projects/
      sales-dashboard.jpg
      customer-segmentation.jpg
      ...
    placeholder.png
```

### 2. WebP Conversion

Convert images to WebP format for better compression. You can use:

**Online tools:**
- [Squoosh](https://squoosh.app/) - Google's image optimization tool
- [CloudConvert](https://cloudconvert.com/jpg-to-webp)

**Command-line tools:**

Using `cwebp` (from WebP tools):
```bash
# Install WebP tools
# macOS: brew install webp
# Ubuntu: sudo apt-get install webp
# Windows: Download from https://developers.google.com/speed/webp/download

# Convert single image
cwebp -q 80 input.jpg -o output.webp

# Batch convert all JPGs in a directory
for file in *.jpg; do cwebp -q 80 "$file" -o "${file%.jpg}.webp"; done
```

Using ImageMagick:
```bash
# Install ImageMagick
# macOS: brew install imagemagick
# Ubuntu: sudo apt-get install imagemagick

# Convert with quality 80
magick input.jpg -quality 80 output.webp
```

### 3. Image Compression

Before converting to WebP, compress original images:

**Recommended tools:**
- [TinyPNG](https://tinypng.com/) - PNG/JPG compression
- [ImageOptim](https://imageoptim.com/) - macOS app
- [Squoosh](https://squoosh.app/) - Web-based

**Target sizes:**
- Project thumbnails: 800x600px, < 100KB
- Project detail images: 1200x900px, < 200KB
- Profile photos: 400x400px, < 50KB

## Usage Examples

### Project Cards (Lazy Loading)

Project cards are below the fold, so they use lazy loading:

```tsx
<OptimizedImage
  src="/images/projects/sales-dashboard.jpg"
  alt="Sales Dashboard showing revenue trends and KPIs"
  loading="lazy"
  className={styles.projectImage}
/>
```

### Hero Images (Eager Loading)

Above-the-fold images should load immediately:

```tsx
<OptimizedImage
  src="/images/hero-background.jpg"
  alt="Data visualization background"
  loading="eager"
  className={styles.heroImage}
/>
```

### Profile Photos

```tsx
<OptimizedImage
  src="/images/profile-photo.jpg"
  alt="Professional photo of John Doe, Data Analyst"
  loading="eager"
  className={styles.profilePhoto}
/>
```

## Current Implementation

### Components Using OptimizedImage

1. **ProjectCard** - Project thumbnail images (lazy loaded)
2. **ProjectDetailPage** - Full-size project images (eager loaded)
3. **Biography** - Profile photo (eager loaded)

### Image Locations

All images referenced in the project:

```
public/images/
  projects/
    sales-dashboard.jpg (and .webp)
    customer-segmentation.jpg (and .webp)
    ab-test.jpg (and .webp)
    covid-trends.jpg (and .webp)
    churn-prediction.jpg (and .webp)
    survey-analysis.jpg (and .webp)
    sentiment-analysis.jpg (and .webp)
  placeholder.png
  avatar-placeholder.svg (already optimized)
```

## Performance Benefits

### Before Optimization
- Original JPG images: ~300-500KB each
- No lazy loading: All images load on page load
- Total page weight: ~3-4MB

### After Optimization
- WebP images: ~100-150KB each (60-70% reduction)
- Lazy loading: Only visible images load initially
- Initial page weight: ~500KB-1MB
- Subsequent loads: Images cached by browser

## Browser Support

- **WebP**: Supported in Chrome, Firefox, Edge, Safari 14+, Opera
- **Lazy loading**: Supported in all modern browsers
- **Fallback**: Older browsers automatically use JPG/PNG versions

## Accessibility

All images include:
- Descriptive `alt` text for screen readers
- Proper semantic HTML structure
- Keyboard navigation support (where applicable)

## Testing

To verify image optimization:

1. **Check WebP loading**: Open DevTools Network tab, filter by "Img", verify WebP files load
2. **Check lazy loading**: Scroll page slowly, watch images load as they enter viewport
3. **Check fallback**: Disable WebP in browser, verify JPG/PNG loads
4. **Check error handling**: Break an image URL, verify placeholder appears

## Future Enhancements

Potential improvements:
- Responsive images with `srcset` for different screen sizes
- Progressive image loading (blur-up effect)
- Automatic image optimization in build process
- CDN integration for faster delivery
- AVIF format support (next-gen format, better than WebP)

## Requirements Validation

This implementation satisfies:
- **Requirement 9.2**: Optimize images and assets for web delivery
- **Requirement 9.4**: Implement lazy loading for images and heavy content
