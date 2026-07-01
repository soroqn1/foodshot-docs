# FoodShot

A Telegram bot that logs food intake via photo recognition, maps it against 300,000+ USDA records, and exports dietary history in a structured format for medical professionals.

## Architecture

![FoodShot Architecture](foodshot_architecture.svg)

Incoming food photos are processed by OpenAI (GPT-4o) solely to extract the dish name and estimated weight. This textual output is then mapped against the USDA FoodData Central API to retrieve exact macronutrients. This hard boundary is crucial: zero LLM hallucinations for nutrition facts, and zero AI medical advice.

## How the bolus calculation works
No AI involved — pure, transparent math:

```python
def calculate_bolus(carbs, icr, isf, target_bg, current_bg=None):
    carb_dose = carbs / icr
    correction_dose = 0.0
    if current_bg is not None and current_bg > target_bg:
        correction_dose = (current_bg - target_bg) / isf
    return {
        "carb_dose": round(carb_dose, 1),
        "correction_dose": round(correction_dose, 1),
        "total_dose": round(carb_dose + correction_dose, 1),
    }
```

1. **Carb dose** = Carbs ÷ ICR
2. **Correction dose** = (Current BG − Target BG) ÷ ISF, applied only if current BG is above target

## Tech stack

- **Language & Tooling:** Python 3.11–3.13, Poetry, Ruff, Taskfile
- **Bot Framework:** aiogram 3.x (FSM, Handlers, i18n with EN/UK locales)
- **External Services:** OpenAI API (GPT-4o Vision), USDA FoodData Central API
- **Webhooks & API:** FastAPI
- **Database & Cache:** PostgreSQL + async SQLAlchemy, Redis
- **Infrastructure & Hosting:** Docker, Docker Compose, Oracle Cloud, Cloudflare Tunnel (secure webhooks), Custom Domain & DNS

## What it does

- **Food Logging:** Users snap a photo of a meal, and the bot identifies the dish and portion size.
- **Nutritional Tracking:** Automatically calculates Carbohydrates, Calories, Protein, and Fat for the logged meal.
- **Data Export:** Generates structured CSV/Excel reports of meal history and blood glucose records.
- **Bolus Calculator (Optional):** Provides a math-based insulin dose estimate based on user-provided ICR (Insulin-to-Carb Ratio) and ISF (Insulin Sensitivity Factor).

## Limitations

- **Single-Dish Focus:** The current vision pipeline struggles with complex, multi-component meals (e.g., a plate with 4 distinct, unmixed sides). 
- **No CGM Integration:** Continuous Glucose Monitors (Dexcom, Libre) are not yet connected via API; glucose levels must be entered manually.
- **Point-in-Time Accuracy:** Nutritional accuracy depends on the LLM's visual weight estimate, which can deviate by 10-20% depending on the photo angle.
- **Localization:** Dish recognition is highly optimized for globally recognized cuisines, but may lack precision for hyper-local regional dishes.

## Demo

Here’s what the current flow looks like:

![FoodShot Demo](demo.gif)

## ⚠️ Medical disclaimer

FoodShot is a data-logging assistant, not a medical device. The AI component is strictly limited to photo recognition. It does not provide medical advice. All insulin calculations rely on transparent mathematical formulas based on user input. Always consult with a qualified healthcare provider for medical decisions.

## Status

**MVP / Closed Beta.** 
Currently being tested by a community whitelist. Core data pipelines and exports are functional.

---

**Built by:** [soroqn1](https://github.com/soroqn1) · [LinkedIn](https://www.linkedin.com/in/yaroslav-sorochan-8098231b1/)  
*If you're a founder, engineer, or health-tech enthusiast—feel free to reach out.*
