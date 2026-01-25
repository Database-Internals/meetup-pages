# Future Automation Opportunities

This document outlines potential automation improvements for the meetup-pages workflow. The manual process remains the current standard, but these options are available if the workflow grows or complexity increases.

## What Could Be Automated

### 1. **Video Embed Generation (Low Effort, High Value)**

**Current workflow**: Manually create iframe and link HTML for each video platform.

**What to automate**:
- Extract video IDs from YouTube and VK URLs
- Generate HTML with correct iframe attributes and Font Awesome icons
- Insert generated HTML into the post

**Implementation approach**:
```python
# Example: tools/generate_embeds.py
import re

def youtube_embed(url):
    """Generate YouTube embed HTML from URL"""
    video_id = extract_youtube_id(url)
    return f'''<iframe width="560" height="315" src="https://www.youtube.com/embed/{video_id}"
    title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write;
    encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin"
    allowfullscreen></iframe>
<p>
  <a href="https://youtu.be/{video_id}" target="_blank" rel="noopener noreferrer">
    <i class="fab fa-youtube"></i> YouTube
  </a>
</p>'''

def vk_embed(url):
    """Generate VK embed HTML from VK video URL"""
    oid, vid = extract_vk_ids(url)
    return f'''<iframe src="https://vkvideo.ru/video_ext.php?oid={oid}&id={vid}&hd=2&autoplay=1"
    width="560" height="315" style="background-color: #000" allow="autoplay; encrypted-media;
    fullscreen; picture-in-picture; screen-wake-lock;" frameborder="0" allowfullscreen></iframe>
<p>
  <a href="https://vkvideo.ru/video{oid}_{vid}" target="_blank" rel="noopener noreferrer">
    <i class="fab fa-vk"></i> VK
  </a>
</p>'''

# Usage: python tools/generate_embeds.py --youtube "https://youtu.be/mcx9ckGy1gA" --vk "https://vk.com/video-226977842_456239030"
```

**When to use**: Saves ~5 minutes per meetup. Reduces copy-paste errors.

### 2. **Post Template from Structured Data (Medium Effort, Medium Value)**

**Current workflow**: Manually create markdown from TimepadIn event details.

**What to automate**:
- Define event details in YAML/JSON (speaker names, times, descriptions)
- Generate complete markdown post from template
- Handle different event types (standard meetup, ISPRAS Open, online-only)

**Implementation approach**:
```yaml
# _data/events/meetup-11.yml
number: 11
date: 2026-02-15
time: "19:00"
timezone: "+0300"
type: "standard"  # or "ispras-open", "online-only"
location: "Yandex Office"
description: |
  Eleventh meetup of the Database Internals community.

talks:
  - time_start: "19:00"
    time_end: "20:00"
    title: "Talk Title"
    speaker: "Speaker Name"
    position: "Role at Company"
    description: |
      Detailed description of the talk.

  - time_start: "20:10"
    time_end: "21:10"
    title: "Second Talk"
    speaker: "Another Speaker"
    position: "Position"
    description: |
      Another talk description.

videos:
  youtube: "https://www.youtube.com/watch?v=..."
  vk: "https://vk.com/video-..."
```

Then use a Jekyll plugin or Python script to generate the markdown post.

**When to use**: Useful if event structure becomes more complex or if multiple properties need updating.

### 3. **Video ID Extraction from URLs (Low Effort)**

**Problem**: URLs have different formats:
- YouTube: `https://youtube.com/watch?v=ID`, `https://youtu.be/ID`
- VK: `https://vk.com/video-OID_VID`, `https://vkvideo.ru/video-OID_VID`

**Solution**: Standardized extraction functions

```python
import re

def extract_youtube_id(url):
    """Extract YouTube video ID from various URL formats"""
    patterns = [
        r'(?:youtube\.com\/watch\?v=|youtu\.be\/)([a-zA-Z0-9_-]{11})',
        r'youtube\.com\/embed\/([a-zA-Z0-9_-]{11})',
    ]
    for pattern in patterns:
        match = re.search(pattern, url)
        if match:
            return match.group(1)
    raise ValueError(f"Could not extract YouTube ID from {url}")

def extract_vk_ids(url):
    """Extract VK video OID and VID from URL"""
    pattern = r'(?:vk\.com/video|vkvideo\.ru/video)-(\d+)_(\d+)'
    match = re.search(pattern, url)
    if match:
        return match.group(1), match.group(2)
    raise ValueError(f"Could not extract VK IDs from {url}")
```

### 4. **GitHub Actions Workflow for Video Updates (Medium Effort)**

**Concept**: Create a GitHub Actions workflow that:
1. Accepts video URLs as workflow input or comment trigger
2. Generates embed HTML
3. Updates the post file
4. Creates a pull request for review

```yaml
name: Add Video Embeds
on:
  workflow_dispatch:
    inputs:
      meetup_number:
        description: 'Meetup number'
        required: true
      youtube_url:
        description: 'YouTube URL'
        required: false
      vk_url:
        description: 'VK video URL'
        required: false

jobs:
  update-post:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.9'
      - run: python tools/generate_embeds.py --meetup ${{ inputs.meetup_number }} --youtube "${{ inputs.youtube_url }}" --vk "${{ inputs.vk_url }}"
      - uses: peter-evans/create-pull-request@v4
        with:
          commit-message: "Add video embeds for Meetup #${{ inputs.meetup_number }}"
          title: "Add video embeds for Meetup #${{ inputs.meetup_number }}"
```

**When to use**: Useful for distributed team or if video links arrive asynchronously.

### 5. **TimepadIn Web Scraping (High Effort, Uncertain ROI)**

**Current blocker**: TimepadIn has no public API.

**What would be needed**:
1. Reverse-engineer TimepadIn HTML structure (changes when site updates)
2. Implement robust HTML parsing with Cheerio/BeautifulSoup
3. Handle dynamic content (JavaScript rendering)
4. Map TimepadIn data to post template format
5. Implement fallback for parsing failures
6. Maintain script when TimepadIn changes layout

**Challenges**:
- TimepadIn structure may change without notice
- Parsing dynamic content requires headless browser (Puppeteer/Selenium) - slow, resource-intensive
- Russian text encoding edge cases
- Legal: check TimepadIn terms of service for scraping permission
- Manual review still needed for accuracy (Russian text, speaker credentials, etc.)

**Cost-benefit**:
- Saves: ~15 minutes per event (copying event details)
- Costs: ~4-6 hours initial development + ongoing maintenance
- At 12 events/year, payback period is ~2 years without failures
- Not recommended unless event frequency increases significantly or team grows

## Recommended Path Forward

### Phase 1 (Immediate, Lowest effort)
- Create `tools/generate_embeds.py` script for video HTML generation
- Document usage in this file
- Usage: Run script once per meetup, copy output to post

### Phase 2 (If needed)
- Create event YAML templates in `_data/events/`
- Create Jekyll Liquid template or Python script to generate posts from YAML
- Allows data reuse for multiple formats (posts, indexes, JSON API, etc.)

### Phase 3 (If team grows or frequency increases)
- Implement GitHub Actions workflow for workflow_dispatch video updates
- Optional: Add issue/comment-based trigger for team coordination

### Phase 4 (Only if justified by scale)
- Reconsider TimepadIn scraping with clear requirements and budget
- Alternatively: Request TimepadIn to provide structured event export
- Or: Maintain manual TimepadIn monitoring but automate post generation from provided data

## Implementation Notes for Future

**Testing automation tools**:
- Create test posts in a `_drafts/` directory before committing
- Verify HTML rendering with `bundle exec jekyll build`
- Check local server: `bundle exec jekyll serve --drafts`
- Validate iframe embeds work in browser (test both YouTube and VK playback)

**Handling edge cases**:
- Some speakers may have special characters in names (ё, ж, ш, etc.) - test UTF-8 handling
- TimepadIn events sometimes change time/speakers last-minute - maintain manual verification step
- Video URLs may be unavailable immediately after event - implement scheduled retry logic

**Maintenance**:
- Document any custom scripts in `tools/README.md`
- Include usage examples and error handling
- Keep regex patterns for URL extraction updated if YouTube/VK change URL formats
