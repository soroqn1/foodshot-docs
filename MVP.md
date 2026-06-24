# FoodShot — MVP Specification (v0.1)

## 1. Objective
**Primary Focus:** A comprehensive and convenient Telegram diary for food, insulin, and blood glucose, designed to help users find repeating patterns in their daily routine and export data for their doctors.

**Secondary Focus:** Assisting the user in maintaining this diary. 

**Crucial Boundary:** The AI (OpenAI GPT-4o) does *not* maintain the diary, make medical decisions, or calculate insulin. Its *only* job is to recognize food from a photo. All nutritional facts and calculations are strictly powered by official US databases (USDA) and transparent math formulas. The bot does not replace a doctor.

## 2. Core User Flow
1. **Input:** User logs a meal (via photo or text), glucose level, or insulin dose.
2. **AI Assistance (Strictly Limited):** If a photo is provided, OpenAI GPT-4o acts as a vision tool to identify the dish and estimated weight (g).
3. **Reliable Data Fetching:** The system queries authoritative US databases (USDA/Nutritionix) for accurate macronutrients.
4. **Logging & Patterns (Core Value):** The data is logged into the diary. Over time, the system helps identify repeating patterns (e.g., glucose spikes after specific meals).
5. **Calculations (Optional Bonus):** A transparent, math-only formula calculates an insulin bolus estimate based on user-provided ICR/ISF.
6. **Export:** User exports their diary and patterns to share with a medical professional.

## 3. Functional Requirements

### 3.1 Onboarding & Profile (`/start`, `/settings`)
- Collect optional parameters (ICR, ISF, Target BG) only if the user wants the insulin bonus.
- Explicit acceptance of the disclaimer: the bot is an assistant, not a medical professional.

### 3.2 Vision, Analysis & Logging
- **AI Vision:** OpenAI API (GPT-4o) extracts ONLY dish name, estimated weight, and confidence score.
- **Nutrition:** USDA FoodData Central / Nutritionix API for exact Carbohydrates, Calories, Protein, Fat based on the recognized dish.
- **Manual Adjustments:** Users can always adjust weight or correct data before it goes into the diary.

### 3.3 Data Export & Patterns (Priority Feature)
- Ability to generate a structured report (CSV/Excel) of logged meals and glucose levels.
- Clear, readable layout optimized for doctors.
- Basic highlighting of repeating nutritional or glucose patterns.

### 3.4 Calculation Engine (Optional Bonus)
- Transparent formula shown to the user: `Carb Dose = Carbs (g) / ICR` + Correction.
- Completely skippable during the daily flow.

## 4. Commercial Model
- **Freemium Tier:** 2-3 free photo analyses per day.
- **Premium Tier:** ~$2-3 symbolic monthly subscription to support the project and unlock higher limits.

## 5. Scope Boundaries (Out of Scope for MVP)
- Continuous Glucose Monitor (CGM) API integrations (Dexcom, Libre) - will rely on manual input or basic exports first.
- Complex multi-component meal splitting (handling 3+ distinct dishes without grouping).
