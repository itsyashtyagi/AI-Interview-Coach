# AI Interview Coach: Requirements (v0.1)

## 1. Purpose

A mobile app where a Flutter or Dart developer practises interview questions, gets rubric-based AI feedback, and tracks progress. It works offline for everything except requesting new AI feedback.

Principle: AI is an enhancement, not a dependency. Opening the app, browsing questions, practising, saving answers and viewing history never require the network or the AI.

## 2. Personas

**Aarav, junior Flutter developer (primary).** Has built small apps, has a first interview in three weeks. Weak on state management and architecture. Wants daily short practice, clear feedback on what he missed, and proof he is improving.

**Meera, career switcher (secondary).** Moving from web development to Flutter. Strong on general programming, new to Dart and the widget tree. Often practises on a commute with poor signal, so offline use matters. Needs behavioral-question practice too.

## 3. MVP features (must have)

| # | Feature | Summary |
|---|---------|---------|

| F1 | Question bank | 40 to 60 original questions across Dart, Flutter fundamentals, widgets and rendering, state management, architecture, testing and behavioral; stored locally with topic, difficulty, type and source. |
| F2 | Practice sessions | Choose topic and difficulty, answer text or multiple-choice questions, move back and forth, finish. |
| F3 | Timer | Optional timer with pause and resume. |
| F4 | AI evaluation | Rubric-based score, strengths, missing concepts, improvements and a sample answer. Validated before display or saving. |
| F5 | Feedback cache | A repeated, unchanged evaluation is reused from local storage. |
| F6 | History | Past sessions, answers and evaluations, available offline. |
| F7 | Progress | Topic performance, score trend, weak-topic suggestion, daily goal. |
| F8 | Privacy controls | Disclosure that answers may be sent to an AI provider; delete all practice history. |

## 4. Features from similar interview-prep platforms

Interview-prep products commonly offer the features below. This table decides which ones we keep. "Should" features are included only if the schedule allows; "Later" features go to `BACKLOG.md`.

| Common feature | Decision | Notes |
|----------------|----------|-------|

| Bookmark questions | Must (F1) | Day 9 |
| Search and filter by topic and difficulty | Must (F1) | Day 9 |
| Timed practice (mock-interview feel) | Must (F3) | Day 13 |
| Detailed explanations and sample answers | Must (F1, F4) | Static explanation for MCQ, AI sample answer for open questions |
| Streaks and daily goal | Should | Day 23 and 27; first to cut if behind |
| Review missed questions (retry incorrect answers) | Should | Reuses the data from F6; add after Day 24 if time allows |
| Daily recommended question or topic | Should | Day 26, from the recommendation rule |
| Dark mode | Should | Free with `ThemeData` on Day 5 if kept simple |
| Behavioral questions with STAR guidance | Must (F2, F4) | Separate rubric on Day 17 |
| Full multi-question mock interview with overall report | Later | Our timed session is a lighter version |
| Spaced repetition scheduling | Later | Needs scheduling logic and more data |
| Voice answers and transcription | Later | Out of scope; many new failure modes |
| Company-specific question sets | Later | Needs licensed content |
| Accounts, cloud sync, leaderboards, sharing | Later | Out of scope; needs a backend |
| Export or share results | Later | |

## 5. User stories and acceptance criteria

Every criterion must be answerable with yes or no by someone who has never seen the code.

**US1. Browse questions offline** (F1)
As a learner, I want to browse and search the question bank without internet, so I can study anywhere.

- With airplane mode on, the question list opens and shows at least 40 questions.
- Choosing a topic filter shows only questions of that topic.
- Choosing a difficulty filter shows only questions of that difficulty.
- Typing a word that appears in a prompt shows matching questions; a word that appears in none shows an empty-state message.
- Bookmarking a question and restarting the app keeps it bookmarked.

**US2. Start a practice session** (F2)
As a learner, I want to start a session by topic and difficulty, so I practise what I need.

- Choosing a topic, a difficulty and a question count starts a session with exactly that many distinct questions, or fewer with a visible message if the bank has fewer.
- If no question matches, the app shows an empty state and does not crash.
- The new session appears in history as "in progress".

**US3. Answer questions** (F2)
As a learner, I want to answer text and multiple-choice questions and move between them, so I can review before finishing.

- A multiple-choice question requires exactly one selected option before submit.
- A text question cannot be submitted when empty.
- Going to the next question and back shows the answer I typed earlier.
- Pressing submit twice quickly saves one attempt, not two.
- After I kill and reopen the app, my submitted answers are still saved.

**US4. Use a timer** (F3)
As a learner, I want an optional timer that I can pause, so I can simulate interview pressure without being punished for interruptions.

- With the timer on, elapsed time increases while the session runs.
- After tapping Pause, the elapsed time stops changing; after Resume, it continues from the same value.
- Sending the app to the background for one minute and returning does not lose or reset the elapsed time.
- With the timer off, no timer is shown.

**US5. Get AI feedback** (F4, F5)
As a learner, I want rubric-based feedback on my answer, so I know what to improve.

- Tapping "Get AI feedback" shows a loading state, then a score between 0 and 10, strengths, missing concepts, improvements and a sample answer.
- A note on the screen says AI feedback is guidance, not a definitive measure.
- If the AI service fails, times out or returns invalid data, a friendly error with a Retry button appears and nothing is saved as a result.
- Requesting feedback again for an unchanged answer shows the stored result immediately and makes no new AI request.
- Changing the answer and requesting again makes a new request.
- The feedback is still visible after restarting the app with airplane mode on.

**US6. Revisit history** (F6)
As a learner, I want to see my past sessions and answers, so I can review what I wrote and what I was told.

- The history list shows completed and in-progress sessions, newest first, with topic, date and score.
- Opening a session shows each question, my answer and its stored evaluation, if any.
- History opens without internet.
- An unfinished session offers Resume and Abandon; Abandon removes it from the in-progress state.

**US7. See progress** (F7)
As a learner, I want to see how I am doing by topic over time, so I know what to practise next.

- With no history, the progress screen shows a friendly "no data yet" state with zeros, not an error.
- After finishing a session, the dashboard totals and topic performance update without restarting the app.
- A topic is shown as "weak" only when it has at least 3 attempts and performance below the threshold, and the screen says why.
- The recommended next topic is shown with a one-line reason.
- The score trend chart shows the most recent sessions in date order.

**US8. Control my data and goal** (F7, F8)
As a learner, I want to set a daily goal and delete my data, so I stay motivated and in control.

- Setting a daily goal of 5 and restarting the app still shows 5.
- The dashboard shows today's progress toward the goal (for example "3 of 5").
- On first use, the app explains that answers may be sent to an external AI provider before any answer is sent.
- "Delete practice history" asks for confirmation, then removes all sessions, attempts and evaluations; history and progress then show empty states.

## 6. Should-have stories (build only if on schedule)

**US9. Review missed questions.** As a learner, I want to retry questions I got wrong, so I fix my weak spots.

- A "Review missed" action lists multiple-choice questions I answered incorrectly and low-scoring open questions.
- Starting it creates a session containing only those questions.

**US10. Streak.** As a learner, I want to see my streak, so I keep practising daily.

- Completing at least one session on consecutive local days increases the streak by one per day.
- Missing a day resets it to 0 (or 1 after practising that day).

## 7. Quality requirements

| Area | Requirement (yes/no testable) |
|------|-------------------------------|

| Persistence | After restart, questions, sessions, attempts, evaluations and settings are all still present. |
| Reliability | Invalid AI output is never shown or saved; every failure type shows a message and offers Retry where it makes sense. |
| Offline | Every feature except a new AI evaluation works with airplane mode on. |
| Privacy | The app contains no raw provider API key; full answers are not written to production logs; history can be deleted. |
| Accessibility | Tappable targets are at least 48 by 48 logical pixels; a full session can be completed with a large system font and with a screen reader. |
| Testing | Pure logic, repositories and BLoCs have automated tests; critical screens have widget tests; CI never calls a live AI model. |
| Delivery | A release build installs from scratch and passes the regression checklist; README, architecture diagram, screenshots and demo exist. |

## 8. Out of scope for the 40-day MVP

- Accounts, cloud sync, social features, leaderboards, recruiter dashboards, subscriptions
- Live voice or video interviews, voice answers and transcription
- A custom backend
- Scraping or copying questions from websites or books; all questions are original or properly licensed
- Spaced repetition, company-specific sets, export or share
- Any promise that AI scores are objectively correct

Add anything else you are tempted to build here, with the date you thought of it:

- (empty)

## 9. Your notes (fill in yourself)

1. In your own words: why is AI an enhancement and not a dependency for this app?
2. Which of the stories above would break if the AI were required to open the app?
3. Which one acceptance criterion above is hardest to verify, and how would you test it?

## 10. Assumptions and open questions

- Target platform: Android first; iOS if a device is available.
- Language: English only.
- Initial topics: Dart, Flutter fundamentals, widgets and rendering, state management, architecture, testing, behavioral.
- Open: default session length (suggested 5 questions) and default daily goal (suggested 5 answers).
- Open: weak-topic threshold (suggested average below 6 out of 10 with at least 3 attempts).
