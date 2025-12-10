# H3 Podcast Sponsor Retention & Brand Safety Analysis

**Author:** Fabian Rosales
**Date:** December 2025

A Python-based analysis proving that 'High Conflict' episodes retain stable audience engagement, identifying untapped revenue opportunities for risk-tolerant sponsors.

## 1. Project Charter

### Business Definition
* **Problem:** The H3 P```odcast faces potential revenue loss from risk-averse sponsors who fear "brand safety" issues with controversial content.
* **Goal:** Prove statistically that "edgy" content does not negatively impact sponsor performance, thereby reducing sponsor churn.
* **Leverage Point:** Churn Reduction & Retention.

### Success Metrics
* **Sponsor Recurrence Rate:** Frequency of brand reappearance.
* **Sentiment Stability Score:** Variance in Like/View ratio between "Safe" vs. "Drama" episodes.
* **Deliverable:** A "Sponsor Compatibility Matrix" identifying which brands survive best in high-variance content.

---

## 2. Data Source & Citation

**Citation:**
Fabian Rosales. (2025). *H3 Podcast Video Performance & Sponsor Metadata (2023-2025)* [Data set]. Derived from YouTube Data API v3. Generated December 4, 2025.

## 3. Data Dictionary
| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `video_id` | String | Unique YouTube ID for the video (e.g., "dQw4w9WgXcQ"). |
| `title` | String | Official video title. |
| `published_at` | DateTime | Date and time of upload (ISO 8601 format). |
| `view_count` | Integer | Total views at time of extraction. |
| `like_count` | Integer | Total likes (proxy for positive sentiment). |
| `comment_count` | Integer | Total comments (proxy for engagement intensity). |
| `description` | String | Full video description text containing sponsor URLs. |

## 4. Feature Engineering & Methodology

### Feature 1: Content Risk Tiers (`category`)
* **Definition:** A rule-based classification sorting episodes into 4 tiers:
    * *Tier 1 (High Conflict):* Lawsuits, allegations, politics.
    * *Tier 2 (Standard):* Internet news, reaction content.
    * *Tier 3 (Safe/Comedy):* Sketches, challenges, wholesome segments.
    * *Tier 4 (Guest):* Interviews.
* **Why it was created:** To simulate real-world "GARM" (Global Alliance for Responsible Media) brand safety standards.
* **Business Value:** Allows the sales team to segment ad inventory. We can prove that "High Conflict" inventory performs differently than "Standard" inventory, rather than treating the whole show as "Unsafe."

### Feature 2: Sentiment Proxy (`like_ratio`)
* **Definition:** Calculated as `Likes / Views`.
* **Why it was created:** Raw "View Counts" are misleading because they include "hate-watching" (negative attention).
* **Business Value:** Proves **Audience Affinity**. A high like ratio during a controversy proves the audience is *supportive*, not repelled, meaning the sponsor's ad is safe from association with negativity.

### Feature 3: Sponsor Resilience Flag (`sponsors`)
* **Definition:** Extracted brand names via Regex from video descriptions.
* **Why it was created:** To identify which specific companies spend money during "Tier 1" episodes.
* **Business Value:** Creates a **Lead Generation List**. Brands that appear in Tier 1 are "Resilient" and should be pitched premium packages during drama arcs. Brands that disappear are "Risk Averse" and should only be pitched comedy specials.

### Algorithm Choice: Rule-Based Classification (Regex)
* **Why we chose this over Machine Learning:**
    1.  **Sample Size Constraint:** With only ~200 data points (N=200), complex ML models (like Random Forest or Naive Bayes) would prone to overfitting and noise.
    2.  **Domain Specificity:** The "H3 Podcast" vernacular is highly specific (e.g., "Goblin Mode," "Steamies"). A pre-trained NLP model (like BERT) would fail to catch these community-specific nuances without extensive fine-tuning.
    3.  **Explainability:** For a "Brand Safety" tool, the sales team must be able to explain *exactly* why an episode was flagged as "High Conflict." A white-box Regex approach provides 100% interpretability, whereas a "black box" neural network does not.

### Logic Refinement & Optimization
* **Iteration 1 (Simple Matching):** Initial keywords relied on simple string inclusion.
    * *Outcome:* High false positive rate (e.g., "Class" triggered "Assault" tier).
    * *Fix:* Implemented Regex Word Boundaries (`\b`) to ensure exact word matching.
* **Iteration 2 (Priority Tuning):** Originally prioritized "Guest" tags first.
    * *Outcome:* Episodes like "Interview with Scammer" were misclassified as "Guest" (Safe) rather than "High Conflict."
    * *Fix:* Reordered logic pipeline to prioritize "High Conflict" keywords *before* "Guest" identification.
* **Iteration 3 (Context Correction):** "The Steamies" was originally flagged as "Comedy/Safe."
    * *Outcome:* Manual review revealed the content focused on analyzing lawsuits/allegations.
    * *Fix:* Moved specific recurring formats to "Tier 1" based on topic toxicity rather than show format.

## 5. Key Findings & Insights

### Insight 1: The "Safety Paradox"
Contrary to traditional marketing logic, "Brand Safe" episodes (Comedy/Sketches) actually drive the **lowest** audience sentiment.
* **Tier 3 (Safe):** 2.6% Like-to-View Ratio (Lowest).
* **Tier 1 (High Conflict):** Tier 1 (High Conflict) maintained a 2.7% Like-to-View Ratio, performing on par with the channel baseline (Tier 2) and outperforming the 'Safe' control group.
* **Conclusion:** The audience does not punish "Drama" content; they expect it. Pivoting to "Safe" content to appease sponsors would likely *hurt* engagement metrics.

### Insight 2: Sponsor Resilience
Analysis of the top sponsors reveals distinct risk profiles:
* **Resilient:** *GamerSupps* frequently sponsors "High Conflict" episodes, suggesting high conversion despite the "edgy" content.
* **Risk-Averse:** *Stamps.com* primarily targets "Standard" episodes, avoiding the "Lawsuit" arcs.
* **Strategic Recommendation:** Pitch "High Conflict" inventory to *Gamersupps-like* brands (Gaming/Lifestyle) rather than Corporate/Utility brands.

## 6. Project Conclusion
This analysis proves that the "Brand Safety" risk for the H3 Podcast is overstated. Engagement remains stable during high-conflict arcs. The optimal strategy is not to sanitize content, but to **segment sponsors**:
* **Tier 1 Inventory:** Sell to Resilience-proven brands (Teddy Fresh, GamerSupps).
* **Tier 2 Inventory:** Sell to Risk-averse brands (Stamps.com, HelloFresh).

## 7. Limitations & Bias Disclosure
* **Recency Bias:** This analysis focuses on the last 200 episodes (~2024-2025). Trends may differ from the show's earlier formats (2018-2023).
* **Keyword Dependency:** The "Risk Tiers" rely on specific keywords (e.g., "Lawsuit"). Episodes that discuss conflict without using these specific terms in the title may be misclassified as "Standard."
* **Sponsor Attribution:** Methodology assumes that if a sponsor link exists in the description, they paid for that specific episode. It does not account for potential "make-good" ads or unpaid affiliate links.

### Validation of Targeting Logic
* **Metric Choice:** Since this is a rule-based inference project (not a predictive ML model), standard metrics like F1-Score or ROC-AUC are not applicable.
* **Targeting Precision:** Instead, we prioritized **Tier Purity** to ensure accurate marketing targeting.
    * *Validation Method:* Automated "Sanity Check" scanning Tier 3 (Safe) episodes for high-conflict keywords.
    * *Result:* 0% False Positive Rate detected in the "Safe" Tier.
    * *Business Implication:* The "Safe" inventory bucket is statistically clean and safe to pitch to conservative sponsors.
    * *Fallback Protocol:* If the Sanity Check fails (detects >0% error), the pipeline is designed to **halt execution** immediately. This prevents corrupted data from reaching the dashboard, forcing a manual review of the new keywords.