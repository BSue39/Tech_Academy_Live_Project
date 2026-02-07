
# Live Project

## Introduction

During my last two weeks at The Tech Academy's AI Developer Bootcamp, I built a Leverage-themed chatbot. Using Python, spaCy, and Tkinter a Leverage-inspired AI chatbot was created that classifies user intent with NLP, handles unknown/gibberish inputs gracefully, and responds with character-driven dialogue. Being based off of the TV show "Leverage", the chatbot has basic functionality that acts as The Mastermind—calculating, slightly cynical, and always strategic. This project explores natural language processing (NLP), intent classification, model training, and GUI development using spaCy and Tkinter, as well as covers model training, debugging, and UX improvements.

---

## Leverage Mastermind Chatbot Project Overview

This project started as a simple question: *"Can I teach a chatbot to think like the Mastermind from Leverage?"* What followed was an iterative, sometimes frustrating, but deeply rewarding journey through natural language processing, model training, debugging, and UX design.

Over the course of this build, the chatbot evolved from a non-functional shell into a character-driven assistant that could recognize basic intent, handle confusion gracefully, and respond with calculated confidence. Every misclassification, error message, and strange response became a clue—just like a con—pointing to what needed to be fixed next.

The Leverage Mastermind Chatbot is a desktop chatbot application trained to recognize user basic intent and respond in a way that the Mastermind character, inspired by the TV show *Leverage*. It uses a custom-trained spaCy **Text Categorization (textcat)** model to classify user input and generate context-aware responses.

This project focuses on:

* NLP fundamentals
* Training and retraining intent models
* Handling unknown and edge-case inputs
* Debugging and improving model accuracy
* Building a functional GUI

---

## Technologies Used

### Core Technologies

The following were used as a foundation:

* **Python 3.10**
* **spaCy** (NLP + model training)
* **random** (provides functions for performing random operations on sequences)
* **Tkinter** (GUI)
* **Regex (`re`)** (text preprocessing)

---

### Web & UI Foundations

While this project is a desktop application, core **front-end and web development concepts** were applied:

* **HTML** concepts (structuring user-facing content)
* **CSS** concepts (layout, spacing, visual hierarchy)
* **JavaScript** concepts (event-driven thinking)
* **Event listeners** (Tkinter events mirror JS event listeners)
* **Responsive** design principles (window resizing, auto-scroll behavior)
* **Customize** UI behavior and chatbot personality
* **Bootstrap** concepts (component-based UI thinking, even outside the web)

While this project is a desktop application, core **front-end concepts** were applied:

* **HTML / CSS / JavaScript concepts** (layout thinking, event-driven behavior)
* **Responsive design principles** (UI resizing and scroll behavior)
* **Customization** of user experience and chatbot personality
* **Bootstrap concepts** (component-based UI thinking, even outside the web)

---

### Development Tools & Environment

* **Visual Studio Code** (IDE)
* **Terminal** / **Console** for running commands
* Executed **commands** like `python train_model.py`
* **Chrome Developer Tools** concepts applied for debugging UI/event flow
* Extensive **debugging** with printed model scores

---

### Version Control & Collaboration

* **Git** for version control
* Feature branches and commits
* **Pull requests** workflow
* **Merge conflicts** and **merge resolutions**
* Handling **migration conflicts** and **migration resolutions** when retraining models as well as updating models or data
* Understanding **pull requests**, **merge conflicts**, and **merge resolutions**

---

### Software Engineering Practices

* **DRY (Don’t Repeat Yourself)** applied across preprocessing and inference
* Modular design using `import`
* Clear separation of training, UI, and logic
* Applied **CRUD functionality**:
  * **Create** intents and models
  * **Read** user input
  * **Update** training data and responses
  * **Delete** duplicated code
* Modular code organization using `import` statements

* Clear separation of concerns (training vs inference vs UI)

---

### Workflow & Project Management

* **Agile framework** mindset

* **Scrum**-style iterations

* Informal **daily standups**

* Regular **code retrospectives**

* **Azure DevOps** concepts for planning and iteration

* Regular **debugging**, testing, and refinement cycles

* Simulated **daily standups** (tracking progress and blockers)

* End-of-phase **code retrospectives** to improve model accuracy and design

---

## Key Features

* Custom-trained intent classification model
* Leverage-themed persona responses
* Input preprocessing (lowercasing, punctuation removal)
* Unknown / fallback intent handling
* Debug printing of intent confidence scores
* Desktop GUI with Enter-key support

---

### Annotated Code: Intent Prediction & Debugging

```python
# Predict the most likely intent using spaCy's textcat scores
def predict_intent(text):
    doc = nlp(text)
    scores = doc.cats

    # Debugging: print confidence scores for each intent
    print("DEBUG intent scores:", scores)

    # Select the intent with the highest score
    best_intent = max(scores, key=scores.get)
    best_score = scores[best_intent]

    # Confidence threshold for unknown handling
    if best_score < 0.55:
        return "unknown"

    return best_intent
```

---

## Project Structure

```
ChatBot/
│
├── training_data.py        # Reusable, balanced intent training data
├── train_model.py          # spaCy model training script
├── leverage_model/         # Saved trained NLP model
├── Leverage_Chatbot.py     # Tkinter GUI chatbot application
└── README.md               # Project documentation
```

---

## How It Works

1. **Training Phase**

   * Intents are defined in `training_data.py`
   * A spaCy `textcat` pipeline is trained in `train_model.py`
   * Model learns to classify text into intents like `greeting`, `planning`, `crew_roles`, etc.

2. **Chatbot Phase**

   * The trained model is loaded into `Leverage_Chatbot.py`
   * User input is preprocessed and passed into spaCy
   * Predicted intent determines the chatbot response

---

## Example Intents

* `greeting` → "You're either in trouble or planning something."
* `planning` → "Every job begins with recon. Who's the mark?"
* `crew_roles` → "Hitter, hacker, grifter, thief. Everyone plays a part."
* `unknown` → "That doesn't fit any known operation."

---

## What I Learned

This project wasn’t just about getting a chatbot to work—it was about learning how systems *fail*, how models *misunderstand*, and how thoughtful debugging turns confusion into clarity.

---

## What Broke and How I Fixed It

This project involved extensive debugging and iteration. Below are some of the most important issues I encountered and how I resolved them.

### The chatbot always responded with the wrong intent

**Problem:** No matter what the user typed, the chatbot responded with suggestions like "Are you asking about greeting, smalltalk, or other?"

**Cause:** The intent prediction logic compared intent *names* instead of intent *confidence scores*, causing the `unknown` fallback to trigger incorrectly.

**Fix:** I added proper confidence threshold checks using spaCy’s `doc.cats` values and debug printing to verify predictions.

```python
best_intent = max(scores, key=scores.get)
best_score = scores[best_intent]

if best_score < 0.55:
    return "unknown"
```

---

### Gibberish input crashed the chatbot

**Problem:** Entering random text caused a `KeyError: 'other'`.

**Cause:** The model predicted the `other` label, but no response was defined for it in the response dictionary.

**Fix:** I either added an explicit response for `other` *or* routed it through the unknown fallback logic.

```python
if intent not in responses:
    return random.choice(unknown_responses)
```

---

### Greeting inputs triggered the wrong response

**Problem:** Typing "hi" returned a crew-related response instead of a greeting.

**Cause:** The training data was imbalanced, causing the model to overweight certain categories.

**Fix:** I rebuilt the dataset to be fully balanced and added paraphrases and punctuation variants.

---

### Model training errors and crashes

**Problems encountered:**

* Missing spaCy model (`en_core_web_sm`)
* Invalid `Example.from_dict()` annotations
* Shadowed variables (`text` reused in loops)

**Fixes:**

* Installed and validated spaCy pipelines correctly
* Cleaned annotation formats
* Renamed loop variables for clarity and DRY compliance

---

### Outcome

By systematically debugging, printing confidence scores, and retraining with cleaner data, the chatbot became:

* More accurate
* More stable
* Better at handling unknown inputs

This process reinforced real-world debugging, iteration, and model evaluation skills.

---

## What I Learned

* How text categorization works in spaCy
* Why balanced training data matters
* How preprocessing affects model accuracy
* Debugging intent confidence scores
* Designing fallback logic for unknown inputs
* Integrating NLP models into a GUI app

---

## Lessons Learned / Takeaways

### Technical Lessons

* **Balanced data matters more than clever logic**
  Early misclassifications (like responding with `crew_roles` to a simple "hi") were caused by uneven training examples. Adding balanced, diverse samples dramatically improved accuracy.

* **Confidence thresholds are essential in NLP**
  Relying solely on the top predicted intent led to confident but incorrect responses. Introducing score thresholds and fallback logic made the chatbot more reliable and human-like.

* **Preprocessing consistency is critical**
  The model only performed well once training data and runtime input shared the *same preprocessing pipeline* (lowercasing, punctuation removal, spacing normalization).

* **Debug output accelerates learning**
  Printing intent confidence scores (`doc.cats`) turned the model from a black box into something observable and debuggable, enabling faster iteration.

* **Unknown handling is a feature, not a failure**
  Explicitly training and handling an `other/unknown` intent improved user experience and prevented crashes on gibberish input.

### Software Engineering Takeaways

* **Small bugs often signal deeper design issues**
  Errors like `KeyError: 'other'` exposed missing mappings and encouraged more defensive programming.

* **Separation of concerns pays off**
  Keeping training, preprocessing, prediction, and UI code separate made the system easier to debug, retrain, and extend.

* **Iterative development beats perfection**
  The chatbot improved through repeated testing, breaking, and fixing—mirroring real-world Agile workflows.

* **Personality layers sit on top of logic**
  The Leverage-themed responses worked best *after* intent accuracy was solid, reinforcing the idea that style should enhance—not replace—correct behavior.

---

## Potential Future Improvements

* Add typing indicators
* Improve confidence thresholds
* Expand training data with real user inputs
* Add scrolling enhancements and UI polish
* Introduce conversation memory

---

## Author

Built by **BobbySue Fenske**

A hands-on exploration of AI, NLP, and character-driven software design.

---

> "People come to me when they have problems."
