# Pixel Count Color Mapping - Backup Version

## Overview

**Puzzle File:** `custom_pixel_count_coloring_backup.json`  
**Concept:** Count pixels in each shape and color it based on pixel count  
**Grid Size:** 12x11 to 14x11 (variable)  
**Difficulty:** Medium  
**Training Examples:** 4  
**Test Examples:** 1

---

## Game Description

In this game, you see grey shapes (color 5) on a grid. Count how many pixels each shape has. Then color that shape with the number that matches its pixel count. For example: if a shape has 9 pixels, color it with color 9 (maroon); if it has 4 pixels, use color 4 (yellow); if it has 12 pixels, use color 2 (red) because we use the last digit.

---

## Rules

1. Identify all grey shapes (color 5) in the input grid
2. Count the total number of pixels in each separate shape
3. If pixel count is 1-9: use that number as the color
4. If pixel count is 10 or more: use only the last digit (ones place) as the color
5. Replace the grey pixels of each shape with its corresponding color number
6. Keep all black pixels (color 0) unchanged

---

## Examples

### Training Example 1:
- Shape 1: 3x3 square = 9 pixels → Color 9 (Maroon)
- Shape 2: 2x2 square = 4 pixels → Color 4 (Yellow)
- Shape 3: 1x7 line = 7 pixels → Color 7 (Orange)

### Training Example 2:
- Shape 1: 2x5 rectangle = 10 pixels → 10 mod 10 = 0 → Color 1 (Blue)
- Shape 2: 3x4 rectangle = 12 pixels → 12 mod 10 = 2 → Color 2 (Red)
- Shape 3: Single pixel = 1 pixel → Color 1 (Blue)

### Training Example 3:
- Shape 1: 2x3 rectangle = 6 pixels → Color 6 (Magenta)
- Shape 2: Single pixel = 1 pixel → Color 1 (Blue)
- Shape 3: 1x6 line = 6 pixels → Color 6 (Magenta)

### Training Example 4:
- Shape 1: 2x4 rectangle = 8 pixels → Color 8 (Azure)
- Shape 2: 2x4 rectangle = 8 pixels → Color 8 (Azure)
- Shape 3: 2x3 rectangle = 6 pixels → Color 6 (Magenta)

---

## ARC Color Palette

| Number | Color Name | RGB/Description |
|--------|-----------|-----------------|
| 0 | Black | Background |
| 1 | Blue | Single/10 pixels |
| 2 | Red | 2/12 pixels |
| 3 | Green | 3/13 pixels |
| 4 | Yellow | 4/14 pixels |
| 5 | Grey | Input color |
| 6 | Magenta | 6/16 pixels |
| 7 | Orange | 7/17 pixels |
| 8 | Azure | 8/18 pixels |
| 9 | Maroon | 9/19 pixels |

---

## Algorithm Logic

```
FOR each grey shape (color 5):
    1. Count total pixels in shape
    2. IF count <= 9:
           color = count
       ELSE:
           color = count % 10
           IF color == 0:
               color = 1 (or 0)
    3. Fill all pixels of that shape with 'color'
```

---

## Key Insights

### What Makes This Puzzle Challenging:

1. Requires Counting: Unlike visual pattern puzzles, you must count precisely
2. Object Segmentation: Must identify separate, connected shapes
3. Mathematical Operation: Need to understand modulo for counts > 9
4. Abstract Mapping: Pixel count to color number is not visually intuitive

### Solution Strategy:

- Step 1: Identify all connected grey (5) regions  
- Step 2: Count pixels in each region  
- Step 3: Map count to color (use last digit if > 9)  
- Step 4: Fill region with mapped color  

---

## Test Case Solution

**Input Shapes:**
- Shape 1: 3x3 square = 9 pixels → Color 9
- Shape 2: 4 pixels in row + 2 below = 6 total pixels → Color 4 (Note: These might be separate shapes)
- Shape 3: 2 pixels → Color 4

---

## Important Notes

- Color 5 (Grey) is reserved for input shapes only
- Connected shapes are determined by 4-connectivity (up, down, left, right)
- Diagonal pixels are NOT connected
- For 10+ pixels, the last digit rule applies (12 → 2, 10 → 0 or 1)

---

## Dataset Statistics

- Total Examples: 5 (4 training + 1 test)
- Pixel Count Range: 1 to 12 pixels
- Colors Used: 1, 2, 4, 6, 7, 8, 9
- Shape Types: Rectangles, squares, lines, single pixels

---

## Learning Objective

This puzzle tests the ability to:
- Count accurately (quantitative reasoning)
- Segment objects (spatial reasoning)
- Apply mathematical operations (modulo for large numbers)
- Map abstract properties to visual attributes (count to color)

---

**Created:** 2024  
**File:** `custom_pixel_count_coloring_backup.json`  
**Rule:** pixel_count to color_number (mod 10 if needed)  
**Difficulty:** Medium