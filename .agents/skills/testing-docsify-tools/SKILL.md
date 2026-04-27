# Testing Docsify Interactive Learning Tools

## Overview
This repo uses [docsify](https://docsify.js.org/) to serve a static documentation site from the `docs/` directory. Interactive learning tools (flashcard, quiz, grammar, pronunciation, progress dashboard) are embedded as inline HTML + `<script>` blocks inside markdown files under `docs/threads/tools/`.

## Running the Dev Server
```bash
cd /home/ubuntu/repos/English-level-up-tips
npx docsify-cli serve docs --port 3000
```
The site is available at `http://localhost:3000`.

## Key Gotcha: Docsify Inline Script Execution
Docsify does NOT execute inline `<script>` tags in markdown files by default. If interactive tools show "Loading..." instead of actual content, check that `docs/index.html` contains a `hook.doneEach()` plugin that manually recreates script elements:

```javascript
hook.doneEach(function () {
  var scripts = document.querySelectorAll('.markdown-section script');
  scripts.forEach(function(oldScript) {
    var newScript = document.createElement('script');
    if (oldScript.src) {
      newScript.src = oldScript.src;
    } else {
      newScript.textContent = oldScript.textContent;
    }
    oldScript.parentNode.replaceChild(newScript, oldScript);
  });
});
```

Without this hook, all JavaScript-based tools will fail silently.

## Testing Procedure

### 1. Flashcard (docs/threads/tools/flashcard.md)
- Navigate to `/#/threads/tools/flashcard`
- Verify card shows a word (e.g., "accomplish") with phonetic transcription — NOT "Loading..."
- Click the card to flip → should show Vietnamese meaning + example sentence
- Click "Tiếp ➡" → card advances, counter updates
- Click "Đã biết" → progress counter increments
- Switch category dropdown → new vocabulary loads

### 2. Quiz (docs/threads/tools/quiz.md)
- Navigate to `/#/threads/tools/quiz`
- Click "Bắt đầu Quiz" → 10 random questions appear
- Select correct answer → green highlight + "Chính xác! ✓"
- Select wrong answer → red on selected + green on correct + "Sai rồi ✗"
- Complete all 10 → score screen with percentage and "Từ cần ôn lại" list

### 3. Grammar (docs/threads/tools/grammar.md)
- Navigate to `/#/threads/tools/grammar`
- Select topic, click "Bắt đầu"
- Verify grammar rule (blue box) is displayed
- Answer a question → explanation text appears regardless of correct/wrong

### 4. Pronunciation (docs/threads/tools/pronunciation.md)
- Navigate to `/#/threads/tools/pronunciation`
- Verify page renders (not "Loading...")
- Note: Recording/playback requires browser microphone access (Web Speech API) — may not work in headless/automated environments

### 5. Progress Dashboard (docs/threads/tools/progress.md)
- Navigate to `/#/threads/tools/progress`
- Verify page renders stats cards and history sections
- Note: Meaningful data only appears after using other tools (flashcard, quiz, grammar) which write to localStorage

### 6. Book Cover Images
- Navigate to `/#/threads/part-1/2-vocabulary`, scroll to "Sách từ vựng đề xuất"
- Verify 3 book covers render (local assets in `docs/assets/`)
- Navigate to `/#/threads/part-1/4-reading`, verify 1 book cover renders
- These images were downloaded locally because external URLs (Douban, Wikipedia) were unreliable

### 7. Homepage Tools Table
- Navigate to `/#/`
- Scroll to "Công cụ luyện tập tương tác" section
- Verify table with 5 tool links is displayed

## localStorage Keys Used
- `fc-known`: Object tracking known flashcard words
- `quiz-history`: Array of quiz attempt records
- `quiz-wrong`: Array of incorrectly answered words
- `gram-history`: Array of grammar exercise records

## Deployment
The site is deployed on Vercel. Configuration is in `vercel.json` (output directory: `docs`). Vercel auto-deploys on merge to `master`.

## Devin Secrets Needed
- `VERCEL_TOKEN`: Required only for manual Vercel deployments (not needed for testing)
