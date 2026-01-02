# Data Structure Documentation

## Overview
The project uses a hierarchical JSON-based data structure to organize art projects and their associated images.

## Directory Structure

```
public/images/
├── projects.json                 # Master index of all projects
├── dragon-mask/
│   ├── project.json             # Project metadata
│   └── IMG_*.jpg               # Project images
├── franko-rabbit-mask/
│   ├── project.json
│   └── IMG_*.jpg
├── digital-art/
│   ├── project.json
│   └── IMG_*.jpg
├── sculpting/
│   ├── project.json
│   └── IMG_*.jpg
├── mural-painting/
│   ├── project.json
│   └── IMG_*.jpg
├── el-mokhtoot-2021/
│   ├── project.json
│   └── IMG_*.jpg
└── el-manfa-2023/
    ├── project.json
    └── IMG_*.jpg
```

## File Formats

### 1. Master Index: `projects.json`
Located at `public/images/projects.json`, this file contains summary data for all projects.

**Schema:**
```json
{
  "projects": [
    {
      "id": "string",                      // Unique identifier (kebab-case)
      "titleEn": "string",                 // English title
      "titleAr": "string",                 // Arabic title
      "category": "string",                // Category type (mask-design, digital, theater, sculpture, mural)
      "year": "number",                    // Year created
      "descriptionEn": "string",           // Short English description
      "descriptionAr": "string",           // Short Arabic description
      "thumbnail": "string",               // Path to thumbnail image
      "imagesCount": "number",             // Total number of images
      "featured": "boolean"                // Whether to feature on homepage
    }
  ]
}
```

**Example:**
```json
{
  "id": "dragon-mask",
  "titleEn": "Dragon Mask",
  "titleAr": "قناع التنين",
  "category": "mask-design",
  "year": 2024,
  "descriptionEn": "Handcrafted dragon mask with intricate scale details.",
  "descriptionAr": "قناع تنين مصنوع يدويًا بتفاصيل دقيقة.",
  "thumbnail": "images/dragon-mask/IMG_20240531_230911.jpg",
  "imagesCount": 3,
  "featured": true
}
```

### 2. Individual Project: `project.json`
Each project folder contains a `project.json` file with detailed project information.

**Schema:**
```json
{
  "id": "string",                          // Unique identifier
  "titleEn": "string",                     // English title
  "titleAr": "string",                     // Arabic title
  "category": "string",                    // Project category
  "year": "number",                        // Year created
  "month": "string",                       // Month created (optional)
  "descriptionEn": "string",               // Detailed English description
  "descriptionAr": "string",               // Detailed Arabic description
  "materials": ["string"],                 // List of materials used
  "director": "string",                    // Director (theater projects only)
  "featured": "boolean",                   // Featured on homepage
  "images": [                              // Array of images
    {
      "fileName": "string",                // Image filename
      "altEn": "string",                   // English alt text
      "altAr": "string",                   // Arabic alt text
      "order": "number"                    // Display order (1-indexed)
    }
  ]
}
```

**Example:**
```json
{
  "id": "dragon-mask",
  "titleEn": "Dragon Mask",
  "titleAr": "قناع التنين",
  "category": "mask-design",
  "year": 2024,
  "month": "May",
  "descriptionEn": "Handcrafted dragon mask with intricate scale details...",
  "descriptionAr": "قناع تنين مصنوع يدويًا بتفاصيل دقيقة...",
  "materials": ["Foam", "Paint", "Leather"],
  "featured": true,
  "images": [
    {
      "fileName": "IMG_20240531_230911.jpg",
      "altEn": "Front view of dragon mask",
      "altAr": "منظر أمامي لقناع التنين",
      "order": 1
    }
  ]
}
```

## Projects

### Current Projects (7 total)

| ID | Title (EN) | Title (AR) | Category | Year | Images | Featured |
|---|---|---|---|---|---|---|
| dragon-mask | Dragon Mask | قناع التنين | mask-design | 2024 | 3 | ✓ |
| franko-rabbit-mask | Franko Rabbit Mask | قناع الأرنب فرانكو | mask-design | 2024 | 3 | ✓ |
| digital-art | Digital Art Collection | مجموعة الفن الرقمي | digital | 2023 | 7 | ✓ |
| el-manfa-2023 | El Manfa 2023 | المنفي 2023 | theater | 2023 | 3 | - |
| sculpting | Sculpting | النحت | sculpture | 2023 | 2 | - |
| el-mokhtoot-2021 | El Mokhtoot 2021 | المخطوطة 2021 | theater | 2021 | 2 | - |
| mural-painting | Mural Painting | لوحة جدارية | mural | 2020 | 1 | - |

## Adding New Projects

To add a new project:

1. **Create a new folder** in `public/images/` with a kebab-case name:
   ```bash
   mkdir public/images/my-project-name
   ```

2. **Add images** to the folder:
   ```bash
   cp /path/to/images/* public/images/my-project-name/
   ```

3. **Create `project.json`** in the folder with complete project details

4. **Update `projects.json`** with a new entry for quick reference:
   - Add to the `projects` array
   - Include thumbnail path, image count, and featured status

## Categories

- `mask-design` - Mask and costume design
- `digital` - Digital art and illustrations
- `theater` - Theater production design
- `sculpture` - Sculptural works
- `mural` - Wall art and murals

## Frontend Integration

### Load All Projects
```javascript
const response = await fetch('/images/projects.json');
const data = await response.json();
```

### Load Single Project
```javascript
const response = await fetch(`/images/${projectId}/project.json`);
const projectData = await response.json();
```

## Bilingual Support

All content supports both English and Arabic:
- `titleEn` / `titleAr` - Project titles
- `descriptionEn` / `descriptionAr` - Project descriptions
- `altEn` / `altAr` - Image alt text

Use language preference to display appropriate content.
