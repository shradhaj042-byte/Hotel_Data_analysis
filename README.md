# 🏨 Hotel Bookings Analysis: Why Do Guests Cancel, and What Does It Cost?

**Tools:** Python, pandas, NumPy, Matplotlib, Seaborn, Plotly, SciPy
**Dataset:** `hotel_bookings_2.csv` (Resort Hotel bookings, about 7,300 rows, 32 columns)

---
## 1. Business Problem 
The hotel loses money when guests **cancel**. A cancelled room may stay empty, and the hotel has already
planned staff, food and cleaning for that guest.

**Main question:** *Which bookings are most likely to be cancelled, how much money does this cost us, and what can we do to reduce it?*

## 2. Assumptions
1. Each row is one booking. Rows that are exact copies of another row are duplicates and are removed.
2. `is_canceled = 1` means the guest cancelled (or never showed up). `0` means the guest stayed.
3. `adr` is the *Average Daily Rate*, the price per night. **Booking value = adr × total nights.**
4. A cancelled room is assumed to stay empty (not re-sold). So **lost revenue = the value of cancelled bookings**.
5. Bookings with 0 guests or 0 nights are data errors and are removed.
6. `reservation_status` says "Canceled" for cancelled bookings. That is the answer itself, so we do **not** use it to explain cancellations.
7. Currency is not given in the data, so we just say "revenue".

## 3. Research Questions and Hypotheses
| # | Question | Hypothesis (our guess before looking) |
|---|----------|----------------------------------------|
| Q1 | How big is the cancellation problem? | More than 1 in 3 bookings are cancelled. |
| Q2 | Does season matter? | Cancellations and prices are higher in summer. |
| Q3 | Does lead time (days booked in advance) matter? | The earlier the booking, the more likely to cancel. |
| Q4 | Does the deposit type matter? | Non-refundable deposits reduce cancellations. |
| Q5 | Which market segments, channels or agents cancel most? | Online travel agencies cancel more than direct bookings. |
| Q6 | Which guests cancel? (country, kids, special requests) | Guests who make special requests or changes are more committed. |
| Q7 | How much revenue is lost? | Lost revenue is large compared with kept revenue. |

## 4.How to run
1. Put `hotel_bookings_2.csv` in the same folder as `hotel_bookings_analysis.ipynb`.
2. Install the libraries: `pip install pandas numpy matplotlib seaborn plotly scipy jupyter`
3. Run `jupyter notebook`, open `hotel_bookings_analysis.ipynb` and choose **Kernel > Restart & Run All**.
4. Charts are saved automatically into the `charts/` folder. The Plotly charts are interactive inside Jupyter.

## 5. Project steps
1. Business problem, assumptions, questions and hypotheses
2. Data cleaning (missing values, duplicates, invalid rows, dates, outliers, new columns)
3. Analysis with charts for 7 research questions, plus statistical tests (chi-square, Mann-Whitney)
4. Findings, recommendations and limitations

### 6. Cleaning summary
| Step | What we did |
|---|---|
| Missing values | Dropped `company`, filled `agent` with 0 and `country` with "Unknown" |
| Duplicates | Removed exact duplicate rows |
| Constant columns | Dropped columns with only one value |
| Invalid rows | Removed 0 guests, 0 nights and more than 10 adults |
| Data types | Ordered months, converted dates (1 cut-off status date set to empty) |
| Outliers | Checked, kept (real values) |
| New columns | `booking_value`, `revenue`, `lost_revenue`, `lead_time_group`, `season`, `has_kids`, `room_changed`, `has_parking` |


## 7. Key Findings
1. **Half of bookings (50%) are cancelled**, and cancelled bookings are worth about **1.46M vs 1.24M kept**, so more than half of potential revenue is lost.
2. **Online Travel Agencies** are the biggest risk: 63% cancellation and over half of all bookings. **Agent #240** alone is about 2,500 bookings at 66%.
3. **Portuguese guests** make up about 60% of bookings and cancel **74%**. Guests from most other countries cancel under 11%.
4. **Season:** prices peak in July and August, so cancellations then cost the most. Cancellation *rates* are highest in the quiet months (January to April).
5. **Commitment signals** (special requests and booking changes) go with far fewer cancellations. Parking shows 0% cancellations, but this is probably recorded only after arrival, so we do not rely on it.
6. **Lead time** has a small effect and is *not* a straight line, so "book early = cancel more" is too simple.
7. **Deposit type** gave a surprising result (non-refundable = 86% cancelled), which most likely comes from a data or policy quirk and needs checking with the hotel.

## 9. Recommendations
| Priority | Action | Why (from the data) |
|---|---|---|
| High | Add a **cancellation policy or small deposit** for Online TA bookings and review the contract with Agent #240 | Biggest segment with 63% cancellation |
| High | Use **controlled overbooking** in high-risk periods (Jan to Apr, Online TA, Portuguese guests) | Cancellation is predictable by segment, country and season |
| Medium | **Reconfirm** bookings before arrival, especially 3 to 6 months ahead for Online TA | Cancellation is highest for 91 to 180 days lead time |
| Medium | Invite guests to **add requests or edit their booking** (small upsells, pre-arrival emails) | These guests cancel less (a link, not proven cause; test it with a small trial) |
| Medium | Protect **July and August** with stricter terms | Every lost booking costs about 3x more per night |
| Check | Ask the hotel how **Non Refund** deposits are recorded before changing deposit rules | The 86% result looks unusual |

## 10. Limitations and Next Steps
- Only **one hotel** (Resort Hotel) and about 2 years of data, and the two years cover different months, so we cannot judge a year-on-year trend.
- Revenue is **estimated** (adr × nights); extras such as meals are not included, and we assume cancelled rooms are not re-sold.
- Several variables (parking, room change) are probably filled in only once a guest arrives, so they are **flags, not proven causes**. The same caution applies to special requests and booking changes.
- **Next step:** build a simple **machine-learning model** (for example, logistic regression or random forest) to predict cancellation risk per booking.
- **Next step:** once you learn SQL, repeat the key questions with `GROUP BY` queries and compare the answers.
## Tech stack

Pandas · NumPy · Matplotlib · Seaborn · GeoPandas · Jupyter Notebook