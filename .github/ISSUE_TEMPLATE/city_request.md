---
name: City request
about: Suggest a city to search and display in home
title: "[CITY] XXX request"
labels: enhancement, help wanted
assignees: ''

---

**Add a city yourself**
Steps to add a city yourself:

1. API:
  Add the tournament in [scraping.py](https://github.com/eduayme/chessfind-api/blob/develop/scraping.py) file adding in popularCities [city_name_in_english, [list of all posible names]]. The list of posible names are used to search in the location attribute of tournaments.

2. Frontend:
  Add the tournament in the language translation files. In the [lang folder](https://github.com/eduayme/chessfind-front/tree/develop/lang) add the city english name in "cities" for each file and add a translation. Also add a vertical 350px x 400px photo of the city inside the [assets/cities](https://github.com/eduayme/chessfind-front/tree/develop/assets/cities) folder using the name in english of the city and the extension ".png".
