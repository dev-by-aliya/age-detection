ageguesser v1.2 — Vercel-ready static site
=========================================

v1.2 changes: the estimator now verifies every model file BEFORE loading the
AI — if a file is missing or arrives empty (0 bytes), the page shows exactly
which file is broken and how to fix it, instead of a cryptic console error.

TROUBLESHOOTING — "AI model file arrived empty (0 bytes)"
This means the models/ folder did not extract completely on your machine.
The site's own model files are intact; the problem is local. Fix:
1. Delete the extracted folder and re-extract the FULL zip again.
2. Check the sizes: models/tiny_face_detector_model.bin ≈ 193KB,
   models/age_gender_model.bin ≈ 430KB. If either is 0 bytes, the unzip
   failed — try a different unzip tool.
3. If the folder lives in OneDrive/Dropbox, make sure the files are
   downloaded locally (not cloud-only placeholders).
4. Hard-refresh the page (Ctrl+Shift+R) and upload again.

WHAT THIS IS
A free "AI guess my age" tool. Visitors upload a selfie and a TensorFlow.js
model running 100% in their browser estimates their age. No backend, no API
keys, no database, no photo uploads to any server.

DEPLOY ON VERCEL (2 minutes)
1. Unzip this file.
2. Go to vercel.com → Add New → Project → upload the unzipped folder
   (or drag-drop it on the Vercel dashboard).
3. Deploy. No build settings needed — it's a static site.

WHAT'S INSIDE
- index.html ......... the whole site (SEO meta, all sections, estimator UI)
- styles.css ......... compiled Tailwind (no runtime CDN)
- js/face-api.js ..... face detection + age estimation library (self-hosted)
- models/ ............ AI model files: tiny face detector + age/gender net
- assets/ ............ logo (favicon.svg), demo-face.svg placeholder
  illustration in the viewfinder — 100% vector, no raster images
- tailwind.config.js + src/input.css ... sources to rebuild styles.css

HOW THE ESTIMATOR WORKS
- First visit: the ~600KB model files download once, then run fully offline.
- Upload (button or drag & drop) → photo shows in the viewfinder → face is
  detected → estimated age appears in the "Apparent Age" badge with an
  approximate range (±3 years) and inference stats.
- JPEG/PNG/WebP, max 15MB. If no face is found, friendly guidance is shown.
- The photo NEVER leaves the visitor's device. Nothing is uploaded anywhere.

REBUILDING styles.css (only if you edit classes in index.html)
  npx tailwindcss -c tailwind.config.js -i src/input.css -o styles.css --minify
  (requires the Tailwind standalone CLI)

TESTED 2026-09-27
- Model load + face detection + age output verified on 2 sample photos
  (group photo → 1 face, age 38.4; portrait → 1 face, age 30.2).
- All element IDs, JS syntax, and local asset paths verified.
