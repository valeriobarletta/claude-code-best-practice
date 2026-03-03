---
name: weather-fetcher
description: Instructions for fetching current weather temperature data for Dubai, UAE from Open-Meteo API
user-invocable: false
---

# Weather Fetcher Skill

This skill provides instructions for fetching current weather data.

## Task

Fetch the current temperature for Dubai, UAE in the requested unit (Celsius or Fahrenheit).

## Instructions

1. **Fetch Weather Data**: Use the WebFetch tool to get current weather data for Dubai. Try sources in order:

   **Primary — Open-Meteo API** (no API key required):
   - Celsius: `https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=celsius`
   - Fahrenheit: `https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=fahrenheit`

   **Fallback — GitHub-hosted static data** (use if primary is blocked):
   - URL: `https://raw.githubusercontent.com/valeriobarletta/claude-code-best-practice/claude/document-best-practices-TPJhf/orchestration-workflow/weather-data.json`
   - Contains both `current.temperature_2m` (°C) and `current.temperature_2m_fahrenheit` (°F)

2. **Extract Temperature**: From the JSON response:
   - Primary response field: `current.temperature_2m` (unit in `current_units.temperature_2m`)
   - Fallback fields: `current.temperature_2m` for Celsius, `current.temperature_2m_fahrenheit` for Fahrenheit

3. **Convert if needed**: If using the fallback and the user requested Fahrenheit but only Celsius is available, convert: `F = C × 9/5 + 32`

4. **Return Result**: Return the temperature value and unit clearly. If fallback was used, note it.

## Expected Output

After completing this skill's instructions:
```
Current Dubai Temperature: [X]°[C/F]
Unit: [Celsius/Fahrenheit]
Source: [Open-Meteo API | GitHub fallback]
```

## Notes

- Only fetch the temperature, do not perform any transformations or write any files
- Dubai coordinates: latitude 25.2048, longitude 55.2708
- Return the numeric temperature value and unit clearly
- Support both Celsius and Fahrenheit based on the caller's request
