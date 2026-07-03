---
name: Casino safe area CSS/JS approach
description: How Royal Casino handles Android/iOS status bar safe area
---

CSS custom properties `--sat` (safe-area-top) and `--sab` (safe-area-bottom) are set by a JS IIFE at startup (before DOMContentLoaded). They default to 32px/8px in CSS `:root` as fallback.

**Why:** `env(safe-area-inset-top)` works on iOS with notches but returns 0 on most Android browsers, even with `viewport-fit=cover` set. The JS IIFE probes via computed style; if env() returns 0, it estimates from `screen.height - screen.availHeight` and clamps to 28-60px.

**How to apply:** All headers use `max(var(--sat), env(safe-area-inset-top, var(--sat)))` padding-top. nav-bar uses `max(var(--sab), env(safe-area-inset-bottom, var(--sab)))` padding-bottom.
