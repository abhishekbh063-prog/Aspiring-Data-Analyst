# Image Optimization Script Guide

This guide provides commands to optimize images for the portfolio website.

## Prerequisites

Install image optimization tools:

### macOS
```bash
brew install webp imagemagick
```

### Ubuntu/Debian
```bash
sudo apt-get install webp imagemagick
```

### Windows
Download and install:
- WebP tools: https://developers.google.com/speed/webp/download
- ImageMagick: https://imagemagick.org/script/download.php

## Batch Optimization Commands

### 1. Compress JPG Images

```bash
# Navigate to images directory
cd public/images/projects

# Compress all JPG files (quality 85, good balance)
for file in *.jpg; do
  magick "$file" -quality 85 -strip "compressed-$file"
done
```

### 2. Convert to WebP

```bash
# Convert all JPG files to WebP (quality 80)
for file in *.jpg; do
  cwebp -q 80 "$file" -o "${file%.jpg}.webp"
done

# Convert all PNG files to WebP
for file in *.png; do
  cwebp -q 80 "$file" -o "${file%.png}.webp"
done
```

### 3. Resize Images

```bash
# Resize project thumbnails to 800x600
for file in *.jpg; do
  magick "$file" -resize 800x600^ -gravity center -extent 800x600 "resized-$file"
done

# Resize detail images to 1200x900
for file in *.jpg; do
  magick "$file" -resize 1200x900^ -gravity center -extent 1200x900 "detail-$file"
done
```

### 4. All-in-One Optimization

```bash
# Optimize and convert in one step
for file in *.jpg; do
  # Compress original
  magick "$file" -quality 85 -strip -resize 1200x900 "optimized-$file"
  # Convert to WebP
  cwebp -q 80 "optimized-$file" -o "${file%.jpg}.webp"
done
```

## Individual Image Optimization

### Single JPG to WebP
```bash
cwebp -q 80 input.jpg -o output.webp
```

### Single PNG to WebP
```bash
cwebp -q 80 input.png -o output.webp
```

### Resize and Optimize
```bash
magick input.jpg -resize 800x600 -quality 85 -strip output.jpg
cwebp -q 80 output.jpg -o output.webp
```

## Quality Guidelines

- **q 90-100**: Highest quality, larger file size (use for hero images)
- **q 80-85**: Good quality, balanced size (recommended for most images)
- **q 70-75**: Lower quality, smaller size (use for thumbnails)

## Recommended Sizes

- **Project thumbnails**: 800x600px
- **Project detail images**: 1200x900px
- **Profile photos**: 400x400px
- **Hero images**: 1920x1080px

## Verification

Check file sizes:
```bash
ls -lh *.jpg *.webp
```

Compare original vs optimized:
```bash
du -h original.jpg optimized.jpg optimized.webp
```

## Online Tools (No Installation Required)

If you prefer not to install command-line tools:

1. **Squoosh** (https://squoosh.app/)
   - Drag and drop images
   - Compare original vs optimized
   - Download WebP versions

2. **TinyPNG** (https://tinypng.com/)
   - Compress PNG and JPG
   - Batch processing available

3. **CloudConvert** (https://cloudconvert.com/)
   - Convert to WebP
   - Batch conversion

## Example Workflow

1. Place original images in `public/images/projects/`
2. Run compression: `magick *.jpg -quality 85 -strip compressed-*.jpg`
3. Convert to WebP: `cwebp -q 80 *.jpg -o *.webp`
4. Verify sizes: `ls -lh`
5. Test in browser: Load page and check Network tab

## Notes

- Keep original images as backup
- Test WebP images in browser before deploying
- Monitor file sizes to ensure they're under target limits
- Use descriptive filenames (e.g., `sales-dashboard.jpg` not `img1.jpg`)
