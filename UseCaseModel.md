# Use Case Model

**Author**: Faizan Bhatti

The Job Offer Comparison App is a single-user application that lets a Job Seeker record their current job and any number of job descriptions and offers, weigh the factors that matter to them, and compare two jobs side by side, ranked by an overall score that accounts for benefits and cost of living.

The application is organized into three tabs:

- **Comparison Settings** — assign the weight given to each comparison factor.
- **Job Descriptions & Offers** — add, edit, and view job descriptions, mark which job is the current job, and record offers received.
- **Compare Jobs** — view all jobs ranked by score and compare two side by side.

A **job description** is the base record for any job the user is tracking. A description may additionally be flagged as the **current job** (the job the user holds today) or marked **offer received** once a real offer arrives, so the "Offer Received" column on the Job Descriptions & Offers tab reads *Yes*, *No*, or *Current Job* for each row.

---

## 1. Use Case Diagram

<img width="933" height="658" alt="image" src="https://github.com/user-attachments/assets/57d0bae2-a51e-431a-84c2-3861b04b9837" />


The Job Seeker is the single actor and directly initiates four use cases: Enter or Edit Current Job, Enter Job Offer, Adjust Comparison Settings, and Compare Job Offers. Compare Job Offers **includes** Rank Job Offers, which the system performs as part of every comparison, and Enter Job Offer **extends** Compare Job Offers, since after saving an offer the user may move directly into a comparison.

---

## 2. Use Case Descriptions

### <u>Use Case 1 — Enter or Edit Current Job</u>

**Requirements**: From the Job Descriptions & Offers tab, the user must be able to enter for the first time or edit all details of a job — title, company, location (city and state), cost of living, salary, bonus, paternity leave, holidays, gym reimbursement, and pet insurance — and flag that job as the current job using the Current Job (No/Yes) control. The user must be able to save or cancel.

**Pre-conditions**: The application is running and the Job Descriptions & Offers tab is displayed.

**Post-conditions**: If saved, the job is stored (or updated) and, if flagged, recorded as the current job; any previously current job is no longer marked current. If cancelled, no data changes. In both cases the user remains on the Job Descriptions & Offers tab.

**Scenarios**:

- **Normal (first time)**: User selects "Add Job Description" → system shows the empty Add Job Description Dialog → user fills all fields → user selects "Save Job Description" → system validates the values are within range and stores the job → the new job appears in the job list. The user then selects that row, sets Current Job to Yes in the Edit Job Description panel, and selects "Save Changes."
- **Alternate (editing)**: User selects an existing job's row → the Edit Job Description panel shows its values → user changes fields and/or the Current Job flag → "Save Changes" → system updates the stored job.
- **Alternate (cancel)**: User selects "Cancel and Return" in the dialog, or "Cancel and Reset" in the edit panel → no changes saved.
- **Exceptional (invalid value)**: User enters a value outside its range → system rejects it and prompts for correction → user corrects or cancels.

### <u>Use Case 2 — Enter Job Offer</u>

**Requirements**: The user must be able to add a job description and record that an offer has been received for it, entering all offer details (title, company, location, cost of living, salary, bonus, paternity leave, holidays, gym reimbursement, pet insurance). After saving, the user must be able to record another, stay on the tab, or move on to comparing jobs.

**Pre-conditions**: The application is running and the Job Descriptions & Offers tab is displayed.

**Post-conditions**: If saved, the job is stored and its Offer Received status is set to Yes. If cancelled, no offer is recorded.

**Scenarios**:

- **Normal**: User selects "Add Job Description," fills the Add Job Description Dialog, and saves → user selects that row and chooses "Offer Received" → system shows the Job Offer Received Dialog pre-filled with the job's details → user confirms or edits the values → "Save Offer" → system validates and marks the job's Offer Received status as Yes.
- **Alternate (enter another)**: User adds another job description and the flow repeats.
- **Alternate (compare now)**: After saving, the user switches to the Compare Jobs tab to compare the offer against other jobs (the Compare Job Offers use case).
- **Alternate (cancel)**: User selects "Cancel and Return" before saving → no offer recorded.
- **Exceptional (invalid value)**: User enters an out-of-range value → system rejects it and prompts for correction.

### <u>Use Case 3 — Adjust Comparison Settings</u>

**Requirements**: On the Comparison Settings tab, the user must be able to assign an integer weight from 0 to 9 to each of the six factors — salary, bonus, pet insurance, paternity leave, holidays, and gym reimbursement. Defaults are 1, so if nothing changes all factors are weighed equally. The system displays each factor's normalized weight (its weight divided by the total) alongside the running Total, which always normalizes to 1.0. The user must be able to save or reset.

**Pre-conditions**: The application is running and the Comparison Settings tab is displayed.

**Post-conditions**: If saved, the weights are updated and used by scoring. If reset, the weights return to their defaults.

**Scenarios**:

- **Normal**: User opens the Comparison Settings tab → system shows the six weights with their current values and normalized fractions → user changes one or more → the Total and normalized fractions update → "Save" → system stores the updated weights.
- **Alternate (reset)**: User selects "Reset" → weights return to the defaults of 1.
- **Exceptional (out of range)**: User enters a weight outside 0–9 → system rejects it and prompts for a valid value.

### <u>Use Case 4 — Compare Job Offers</u>

**Requirements**: On the Compare Jobs tab, the user must be able to view all jobs (offers plus the current job, if present) listed by title, company, and location with their computed Score and Current Job indicator, ranked best to worst; select two jobs; view a side-by-side comparison of their factors and scores; optionally accept one of the two offers; and then compare another pair or leave the tab.

**Pre-conditions**: The application is running and the compare option is enabled — at least two jobs exist, or at least one job offer exists alongside the current job.

**Post-conditions**: A comparison of the two selected jobs has been displayed. No job, offer, or settings data is modified unless the user accepts an offer and saves, in which case the accepted job is recorded.

**Scenarios**:

- **Normal**: User selects the Compare Jobs tab → system ranks all jobs by score (see Rank Job Offers) and displays the ranked table with Score and the current job marked → user selects two jobs → system displays the side-by-side comparison of their factors and scores → user chooses to compare another pair or leave the tab.
- **Alternate (accept offer)**: From the side-by-side view the user selects "Accept Offer Job 1" or "Accept Offer Job 2" and selects "Save Acceptance" → system records the accepted offer.
- **Alternate (compare another)**: User returns to the ranked list and selects a new pair.
- **Alternate (cancel)**: User selects "Cancel and Return" → no acceptance is recorded.
- **Exceptional (not enough jobs)**: If the enabling condition is not met, the compare option is disabled until enough jobs have been entered.

### <u>Use Case 5 — Rank Job Offers</u>

**Requirements**: The system must compute a score for every job (offers and the current job) and order them from best to worst so the ranked list can be displayed on the Compare Jobs tab.

**Pre-conditions**: Invoked as part of Compare Job Offers; at least one job exists to rank.

**Post-conditions**: An ordered list of jobs, sorted by score from best to worst, is available for display. No stored data is modified.

**Scenarios**:

- **Normal**: For each job, the system computes its score using the current comparison settings (the six normalized weights and the cost-of-living adjustment) → the system sorts the jobs in descending order of score.
