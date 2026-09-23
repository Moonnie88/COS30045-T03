# COS30045 – T03: TV Energy Data Story (PowerWise)

A three-page website (Home, Televisions, About Us) that tells the story of TV energy consumption in the Australian market, using the Australian Government's Energy Rating data.

**Live demo:** https://cos-30045-t03-six.vercel.app/index.html

## Structure

```
index.html        – Home page
televisions.html  – TV energy data story (KNIME charts + running cost chart)
about.html        – About Us page
css/styles.css    – styling, colours taken from the power logo
js/script.js      – builds the shared nav menu and footer, highlights the current page, switches chart views
images/PowerIcon.png
images/charts/    – KNIME charts answering the 7 TV questions
```

## Features

- JavaScript navigation: `script.js` builds the menu from a list of pages, so every page shares the same menu and footer.
- Power logo in the top-left returns to the Home page.
- Hover feedback: tooltips on nav links, logo and chart tabs, plus colour change (orange background) and logo rotation.
- Current page: each page's `<body data-page="...">` tells the script which nav link gets the `active` class. The browser tab title also shows the page name.
- Colour palette matched to the logo: cream `#f9e5a7`, orange `#eca843`, brown `#7b6344`.
- Footer with year, author name and GenAI acknowledgement.
- Q1 and Q7 charts have tabs to switch between chart views (Q1: pie/bar; Q7: bar/box plot/Spark Electronics sizes).

---

## Data Story

### Audience
**Australian households shopping for a new TV.**

- General public, not data experts: they need plain language, little jargon and one clear message per chart.
- They care about **what a TV will cost to run** and **which TV to buy**, more than watts or technical detail.
- Most will skim, so each chart has a heading that states the takeaway, with the original question shown above it.

### What they want to know (in order of importance)
1. Will a bigger TV increase my power bill? (Q5)
2. Can I get a big TV that is still efficient? (Q6)
3. Does screen technology matter? (Q4)
4. Is one brand more efficient than another? (Q7)
5. What is available in the shops? (Q1–Q3, used as background)

### Guidelines for the visualisation story
- Lead with the key message ("size matters, but check the stars"), then give the evidence.
- Every chart gets a takeaway heading and a caption explaining what to notice.
- Translate power (W, kWh) into **dollars per year**, which is what households understand.
- Point out where a chart could mislead (e.g. OLED TVs are larger on average, brand averages hide wide ranges).
- End with practical actions the reader can take.

### Storyboard
<!-- Add a screenshot of your storyboard (Miro / FigJam / PowerPoint) to images/ and link it here:
![Storyboard](images/storyboard.png) -->

| # | Scene | Visualisation |
|---|-------|---------------|
| 1 | **Issue:** TVs are in almost every home and screens keep getting bigger. What does that cost? | Headline + three key points |
| 2 | **What's in the shops:** mostly LED; 55"–65" most common; a few brands dominate | Q1, Q2, Q3 |
| 3 | **Main finding:** screen size is the biggest driver of power use | Q5 scatter plot |
| 4 | **In dollars:** yearly running cost by screen size | Cost bar chart (HTML/CSS) |
| 5 | **The twist:** star ratings are not tied to size, so efficient big TVs exist; same-size TVs can differ by ~$285/year | Q6 scatter plot + callout |
| 6 | **Smaller factors:** technology and brand matter less than size and stars; Spark Electronics' high average is because it only sells large TVs | Q4, Q7 + Spark Electronics size chart |
| 7 | **Recommendation:** choose the size you need, compare stars within that size, read the label | Three action cards |

---

## About the Data

### Data source
- **Dataset:** Energy Rating Data for Household Appliances – Labelled Products (Televisions)
- **Publisher:** Australian Government Department of Climate Change, Energy, the Environment and Water (DCCEEW)
- **Link:** https://data.gov.au/data/dataset/559708e5-480e-4f94-8429-c49571e82761
- **Downloaded:** 6th September 2026
- **Licence:** Creative Commons Attribution 3.0 Australia (CC BY 3.0 AU)

The data is collected from suppliers when they register TVs for sale in Australia and New Zealand under the Greenhouse and Energy Minimum Standards (GEMS). Key fields used: `Brand_Reg`, `Model_No`, `screensize` (cm), `Screen_Tech`, `Avg_mode_power` (W), `Star Rating Index` and `Labelled energy consumption (kWh/year)`.

### Data processing
<!-- Check this against your KNIME workflow and edit to match the nodes you actually used. -->
Processing was done in KNIME:
1. **CSV Reader:** loaded the dataset.
2. **Math Formula:** converted `screensize` from centimetres to inches (`screensize / 2.54`) as `screensize_inches`.
3. **GroupBy:** counted models by screen technology (Q1), screen size (Q2) and brand (Q3); calculated the median `Avg_mode_power` by screen technology (Q4) and the mean by brand (Q7).
4. **Sorter + Row Filter:** kept the top 10 brands for Q3 and Q7.
5. **Visualisation nodes:** pie and bar charts (Q1–Q4, Q7), scatter plots (Q5, Q6) and a box plot (Q7).
6. **Row Filter + GroupBy (checking an insight):** filtered to Spark Electronics and counted models by screen size, to check why it has the highest average power (all 25 models are 55" or larger).

### Privacy
The dataset describes products, not people. It contains no personal information: only brand names, model numbers, technical specifications and company website links that suppliers submit publicly. No privacy risk was identified.

### Accuracy and limitations
- **Registered, not sold:** the data lists models registered for sale, not sales figures. A model with one sale counts the same as a best-seller.
- **Lab-tested values:** power is measured under a standard test (AS/NZS 62087), not in real homes. Real use depends on brightness settings, content and viewing hours.
- **Labelled energy assumes about 10 hours of viewing a day**, which is more than many households watch, so real costs may be lower.
- **Electricity price is an assumption:** $0.30/kWh is an approximate figure; prices vary by state, retailer and plan.
- **Inconsistent brand names:** the same company can appear under different names (e.g. "SAMSUNG" and "SAMSUNG ELECTRONICS"), which splits their counts.
- **Confounding with size:** OLED TVs are larger on average than LCD TVs, so technology comparisons partly reflect screen size.
- **Brand averages reflect the sizes a brand sells:** e.g. Spark Electronics has the highest average power, but all 25 of its models are 55" or larger (checked in KNIME with a Row Filter + GroupBy by screen size), so this reflects screen size more than efficiency. Brands with few models also give less reliable averages.
- **The dataset changes daily** as models are registered and expire, so counts depend on the download date.

### Ethics
- **Fair to brands:** the story does not recommend or criticise specific brands. It shows that variation within brands is larger than between them and advises comparing individual models.
- **Honest visuals:** bar charts start at zero, and captions point out where a chart could be misread.
- **Transparent assumptions:** the electricity price and viewing hours behind the cost estimates are stated on the page.
- **Accessibility:** every chart has descriptive alt text, and the cost chart has an ARIA label.
- **Attribution:** the data source and licence are credited on the website and in this README, as CC BY 3.0 AU requires.

---

## AI Declaration

I used **Claude Code** (Anthropic) to help build and explain this website. All AI-generated code and content was reviewed by me, and I can explain how it works.

| Date      | Tool        | What I asked for | What I changed / checked |
|-----------|-------------|------------------|--------------------------|
| 23/9/2026 | Claude Code | Build the T01 three-page website (navigation, CSS, footer) and explain the code | Checked the code, changed for preference |
| 23/9/2026 | Claude Code | Add my KNIME charts, chart view tabs and tooltips | Asked for tooltips after it misunderstood mouse-over feedback |
| 23/9/2026 | Claude Code | Split the site into separate HTML pages | Tested all pages and navigation |
| 23/9/2026 | Claude Code | Draft the T03 data story text, running cost chart and README sections | input the real data and do the miro |

The charts (Q1–Q7) were created by me in KNIME.
The miro is done by me and idea taken from claude. After that, prompt claude to implement and enhance the idea.

### Reflection

<!-- Write a short reflection: how helpful was the tool, what did it get wrong, what did you learn? -->
The tool is helpful in perfecting my idea and in fact it is getting even better. It does make a statement without showing the prove as it done the checking at the back but people will prefer to see the evidence. I have learnt to also check and making sure the statement being put out there is true.

