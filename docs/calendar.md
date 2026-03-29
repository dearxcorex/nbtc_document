---
title: Calendar & Tasks
tags:
  - calendar
  - tasks
---

# Calendar & Tasks

View the project schedule and upcoming tasks from Google Calendar.

!!! note "Setup Required"

    Replace the `src` URL below with your own Google Calendar embed URL.
    Go to **Google Calendar** > **Settings** > **Settings for my calendars** > **Integrate calendar** > copy the **Embed code** URL.

<div class="calendar-container">
  <iframe
    src="https://calendar.google.com/calendar/embed?src=YOUR_CALENDAR_ID&ctz=Asia%2FBangkok"
    class="calendar-frame"
    frameborder="0"
    scrolling="no">
  </iframe>
</div>

## How to Update

1. Go to [Google Calendar Settings](https://calendar.google.com/calendar/r/settings)
2. Select your calendar under **Settings for my calendars**
3. Scroll to **Integrate calendar**
4. Copy the **Public URL to this calendar** or the embed `src` URL
5. Replace `YOUR_CALENDAR_ID` in this page with your calendar ID (e.g., `your.email@gmail.com`)

!!! tip "Multiple Calendars"

    You can embed multiple calendars by adding `&src=ANOTHER_CALENDAR_ID` to the URL.
