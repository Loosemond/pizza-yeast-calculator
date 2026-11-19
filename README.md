# Pizza Yeast Calculator

A one-page pizza dough calculator. Enter the flour, or the number and weight of dough balls. Set how long the dough stays out of the fridge before, in the fridge, and out of the fridge after. It works out the yeast and the rest of the recipe, and gives a schedule back from the time you want to bake.

It is a single `index.html` with no build step and no dependencies apart from Google Fonts. Open it in a browser.

## Inputs

- **Dough:** flour in grams, or number × weight of dough balls; water, salt and oil as % of the flour; instant dry, active dry or fresh yeast.
- **Fermentation:** room and fridge temperature; hours out of the fridge before, in the fridge, and out of the fridge after. There are presets for same-day, 12 h at room temperature, and 24, 48 or 72 h in the fridge.
- **Starting point:** "Typical recipes" (1.5 × Craig's chart) or "Craig's chart" (the model unchanged).
- **Your adjustment:** −50% to +100%, to correct after a bake that rose too slowly or too fast.

## How the yeast is worked out

The calculator uses Craig's baker's yeast prediction model from the pizzamaking.com forum.

1. **Temperature.** Yeast speed follows the growth curve from Gänzle, Ehmann & Hammes (1998), with Craig's fitted values:

   ```
   rate(T) = a · x^b · e^(c·x),   x = 45 − T (°C)
   a = 0.02645608,  b = 2.037020784,  c = −0.198964236
   ```

   Compared with 21.1 °C, an hour at 4 °C counts 0.10 h, at 6 °C 0.13 h, at 18 °C 0.69 h, at 24 °C 1.37 h and at 30 °C 2.3 h. The curve peaks at about 34.8 °C and is held there above that.

2. **Dough temperature.** Dough does not get cold the moment it goes in the fridge. The calculator steps through the schedule minute by minute and lets the dough temperature move toward the fridge or room temperature with a 1.25 h time constant. Pizzeria guidance is 1.5 to 3.5 hours to cool dough balls, depending on size. Each minute adds `rate(T)` to the effective hours `E`.

3. **Amount.** 0.048% instant yeast is ready in 12 h at 21.1 °C (70 °F):

   ```
   instant yeast % = 0.048 × (E / 12)^−1.6 × starting point × salt factor × (1 + adjustment)
   ```

   With these values the model gives 0.63% for 24 h in a 4 °C fridge (Craig's chart: 0.64%) and 0.21% for 48 h (chart: 0.20%).

4. **Salt.** Belz et al. (2017) measured total gas over 3 hours at different salt levels. The calculator takes Craig's dough as 2.8% salt and scales the yeast by the measured gas ratio: × 0.72 at 2% salt, × 1.08 at 3%, × 1.29 at 3.5%. The test ran 3 hours at 30 °C, so in a long cold rise the effect may be smaller.

5. **Yeast type.** Fresh : active dry : instant = 1 : 0.4 : 0.33, so active dry is 1.2 × instant and fresh is 3 × instant.

Under 1 g of yeast, the page explains how to dissolve 1 g in some of the recipe water and add only part of it.

## Compared with published recipes

Instant yeast as % of flour, with 21 °C room, 4 °C fridge and 2.8% salt:

| Schedule | Typical recipes | Craig's chart | Published |
|---|---|---|---|
| 12 h at 22 °C | 0.061% | 0.041% | 0.07–0.08% (Gigacalculator) |
| 24 h at 22 °C | 0.020% | 0.013% | 0.025% (Gigacalculator) |
| 2 h + 48 h at 4 °C + 3 h | 0.12% | 0.077% | 0.10–0.20% (Dough Lab, Impasto, My Pizza Night, Fond) |
| 1 h + 72 h at 4 °C + 3 h | 0.092% | 0.061% | 0.05–0.12% (same sources) |

The 1.5 × factor for "Typical recipes" is a calibration against these published amounts, not part of Craig's model.

## Limits

- The amount of water in the dough is not part of the formula. No study on it was found.
- Fridges vary. A 6 °C fridge ferments a third faster than a 4 °C one, so check yours with a thermometer.
- Sugar, oil and preferments (poolish, biga) are not modelled.

## Sources

- TXCraig1, [Baker's yeast quantity prediction model](https://www.pizzamaking.com/forum/index.php?topic=26831.0), pizzamaking.com. The fitted values are as implemented in [Dough Designer Pro](https://github.com/Alexbl00m/dough-designer-pro/pull/2), which checks them against the chart.
- Gänzle, Ehmann & Hammes (1998), [Modeling of growth of *Lactobacillus sanfranciscensis* and *Candida milleri* in response to process parameters of sourdough fermentation](https://doi.org/10.1128/aem.64.7.2616-2623.1998), Applied and Environmental Microbiology 64:2616–2623.
- Belz, Axel, Beauchamp, Zannini, Arendt & Czerny (2017), [Sodium chloride and its influence on the aroma profile of yeasted bread](https://doi.org/10.3390/foods6080066), Foods 6:66, Table 2.
- Tom Lehmann, cross-stacking times for dough balls, [Pizza Today](https://pizzatoday.com/?p=130064).
- Published amounts: [Gigacalculator](https://www.gigacalculator.com/calculators/pizza-dough-calculator.php), [Dough Lab](https://dough-lab.com/blog/cold-ferment-pizza-dough), [Impasto](https://impasto.pizza/guides/cold-ferment), [My Pizza Night](https://mypizzanight.com/guides/cold-fermentation-pizza-dough), [Fond](https://fond.kitchen/guides/pizza-dough/pizza-dough-fermentation/).
