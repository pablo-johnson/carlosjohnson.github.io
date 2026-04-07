# Homepage Carousel Image Guidelines

These recommendations apply to the homepage hero carousel configured in the CMS.

## Recommended Size

- Ideal size: 1600 x 2400 px
- Minimum size: 1200 x 1800 px
- Recommended aspect ratio: 2:3

## Why

The homepage hero is now tuned to favor vertical portrait images in desktop layouts while still using cover cropping. That means images still fill the available area, but the visual box is better suited to portraits and crops less aggressively than before.

## Composition Guidance

- Use vertical portrait images
- Keep the main subject centered or slightly to the right
- Keep the face or instrument relatively high in the frame
- Avoid placing important details too close to the extreme edges
- Leave some safe margin around the subject for cropping on tablet and mobile

## Current Implementation Notes

- The hero image still uses cover behavior, so consistency of aspect ratio matters more than exact pixel dimensions
- The current CSS now gives the image a portrait-friendly panel on desktop and a full-bleed crop on smaller screens

## File Weight

- Target roughly 250 KB to 500 KB per image when optimized
- Prefer WebP when possible, although JPG is still acceptable

## Quick Rule

If you only follow one rule, export each carousel image at 1600 x 2400 px and make sure the subject still looks good when cropped around the center area on smaller screens.
