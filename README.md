# NovaVoice AI: Voice Receptionist for Elderly Patients

A voice AI receptionist for **NovaCare Hospital** (Bengaluru), designed for older adults who live far from their children and are not comfortable with apps. They just call and talk: the agent books a doctor's appointment, points them to the right department, and answers common hospital questions.

Built on **Retell AI** with a GPT-5.1 reasoning layer, tool calling into a live calendar, and safety guardrails for a medical setting.

## Why this exists

Many elderly people manage hospital visits alone. Booking apps and phone menus assume digital confidence they may not have. A patient, calm voice that asks one question at a time removes that barrier.

## What the agent does

- **Books appointments**: collects patient name, department, date and time, confirms them back, then books through a calendar function call
- **Routes symptoms to a department** (General Medicine, Dermatology, Cardiology, Orthopedics, Pediatrics) without diagnosing or prescribing
- **Answers hospital FAQs**: consultation fees, timings, location
- **Escalates emergencies**: chest pain, severe bleeding, unconsciousness, stroke symptoms or breathing difficulty get an immediate "seek emergency care" response, not a booking

## Applied AI and ML skills demonstrated

| Skill | Where it shows up |
|---|---|
| **Prompt engineering** | A scoped system prompt with an explicit task allow-list, tone rules, and hard "never" rules ([prompts/system_prompt.md](prompts/system_prompt.md)) |
| **LLM tool / function calling** | A `book_appointment_cal` function called only once all four required slots are filled, with slot-filling and no re-asking |
| **Safety and guardrails** | No diagnosis, no prescriptions, emergency escalation, "say you don't know" over guessing, never invent calendar slots |
| **Hallucination control** | Availability comes only from the calendar response; hospital facts are fixed in the prompt |
| **Conversation and voice UX design** | Replies under about 25 words, one question per turn, warm greeting, high interruption sensitivity for natural barge-in, DTMF enabled |
| **Failure-mode handling** | Slot unavailable, booking failure, timezone pitfalls (all times in Asia/Kolkata, no conversion) |
| **Evaluation design** | 11 designed test scenarios covering happy path, edge cases and safety ([docs/test-scenarios.md](docs/test-scenarios.md)) |
| **Privacy by configuration** | Post-call PII handling setting, AI-disclosure turned on, secrets kept out of the repo |
| **Observability** | Post-call analysis with GPT-4.1 for reviewing calls |

## Configuration at a glance

| Setting | Value |
|---|---|
| Platform | Retell AI (voice agent) |
| LLM | GPT-5.1, cascading pipeline |
| Voice | retell-Cimo, en-US |
| Interruption sensitivity | 0.9 |
| Max call length | 60 minutes |
| Calendar integration | Cal.com via function calling, Asia/Kolkata |
| Post-call analysis | GPT-4.1 |

The redacted export is in [config/agent_config.redacted.json](config/agent_config.redacted.json). API keys and IDs are removed.

## Iteration notes (v0)

- The prompt has two overlapping "IMPORTANT" booking blocks from successive tuning passes, with slightly different timezone wording. The next version merges them into one block.
- The agent does **not** yet describe individual doctors. The knowledge base is empty. Adding doctor profiles and specialties through Retell's knowledge base is the next step.
- Not yet tested at scale. See the scenarios list for what I plan to verify before publishing.

## Roadmap

- Hindi and regional-language support (a large share of the target users)
- Doctor profiles and availability through a knowledge base
- Appointment reminders and a family-member notification option
- Call-quality dashboard built from post-call analysis

## Security note

The original export contained a live calendar API key. It has been removed from this repo. Set your own key in the Retell dashboard. Never commit real exports: `.gitignore` blocks `Retell*.json`.

## Author

Sanskar Mishra
